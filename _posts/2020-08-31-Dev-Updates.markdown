---
layout: default
title: Smithproxy updates - Aug/31
date: '2020-08-31 23:59:00 +0200'
categories: updates
---

Hi all,
welcome back at [**smithproxy**](https://www.smithproxy.org "Your favorite project's homepage") irregular development update!

# [Smithproxy](https://www.smithproxy.org "Your favorite project's homepage")
There is one interesting change in [socle](https://github.com/astibal/socle/ "smithproxy foundation socket library")  [io2 branch](https://github.com/astibal/socle/tree/io2/ "currently in-development branch focused on IO improvements"): memory management improvements. You might say why memory features are touched in branch focused on I/O? Because better I/O deserves faster memory allocation! :)

## [mempool](https://github.com/astibal/socle/tree/master/common/mempool "home-brew memory management using pools of pre-allocated memory chunks") improvements
In [master branch](https://github.com/astibal/smithproxy/tree/master/) code, memory buffers were separately pre-allocated, added per-size to their lists, assigned and then released back when needed. 
Pool memory allocator is used in speed-critical parts of [socle](https://github.com/astibal/socle/ "smithproxy foundation socket library"), and also hooked into [OpenSSL library API](https://www.openssl.org/docs/man1.1.1/man3/) - whenever crypto took place, it was using these pre-allocated memory chunks.
This worked quite well, it was preventing [ `malloc()`]("https://man7.org/linux/man-pages/man3/malloc.3.html" "standard library memory allocation function") spamming, but ... 

### On design flaws

> **There was a flaw in it**: when pointer was being released, we didn't know the size of allocated memory it points to. 

[Socle](https://github.com/astibal/socle/ "smithproxy foundation socket library") and internal code was changed to specify the buffer capacity on release, ie:
```C++
// create mem_chunk to release my buffer
mem_chunk_t to_release = mem_chunk(my_data_, my_data_capacity_);

// access pool and release it!
memPool::pool().release(to_release);
```
but the rest of the issue remained:  external libraries made to use mempool were not aware of our custom memory management. 
Standard memory releasing functions release pointer only. For example, see the code registering custom mempool allocator via [`CRYPTO_set_mem_functions`](https://www.openssl.org/docs/man1.1.1/man3/CRYPTO_set_mem_functions.html):

`mempool.hpp:`
```C
void* mempool_alloc(size_t);  
void* mempool_realloc(void*, size_t);  
void mempool_free(void*);
```
`main.cpp:`   
```C
#include <openssl/crypto.h>
#include <mempool/mempool.hpp>
// ...
CRYPTO_set_mem_functions(mempool_alloc, mempool_realloc, mempool_free);
``` 
As you see, there is no size parameter in `mempool_free` function.

To quickly *"fix"* this issue, hash-map has been created to maintain such an information: mapping pointer address to the size. Not ideal at all.  This complex structure must be protected by lock. 
Not only it takes time to insert element in the map, but also other threads are waiting  for the current thread to release this lock ... basically making `mempool_release` possible to run only from single thread at a time.

### Fixing the fix: lock-free mempool implementation. Well, almost.

How then we remember chunk size without having extra structure for it? 
We have 2 options:

#### A) having **control block in the allocated chunk** itself, ie:
```
+----------+---------- ---
| 4B: size |<--- actual pointer given to requestor
+----------+--------- ---
```
... by simply allocating some extra few bytes. But there are many issues with this design.
What if the pointer was not requested from our mempool and it was by some program logic or error allocated by other means? Reading bytes outside pointer scope would probably lead to segmentation fault or other under/overruns or bad reads based on previously incorrect buffer size calculation.
Worse - this can't be actually prevented  - there is no way we can know memory boundaries for this pointer, we can only *assume* it was allocated by mempool. 

#### B) winning approach - allocating and **slicing parent memory chunk** to smaller, fixed-size ones, ie:
```
+-----+-----+----  ---                      ---+
| 64B | 64B | ...                          ... |
+-----+-----+----  --                        --+
^---  ^---      ... pointers given to requestor 
```
With this aproach, we immediately know the size of pointer by simple pointer address comparison to the range  of bytes totally allocated. Also, this still makes room for header data.
There are few disadvantages: 
* flexibility: we cannot further request more chunks dynamically on the fly. We can just allocate another big chunk, reasonably big to pay off the extra address comparisons on release. This aproach requires good mempool planning before on start.
* Each unknown-size release implies multiple address comparisons, so it's good idea to not having too many  parent chunks. On the other hand, having too few, there will be too much space wasted on unnecesarily big chunks, ie allocation of 64B resulting of getting 512B. 


### Lock-free?
Not really, but we limited mepool memory locking to  a bare minimum. There is no `pointer->size` map anymore, therefore its heavy locking is gone. This is a *good thing*. 
But we still have a lock on a free chunk list (actually it's a pre-sized [vector](https://en.cppreference.com/w/cpp/container/vector "fast standard library container")). Adding a new element into the pre-sized vector is quite fast, but still there might be a need to remove this lock in the future. We can write special, lock-free [singly-linked list](https://en.wikipedia.org/wiki/Linked_list#Singly_linked_list), but it seems it's an overkill doing so at this time.   

### Bonus: memory corruption detection
There is a new feature to fight (possible) memory corruption issues in [socle](https://github.com/astibal/socle/ "smithproxy foundation socket library"). It's check of specific content *before* the allocated data and *after* the end of them. These zones are pre-allocated with parent chunk and we call them canaries:

```
    8B       64B        8B  
+--------+----   -+--------+--------+----  ---                      ---+
|CaNaRyCa| 64B    |CaNaRyCa|CaNaRyCa| 64B                          ... |
+--------+----   -+--------+--------+----  --                        --+
         ^---                       ^---      ... pointers given to requestor 
```
There is *front*, and *rear* canary. Both are checked when the memory is being released back to pool. If they didn't match predefined content, we know something must have overwritten them and we [throw an error](https://en.wikipedia.org/w/index.php?title=Throw_command&redirect=no "... making the program to handle some exceptional condition").  
This adds of course some extra CPU cycles, the penalty is noticeable (on idle in few percents on modern processors).   But it protects program to underrun or overrun buffer, assuring stronger confidence on how data are handled by [socle](https://github.com/astibal/socle/ "smithproxy foundation socket library"). 
Feature is currently enabled only in debug mode, if `MEMPOOL_DEBUG` is defined in the source code.

# Conclusion
Even though holidays were good time to relax, it was quite nice to have some spare time to think and materialize ideas into code. 

Thanks for reading, till next time!  
  Ales, [Smithproxy](https://www.smithproxy.org "Your favorite project's homepage") dev
