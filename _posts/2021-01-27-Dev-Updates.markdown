---
layout: default
title: Smithproxy updates - Jan/27
date: '2021-01-27 18:00:00 +0100'
categories: updates
---

Hi all,
welcome back at [**smithproxy**](https://www.smithproxy.org "Your favorite project's homepage") irregular development update!

# [Smithproxy](https://www.smithproxy.org "Your favorite project's homepage")

## What's new

### 0.9.12 release is out!
This is great news. 0.9.12 release was focused on CLI. Unless one needs to edit signatures, it's all doable in CLI. Cli is smart enough to not let you delete used component or do a typo when adding an address into policy match.

Let's see and summarize what was added (and is properly documented in [Release Notes](https://download.smithproxy.org/0.9/Release_Notes.md)):
* CLI `add` command to add element into configuration
* CLI `remove` elements - checks for references and don't allow removal if it's used
* CLI `move` policies - move `before`, `after`, `top`, `bottom`
* CLI `edit` element - change context to the object and edit its contents
* CLI `set` command with value validity check - it reject any obvious nonsense for the option, or non-existing object name

Logic at the background is not really straightforward, but the API was made the way it's easy to describe configuration with ease:

##### Code snippet  for CLI values initialization

Maybe you remember groovy-style API introduced in previous update:

```c++
void CliHelp::init() {
    //... statically located object example with string value:
    add("settings.certs_path", "directory for TLS-resigning CA certificate and key")  
        .help_quick("<string>: (default: /etc/smithproxy/certs/default)")  
        .may_be_empty(false)  
        .value_filter(CliElement::VALUE_DIR);    // lambda check if directory exists

    // ... dynamic object example with fixed set of valid values:
    add("policy.[x].proto", "protocol to match (see proto_objects)")  
        .may_be_empty(false)  
        .value_filter(is_in_vector([]() { return CfgFactory::get().keys_of_db_proto(); },
				        "must be in proto_objects"))  
       .suggestion_generator([](std::string const& section, std::string const& variable) {  
           return CfgFactory::get().keys_of_db_proto();  });    
```

##### Code snippet for CLI section initialization
```c++
// code registers callbacks for dynamic item: 'x' is a wildcard for arbitrary port_object name
register_callback("port_objects.[x]", MODE_EDIT_PORT_OBJECTS)  
    .cmd_set(cli_generic_set_cb)  
    .cap_edit(true).cmd_edit(cli_conf_edit_port_objects)  
    .cap_add(true).cmd_add(cli_generic_add_cb)  
    .cap_remove(true).cmd_remove(cli_generic_remove_cb);
```

Even though implementation had hit some hurdles on they way with `libcli` (now vendored with few fixes directly into smithproxy), the effort was worth it. Current CLI design has still some room for improvements, but is flexible enough and allows further project growth and new capabilities. 

## What's next?
Let's check [Release Notes](https://download.smithproxy.org/0.9/Release_Notes.md) for  our ~~wet dreams~~ roadmap:
* [ ] better certificate cache - certificates from cache expire on LRU-similar basis
* [ ] memory profiles - more flexible mempool with, or optionally without fallback to heap for constrained systems
* [ ] *new installations affecting change* split portal services and core   
* [ ] json/api interface

Let's take a look at each of them now.

#### Portal split - 0.9.13 (mid of February)
This is something which must have to come at some point. Smithproxy is multi-component project, no single binary that does it all. There is `smithdog` python process tracking if `smithproxy` or `smithd` is running, eventually restarting it. There are http/https portals (each runs in its own process) for user logins. There is `bend` process, managing logon information collected from portals.
It started to be difficult to maintain all in single bundle; portals require quite few python libraries, while the smithproxy core just few, most common ones. Dependencies are thanks to authentication components quite heavy, but what if the user wants only TLS decryption features?

It will be easier to split smithproxy project to `smithproxy-core` package (likely keeping original name - just `smithproxy`) and another package `smithproxy-auth` managing authentication features: portals and bend component.
This requires quite some refactoring and detection mechanics on `-core` side to (attempt) launch portals only if present, etc. Not extremely difficult task, but require patience and *lot* of testing.

> **Portal split affects new installations** - all new builds of `smithproxy` > 0.9.12-2 (official 0.9.12 release) will not contain any authentication features (auth portals and backend), just the core. `smithproxy-auth` must be downloaded and installed separately.
> There isn't however any other extra configuration needed.
> Already existing installations are not affected. Portal-related files only won't be installed and updated.

This is actually currently what I am working on. At the time of writing this update, `smithproxy-core` is separated from portal components already. Portals were extracted and new package is about to be created in upcoming days or weeks.
All this is happening in separate `portal-split` branch, `master` code branch is not *yet* affected by this change.  That means this milestone will make it first into the streamline versions.

#### Better certificate cache - 0.9.14 (Feb/March)

This is legacy code rejuvenation. Smithproxy wasn't meant to be run long-term; for that purpose there was no need to do any smart caching and releasing. The same applies to abbreviated handshake tickets and various keying material stored associated with it.
As a result, certificate cache (let me carefully whisper it) elements don't expire at all. It's a bit shameful, since otherwise there are capabilities in the code to expire things, at least on FIFO basis. But this is not the right thing to do. We need cache, which takes into account if it's frequently used element, or very fresh. Plan is to introduce in the code smart LRU cache, which will emphasize access count and last access time. 
The cache mechanism should be flexible enough to change the mechanism if needed. 

#### Memory profiles - 0.9.15 (March/Apr)

In previous versions it turned out it's quite usable redirect all allocations from the program to our `mempool` mechanism.  The long-term goal is aims there, but we are not yet there. We have things to solve before doing so:
* even better locking mechanism for fixed size pools (ie. per-pool lock)
* scalable pools - determine how much of memory we can (or is configured) to take
* optionally allowing heap allocation (and possibly oversubscribing)

All above is not very easy to accomplish. But it it has strong advantages: crystal-clear memory tracing. Smithproxy is quite difficult to troubleshoot, at least in memory leaking area. Valgrind is too slow on so many threads handling real-time connections. Compiler sanitizers do much better job, but both approaches are not perfect: there are few caches which may hold the pointer forever, something can be left behind in OpenSSL part, which is not possible to detect as a leak (there is still some reference somewhere).

#### JSON/API interface - 0.9.16 (Apr)
Smithproxy deserves some automatization or GUI in the future. Version will introduce tech-preview *basic, read-only* access to live  elements, like sessions, configuration, and so on. Depth of the implementation depends on the overall schedule. 
This milestone is not certainly counting with any sort of frontend, though. Also note GUI made at some point will be almost certainly different `smithproxy-gui` package. 

# Summary
Libcli is a great lib, but it really slowed down things. It's not really well documented, code examples are just scratching the surface and more complicated constructs must be researched without much of information anywhere.  Besides, smithproxy is squeezing every last of features it offers.  
And that's it - hitting corner-cases usually means hitting bugs. No software is perfect. 
Anyway - this is mostly behind us and there is a lot of work ahead. I am really looking forward to make the plan above the reality. Hopefully it will be more fun than libcli hacking.  Stay tuned. 

Thanks for reading, till next time!
  Ales, [Smithproxy](https://www.smithproxy.org "Your favorite project's homepage") dev
