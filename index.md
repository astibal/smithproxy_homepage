---
layout: default
---

# Smithproxy homepage

![](img/badger-status.png) [![travis](https://travis-ci.org/astibal/smithproxy.svg?branch=master)](https://travis-ci.org/astibal/smithproxy)
  [![docs](https://readthedocs.org/projects/smithproxy/badge/?version=latest&style=flat)](https://smithproxy.readthedocs.org)
  [![Docker Cloud Build Status](https://img.shields.io/docker/cloud/build/astibal/smithproxy)](https://hub.docker.com/repository/docker/astibal/smithproxy)[![Snapstore](https://img.shields.io/badge/snap_build-passing-passing)](https://snapcraft.io/smithproxy)  
![](img/badger-versions.png) [![GitHub tag (latest by date)](https://img.shields.io/github/v/tag/astibal/smithproxy)](https://github.com/astibal/smithproxy/releases)[![Core commit activity](https://img.shields.io/github/commit-activity/m/astibal/smithproxy)](https://github.com/astibal/smithproxy/)[![Lib commit activity](https://img.shields.io/github/commit-activity/m/astibal/socle)](https://github.com/astibal/socle/)[![Docs commit activity](https://img.shields.io/github/commit-activity/m/astibal/smithproxy_docs)](https://github.com/astibal/smithproxy_docs/)   
![](img/badger-love.png) [![become a patron](https://img.shields.io/badge/donate-patreon-FF338B)](https://www.patreon.com/bePatron?u=23520766)[![donate using liberapay](https://img.shields.io/badge/donate-liberapay-FFBE0B)](https://liberapay.com/astib/donate)


Smithproxy is highly configurable, fast and transparent TCP/UDP/TLS (SSL) proxy written in C++,
on top of homebrew socket proxying library called [*socle*](https://github.com/astibal/socle).  

Check [documentation](https://smithproxy.readthedocs.org) for more information about scenarios and how it can be deplyed.

### Availability:
  * **Linux** - can be installed as a service (distro packages, or easily compiled from sources)
  * **Docker** - available as an image on docker hub
  * **Snap** - you can also install it as a confined snap!

### Core features:
  * intercept **routed** traffic, **locally-originated** traffic and **SOCKS** requests
  * Policy based traffic matching
  * Per-policy applicable *content*, *dns*, *tls*, *detection* and *authentication* profiles
  * Insightful **CLI** with configuration control

### TLS features:
  * dumping plaintext to files, exporting sslkeylog
  * TLS security checks (ocsp, ocsp stapling, automatic crl download)
  * Html replacement warnings
  * STARTTLS support
  * Seamless HTTPS redirection to authentication portal

### Other:
  * Local and ldap user authentication using builtin web portal
  * SOCKS4/SOCKS5 explicit proxy with DNS hostname support
  * DNS ALG allows FQDN policy objects
  * Policies based on FQDN and 2nd level DNS domain
  * both IPv4 and IPv6 are supported

### Complementary tools:
  * built-in tools to help with network redirections and certificates
  * check [pplay tool](https://pypi.org/project/pplay/): replays captures
    over the network
