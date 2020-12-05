---
layout: default
title: Smithproxy updates - Dec/5
date: '2020-12-05 23:59:00 +0200'
categories: updates
---

Hi all,
welcome back at [**smithproxy**](https://www.smithproxy.org "Your favorite project's homepage") irregular development update! Even though a small one this time.

# [Smithproxy](https://www.smithproxy.org "Your favorite project's homepage")
Finally it's here. [Socle](https://github.com/astibal/socle/ "smithproxy foundation socket library")  [io2 branch](https://github.com/astibal/socle/tree/io2/ "branch focused on IO improvements") has been merged recently into the master. What was reasons and goals? See recap below.  Current build `0.9.11` is considered stable enough to call it "a release" and is available now on with new versions to download in all supported packages and variants.

Unfortunately development was delayed by my covid-19 infection and recovery.   

# What's new

* **UDP code totally rewritten** - no "quick" mode ports anymore. "quick_ports" configuration option is now no-op
*  new proxy multiplexer with different and more robust internal connection handover and routing
* **memory pool system totally rewritten** to almost lock-free mechanism
* new `diag worker list` CLI command to diagnose new worker architecture
* new `diag mem udp stats` CLI command to diagnose new UDP flow datagrams (they will become a connection)
* new config `settings/` area bool variables: `accept_tproxy`, `accept_redirect` and `accept_socks` to fully disable respective worker trees and save some idle CPU cycles + startup script support for these.
* reworked **automated build system** - new binary package builds don't require any intervention and are compiled and uploaded to mag0.net/out/smithproxy repository automatically.
* **[Release Notes](https://www.mag0.net/out/smithproxy/0.9/Release_Notes.md)** introduction - each smithproxy version will have its own entry in Release Notes specifying what's new and what to consider when upgrading.
* Package directories also contain SHA2 checksums and detailed changelog generated from git changes
* Documentation updates 


# Summary
Important changes merged into `master` from `io2`, we have better automated build system, and much more. Definitely check [Release Notes](https://www.mag0.net/out/smithproxy/0.9/Release_Notes.md), they will replace most of "what's new" area in further dev updates. I will now focus on CLI improvements, mainly operating policies and objects, so there will be no need anymore to edit `smithproxy.cfg` manually. 

Thanks for reading, till next time!
  Ales, [Smithproxy](https://www.smithproxy.org "Your favorite project's homepage") dev
