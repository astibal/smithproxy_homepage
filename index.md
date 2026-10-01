---
layout: default
title: Smithproxy
description: Transparent traffic inspection for Linux
---

<section class="hero">
  <div class="hero-copy">
    <p class="eyebrow">OPEN SOURCE · GPL-3.0</p>
    <h1>See what moves<br><span>through your network.</span></h1>
    <p class="hero-lead">Smithproxy is a fast, policy-driven TCP, UDP and TLS inspection proxy for Linux. Capture decrypted traffic, inspect modern protocols and control how connections are handled.</p>
    <div class="hero-actions">
      <a class="button button-primary" href="https://smithproxy.readthedocs.io/en/latest/">Read the docs <span aria-hidden="true">→</span></a>
      <a class="button button-secondary" href="https://github.com/astibal/smithproxy">View on GitHub</a>
    </div>
    <div class="hero-meta" aria-label="Project highlights">
      <span>C++17</span><span>Linux</span><span>IPv4 + IPv6</span><span>Transparent + explicit</span>
    </div>
  </div>
  <div class="terminal-card" aria-label="Example output of the Smithproxy session list command">
    <div class="terminal-bar"><i></i><i></i><i></i><span>smithproxy CLI</span></div>
    <pre><code><span class="prompt">smithproxy#</span> diag proxy session list

MitM|l:ssl_<em>10.0.0.42:53124</em> &lt;+&gt;
     r:[sni]<strong>example.net:443</strong> policy: <mark>3</mark>
     up/dw: <span class="ok">24.8k/91.2k</span> (HTTP)

MitM|l:tcp_<em>10.0.0.18:49822</em> &lt;+&gt;
     r:<strong>1.1.1.1:53</strong> policy: <mark>1</mark>
     up/dw: <span class="ok">1.2k/3.8k</span> (DNS)

<span class="muted">Proxy performance: upload 26.0kbps,
download 95.0kbps in last 60 seconds</span></code></pre>
  </div>
</section>

<section class="proof-strip" aria-label="Core capabilities">
  <div><strong>TCP / UDP / TLS</strong><span>One policy engine</span></div>
  <div><strong>PCAPNG + GRE</strong><span>Decrypted capture</span></div>
  <div><strong>SOCKS + CONNECT</strong><span>Explicit proxy modes</span></div>
  <div><strong>HTTP/1 · HTTP/2 · QUIC*</strong><span>* QUIC is experimental</span></div>
</section>

<section class="section" id="capabilities">
  <div class="section-heading">
    <p class="eyebrow">CAPABILITIES</p>
    <h2>Inspection without a black box.</h2>
    <p>Smithproxy combines transparent interception, explicit proxy listeners and a firewall-like policy model in one inspectable C++ codebase.</p>
  </div>
  <div class="feature-grid">
    <article class="feature-card featured"><span class="feature-number">01</span><h3>Traffic interception</h3><p>Intercept routed and locally originated TCP, UDP and TLS traffic, or accept clients through SOCKS4/5 and HTTP CONNECT.</p><ul><li>TPROXY and REDIRECT</li><li>IPv4 and IPv6</li><li>DNS hostname resolution</li></ul></article>
    <article class="feature-card"><span class="feature-number">02</span><h3>Policy & routing</h3><p>Match traffic with ordered policies and apply TLS, DNS, content and detection profiles per connection.</p><ul><li>DNAT and load balancing</li><li>Outbound TLS SNI rewrite</li><li>Reject and sinkhole actions</li></ul></article>
    <article class="feature-card"><span class="feature-number">03</span><h3>Capture & observability</h3><p>Write intercepted traffic to rotating capture files or stream it to a remote collector for live analysis.</p><ul><li>PCAP / PCAPNG and GRE export</li><li>SSLKEYLOG export</li><li>Connection history and events</li></ul></article>
    <article class="feature-card"><span class="feature-number">04</span><h3>TLS intelligence</h3><p>Inspect handshakes, validate certificates and add useful client, server and HTTP fingerprints to session metadata.</p><ul><li>JA4, JA4S and JA4H</li><li>OCSP, CRL and Certificate Transparency</li><li>STARTTLS and KTLS</li></ul></article>
    <article class="feature-card"><span class="feature-number">05</span><h3>Automation interfaces</h3><p>Operate the proxy interactively or integrate it with surrounding systems and decision services.</p><ul><li>Interactive libcli2 CLI</li><li>Authenticated HTTP API</li><li>Webhooks and access requests</li></ul></article>
    <article class="feature-card experimental"><div><span class="feature-number">06</span><span class="pill">Experimental</span></div><h3>QUIC & HTTP/3</h3><p>Current development builds add transparent multi-flow QUIC handling and native HTTP/3 capture visibility.</p><ul><li>Stream-aware forwarding</li><li>QPACK header decoding</li><li>Native capture with TLS secrets</li></ul></article>
  </div>
</section>

<section class="section two-column" id="start">
  <div class="section-heading compact"><p class="eyebrow">GET STARTED</p><h2>Choose your path.</h2><p>Build the current code from source, use the published Linux packages, or run it from a Docker image.</p></div>
  <div class="start-list">
    <a href="https://github.com/astibal/smithproxy" class="start-item"><span class="start-icon">$</span><span><strong>Build from source</strong><small>Current code, build notes and issue tracker</small></span><b>→</b></a>
    <a href="https://download.smithproxy.org/" class="start-item"><span class="start-icon">↓</span><span><strong>Linux packages</strong><small>Published builds and release files</small></span><b>→</b></a>
    <a href="https://hub.docker.com/r/astibal/smithproxy" class="start-item"><span class="start-icon">◇</span><span><strong>Docker</strong><small>Container images and deployment path</small></span><b>→</b></a>
  </div>
</section>

<section class="section workflow">
  <div class="section-heading compact"><p class="eyebrow">TRAFFIC WORKFLOW</p><h2>Match. Inspect. Export.</h2></div>
  <div class="flow" role="img" aria-label="Traffic flows from client through policy matching and protocol inspection to upstream, with captures exported for analysis">
    <div><span>01</span><strong>Client traffic</strong><small>Transparent, SOCKS or CONNECT</small></div><i>→</i>
    <div><span>02</span><strong>Policy match</strong><small>Address, port, DNS and protocol</small></div><i>→</i>
    <div><span>03</span><strong>Inspect & route</strong><small>TLS, HTTP, DNS and content</small></div><i>→</i>
    <div><span>04</span><strong>Capture & export</strong><small>PCAPNG, GRE and webhooks</small></div>
  </div>
</section>

<section class="cta">
  <p class="eyebrow">READY TO LOOK INSIDE?</p><h2>Start with the documentation.</h2><p>Learn the deployment modes, configure your first policy and inspect a live session.</p>
  <div class="hero-actions centered"><a class="button button-primary" href="https://smithproxy.readthedocs.io/en/latest/">Open documentation <span aria-hidden="true">→</span></a><a class="button button-ghost" href="https://discord.gg/vf4Qwwt">Join Discord</a></div>
</section>
