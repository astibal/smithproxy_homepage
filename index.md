---
layout: default
title: Smithproxy homepage
---

![](img/badger-love.png) [![become a patron](https://img.shields.io/badge/donate-patreon-FF338B)](https://www.patreon.com/bePatron?u=23520766)[![donate using liberapay](https://img.shields.io/badge/donate-liberapay-FFBE0B)](https://liberapay.com/astib/donate)  ![](img/badger-status.png) [![travis](https://travis-ci.org/astibal/smithproxy.svg?branch=master)](https://travis-ci.org/astibal/smithproxy)
  [![docs](https://readthedocs.org/projects/smithproxy/badge/?version=latest&style=flat)](https://smithproxy.readthedocs.org)
  [![Docker Cloud Build Status](https://img.shields.io/docker/cloud/build/astibal/smithproxy)](https://hub.docker.com/repository/docker/astibal/smithproxy)[![Snapstore](https://img.shields.io/badge/snap_build-passing-passing)](https://snapcraft.io/smithproxy)  
![](img/badger-versions.png) [![GitHub tag (latest by date)](https://img.shields.io/github/v/tag/astibal/smithproxy)](https://github.com/astibal/smithproxy/releases)[![Core commit activity](https://img.shields.io/github/commit-activity/m/astibal/smithproxy)](https://github.com/astibal/smithproxy/)[![Lib commit activity](https://img.shields.io/github/commit-activity/m/astibal/socle)](https://github.com/astibal/socle/)[![Docs commit activity](https://img.shields.io/github/commit-activity/m/astibal/smithproxy_docs)](https://github.com/astibal/smithproxy_docs/)   



**Smithproxy** is highly configurable, fast and transparent TCP/UDP/TLS (SSL) proxy written in C++17.  
It uses our C++17 socket proxying library called [*socle*](https://github.com/astibal/socle). Yes, we love C++!

> Check [documentation](https://smithproxy.readthedocs.org) for information about scenarios and how
it can installed, compiled or deployed using docker or snap.

## Availability:
  * **Linux** - can be installed as a service (distro packages, or easily compiled from sources)
  * **Docker** - available as an image on docker hub
  * **Snap** - you can also install it as a confined snap!

## Core features:
  * intercept **routed** traffic, **locally-originated** traffic and **SOCKS** requests
  * Policy based traffic matching similar to modern firewalls
  * Per-policy applicable *content*, *dns*, *tls*, *detection* and *authentication* profiles
  * Insightful CLI with configuration control

## TLS features:
  * dumping plaintext version of traffic to files, exporting sslkeylog
  * TLS security checks (OCSP, OCSP stapling, automatic CRL download)
  * HTML replacement browser warnings
  * STARTTLS support for most used protocols
  * Seemless HTTPS redirection to authentication portal

## Other:
  * Local and LDAP user authentication using builtin web portal
  * SOCKS4/SOCKS5 explicit proxy with DNS hostname support
  * DNS inspection allows FQDN policy objects
  * Policies based on FQDN and 2nd level DNS domain
  * both IPv4 and IPv6 are supported

## Tools:
  * built-in tools to help with CA and certificate enrollment needed to run smithproxy
  * auto-enrolling portal certificate based on system IP and hostname
  * auto-detect inspection interface(s) based on system routing information
  * check [pplay tool](https://pypi.org/project/pplay/): replays captures
    over the network with many cool features  
