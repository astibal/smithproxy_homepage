---
layout: default
title: Smithproxy updates - Aug/17
date: '2020-08-17 23:00:00 +0200'
categories: updates
---


Hi all,  
holidays are slowly reaching the inevitable end and I am returning to my updates for you. There are not many news to share, due to my holiday out of the reach, on really bad internet and too many distractions around. Luckily ;-)

## Smithproxy - io2 branch

The most of work has been done in **io2** branches of both, [smithproxy](https://github.com/astibal/smithproxy/tree/io2)  and [socle](https://github.com/astibal/socle/tree/io2).

#### N:M workers
A new *N:M* worker architecture has been introduced, where one can have *N* acceptors competing for the same listening tproxy socket (ie. where all passing connections to tcp/443 are diverted), and *M* workers, all independently spawn to be notified by any from acceptors (which process and actually proxy the connection).  This should improve responsiveness and general performance while processing a new connections.
Also acceptor-worker notification mechanism has been changed to shared, full mesh socket-pair design - all acceptors can talk to all workers via their exclusive socket-pair.

#### UDP code rewrite
To be honest, UDP code in *master* branch was a mess. Originally implemented purely with DNS in mind, code was not really suited for generic UDP.  Not speaking of modern, UDP-based encrypted protocols or performance. There was an attempt in master branch to improve it, distinguishing *quick* vs. new *cooked* mode. Cooked mode however never reached production quality, leaving **smithproxy** relying on quick mode.  

How it was supposed to work? Basically when receiving arbitrary UDP datagram one can can't *accept()* it as it could be done in case of TCP.  First datagram mus be still processed for even though there is no connection yet. That's because data and datagram already landed in TPROXY listening socket, which can't be undone and moved to another socket. Both modes were trying to cope with that situation slightly different way.  

In **quick mode**, socket to the originator is not created at all. All incoming UDP datagrams were handled by acceptor mechanism and queued without any further checks. In **cooked mode** very first  datagram is queued too, but a new socket with originator is created - this socket represents *left* connection between originator and proxy, where *right* socket has yet to be created and if originator sends another datagram, it already lands there.

New code in **io2** is generic and this time hopefully correct implementation of *cooked* mode. Therefore, after io2 is merged in, **cooked and quick mode configuration options are noop** in favor of a new, unified approach. This new cooked mode successor is good compatibility candidate for further implementation of QUIC, HTTP3 and DTLS.

####  libcli troubles - removal 
Don't get me wrong. There still **is** *libcli*, but is compiled-in as git submodule. Why not using system distribution packages?   
Using the new *N:M* architecture, we are creating many socketpairs between acceptors and workers. With "many" I mean possibly hundreds, depending on number of your CPU cores and settings. And *libcli* still uses ... ehm ... *select()* for socket polling. This couldn't worked even for some busy situations in current master branch, however I've never met such conditions.  

I had basically 2 options: either to submit PR with *epoll()* implementation and hope to be soon distributed to all mainstream distros (and then make all dependency scripts explicitly check for corrected version), or to fork libcli and use my own, with all cons it might have.  

I decided to go for second option. I forked libcli and immediately removed all ~~crap~~ unnecessary files, changed source to be possible to compile with modern C++ compiler (no old-style type conversions from \*void) and added linux detection macro directive check to enable *epoll_\*()* calls.  
I am even considering to convert whole libcli (it's not that many lines of code) into modern C++ header only lib. Over the time, if circumstances allow.

## Smithproxy master branch - small but important bugfix
Last, but not least, I have fixed memory pool exhaustion issue, which slowly made **smithproxy** to leak all its 64B memory pool resources. If you are snap user, upgrade to 0.9.6-3 patch.  
Fix is so important that it deserves a new, proper release version. 0.9.7 is going to be released in upcoming days for this purpose.  

## Conslusion
There is still a lot of work and polishing to be done in **io2** branch, but at least the results so far seem to be promising. Important fixes are being cherry-picked from io2 to master, which continues to be main 0.9.x source.
With io2 merge I am planning to jump with numbering to 0.9.20 (or more on the last version tag).
And that's it!

Thanks a lot for your support,
Ales

P.S.: sorry, no pplay news, hopefully next time!
