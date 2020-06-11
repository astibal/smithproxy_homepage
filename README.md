# Smithproxy homepage

  [![travis](https://travis-ci.org/astibal/smithproxy.svg?branch=master)](https://travis-ci.org/github/astibal/smithproxy)
  [![docs](https://readthedocs.org/projects/smithproxy/badge/?version=latest&style=flat)](https://smithproxy.readthedocs.org)
  [![Docker Cloud Build Status](https://img.shields.io/docker/cloud/build/astibal/smithproxy)](https://hub.docker.com/repository/docker/astibal/smithproxy)
  [![Snapstore](https://img.shields.io/badge/snap_build-passing-passing)](https://snapcraft.io/smithproxy)  
[![GitHub tag (latest by date)](https://img.shields.io/github/v/tag/astibal/smithproxy)](https://github.com/astibal/smithproxy/releases)[![Core commit activity](https://img.shields.io/github/commit-activity/m/astibal/smithproxy)](https://github.com/astibal/smithproxy/)[![Lib commit activity](https://img.shields.io/github/commit-activity/m/astibal/socle)](https://github.com/astibal/socle/)[![Docs commit activity](https://img.shields.io/github/commit-activity/m/astibal/smithproxy_docs)](https://github.com/astibal/smithproxy_docs/)    


Smithproxy is highly configurable, fast and transparent TCP/UDP/TLS (SSL) proxy.  

  
... you can also support our effort and [become a patron](https://www.patreon.com/bePatron?u=23520766), or donate using [liberapay](https://liberapay.com/astib/donate).

### Availability
  * **Linux** - can be installed as a service (distro packages, or easily compiled from sources)
  * **Docker** - available as an image on docker hub
  * **Snap** - you can also install it as a confined snap!

### Core features:
  * intercept **routed** traffic, **locally-originated** traffic and **SOCKS** requests
  * Policy based traffic matching
  * Per-policy applicable profiles
  * Insightful **CLI** with configuration control

### Policy profiles:
  * content - replacing data online, writing to disk
  * dns - dns inspection settigns
  * tls - per-policy specific settigns
  * auth - match users and enforce authentication
  * detection - match specific patterns in the traffic

### TLS protocol:
  * dumping plaintext to files, exporting sslkeylog
  * TLS security checks (ocsp, ocsp stapling, automatic crl download)
  * Html replacement warnings
  * STARTTLS support
  * Seamless redirection to login portal

### Other:
  * Simple authentication portal with local and ldap user support
  * SOCKS4/SOCKS5 support with DNS
  * DNS ALG
  * policies based on FQDN or 2nd level domain
  * both IPv4 and IPv6 is supported

### Complementary tools:
  * built-in tools to help with network redirections and certificates
  * check [pplay tool](https://pypi.org/project/pplay/): replays captures
    over the network
    

