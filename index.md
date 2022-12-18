---
layout: default
title: Smithproxy homepage
---

![](img/badger-love.png) [![become a patron](https://img.shields.io/badge/donate-patreon-FF338B)](https://www.patreon.com/bePatron?u=23520766)[![donate using liberapay](https://img.shields.io/badge/donate-liberapay-FFBE0B)](https://liberapay.com/astib/donate)  ![](img/badger-status.png) [![travis](https://travis-ci.org/astibal/smithproxy.svg?branch=master)](https://travis-ci.org/astibal/smithproxy)
  [![docs](https://readthedocs.org/projects/smithproxy/badge/?version=latest&style=flat)](https://smithproxy.readthedocs.org)
  [![Docker Cloud Build Status](https://img.shields.io/docker/cloud/build/astibal/smithproxy)](https://hub.docker.com/repository/docker/astibal/smithproxy)[![Snapstore](https://img.shields.io/badge/snap_build-passing-passing)](https://snapcraft.io/smithproxy)  
![](img/badger-versions.png) [![GitHub tag (latest by date)](https://img.shields.io/github/v/tag/astibal/smithproxy)](https://github.com/astibal/smithproxy/releases)[![Core commit activity](https://img.shields.io/github/commit-activity/m/astibal/smithproxy)](https://github.com/astibal/smithproxy/)[![Lib commit activity](https://img.shields.io/github/commit-activity/m/astibal/socle)](https://github.com/astibal/socle/)[![Docs commit activity](https://img.shields.io/github/commit-activity/m/astibal/smithproxy_docs)](https://github.com/astibal/smithproxy_docs/)   

[![Language grade: C/C++](https://img.shields.io/lgtm/grade/cpp/g/astibal/smithproxy.svg?logo=lgtm&logoWidth=18)](https://lgtm.com/projects/g/astibal/smithproxy/context:cpp)

**Smithproxy** is highly configurable, fast and transparent TCP/UDP/TLS (SSL) proxy written in C++17.  
It uses our C++17 socket proxying library called [*socle*](https://github.com/astibal/socle).
> Read fresh [**Release Notes**](https://download.smithproxy.org/0.9/Release_Notes.md) to stay tuned!  

> To replay traffic captures, or script custom network activity - sister project [pplay](https://pypi.org/project/pplay/).

## Availability:
  * **Linux** - can be installed as a service (distro packages, or easily compiled from sources)  
    * Download  binary linux .deb (*arm64*, *armhf*, *amd64*) packages and source from: [https://download.smithproxy.org/](https://download.smithproxy.org/)  
    * Download and compile directly from source (known to work: Debian, Ubuntu, Alpine, Fedora, Kali, Arch)  
  * **Docker** - available as an image on docker hub  
    * See our docker hub page: [https://hub.docker.com/r/astibal/smithproxy](https://hub.docker.com/r/astibal/smithproxy)
    * ![](https://img.shields.io/docker/pulls/astibal/smithproxy)
  * **Snap** - install smithproxy service as a confined snap  
    * Visit snap store here: [https://snapcraft.io/smithproxy](https://snapcraft.io/smithproxy)  

## Core features:
  * TCP/UDP and TLS - intercept **routed** traffic, **locally-originated** traffic and **SOCKS** proxy requests
  * configure policy based traffic matching similar to modern firewalls
  * utilize per-policy applicable *content*, *dns*, *tls*, *detection* and *authentication* profiles
  * re-route traffic (DNAT) and load-balance it, stickiness based on source-IP, L3 or L4 header data
  * insightful CLI with configuration control
  * HTTP API to control configuration options ( -- new in `0.9.31`)
  * export intercepted traffic to rotated pcap files, or emitting it to remote workstation in GRE
  * limited UDP SOCKS5 support ( -- new in `0.9.31`)


## TLS features:
  * TLS security checks (OCSP, OCSP stapling, automatic CRL download)
  * Certificate Transparency checks for outbound connections
  * HTML replacement browser warnings
  * STARTTLS support for most starttls capable protocols, including HTTP proxy CONNECT
  * Seamless HTTPS redirection to authentication portal (if installed)
  * Exporting sslkeylog
  * KTLS support with OpenSSL 3.x.x. ( -- new in `0.9.31`)

## Other:
  * Local and LDAP user authentication using builtin web portal (using complementary package)
  * SOCKS4/SOCKS5 explicit proxy with DNS hostname support
  * Engines: limited HTTP1 and HTTP2 support
  * DNS inspection allows FQDN policy objects, including DoH
  * Policies based on FQDN and 2nd level DNS domain
  * both IPv4 and IPv6 are supported
  * detailed debugging messages in CLI if needed

## Tools:
  * built-in tools to help with CA and certificate enrollment needed to run smithproxy
  * auto-enrolling portal certificate based on system IP and hostname
  * auto-detect inspection interface(s) based on system routing information
  * check [pplay tool](https://pypi.org/project/pplay/): replays captures
    over the network with many cool features  

# Support and contacts
  * Discord server: [https://discord.gg/vf4Qwwt](https://discord.gg/vf4Qwwt)  
  * email support: `<support@smithproxy.org>`  
  * Documentation: [https://smithproxy.readthedocs.org](https://smithproxy.readthedocs.org)  
