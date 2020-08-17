---
layout: default
title: Smithproxy and Pplay updates - July/1
date: '2020-07-01 08:00:00 +0200'
categories: updates
---


Hi all, 
let me share with you some updates on tools I am writing with love and care for you (and me).

## Smithproxy

#### Core components

To not so big surprise, snap release revealed few niche (or not that niche) issues. 
Namely:  
 - capture path strange permissions due to decimal and not octal permission value in the sources
 - IPv6 capture and redirection issues
 - missing IPv6 default policies (creating and applying `any6` address objects)
 
 Some more fixes in improvements:
 - fixing bug swapping ip src and dst under some circumstances in traffic capture
 - threading and mutex locking improvements
 - SNI bypass fix - this was plain broken
 - fixing shamefully crashing crash handler (yeah)

#### Snap(craft)
Few honest words about that: it was pain. Creating confined snap for such an application like [smithproxy](https://www.smithproxy.org) was not by far that easy how I (and you) would think. 
> Please use always latest snap possible, this is moving battleground, currently.

There was quite strong reasoning why I really couldn't have used so popular [AppImage](https://appimage.org/).  First of all, I need data left behind on the system. This is not possible with appimage, unless you have world-writable access to everything. It maybe gives a freedom to pack all dependencies together, but confined/sandboxed, loop-mounted snap provides much better system vs. app protection and isolation.

Despite [snapcraft snap system](https://snapcraft.io/) bounds to [Canonical](https://canonical.com/), I really like it, at the end.  Also I like to see getting new users, because snapcraft has its own (and simple) metrics. This really help, open-source world is cruel and unless you receive tons of issue/bug reports, you as the author don't know anything your programs users.

#### Docker
There is something with our dockerfiles. I was checking something and decided to redo this part. Current system was made the way we are building `*-base` images, and on top of them we were building the rest, like `*-run` images. This worked well until I released building scripts in `/tools`. 
Building scripts build all dependencies, but it's unpredictable how it will go with docker caching. So I decided to abandon using `base` images and everything will be built fresh and clean. 
This is currently in progress and will come in live in next few days.
Also, `legacy` tag has been removed. Nobody should be running [smithproxy](https://www.smithproxy.org) older than `0.9`.  Really.


#### Documentation
Not much news in this area. 
New tutorial video:
[How to test IPv6](https://youtu.be/vQfz0nfnHgA)


## Pplay news
There was a lot things done in [pplay](https://pypi.org/project/pplay/) over last 2 weeks. It got a bit more acceptance recently and new bugs have been discovered and reported. I didn't want to throw away the opportunity and tried to fix all of them soonest possible.
Apart of fixes, there are also few important new features!

- `--gencap` packet source. No, there is no new capture format around :) It's a procedurally generated stream of random client-server exchanges. To make it work, one needs pseudo-random source which gives exactly the same stream of bytes. Since there was nothing convenient enough for me in the library,  I wrote simple one myself.  `gencap` must be provided with its *magic seed*.
- `--fuzz` - taint your data with random changes! Have fun by observing what breaks and where. Excellent to test signature engines and fuzzing first packets sent on real targets (and expecting something else you have in capture, of course). `fuzz` takes numerical argument between 0 and 255, which is ceiling from where data are tainted. So lower the value is, more bytes are to be tainted.  Fuzz is deterministic. Fuzzing with same values from `--server` and `--client` produces exactly same stream. You can also specify `--fuzz-magic` to *seed* it.
- `--scatter` will split originally one chunk of data  into multiple parts and tries to send them separately, with a small, but noticeable delay. 
 


Thanks a lot for your support,
Ales
 
