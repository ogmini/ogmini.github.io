---
layout: post
title: Examining Tailscale Artifacts - Part 8
author: 'ogmini'
tags:
 - Tailscale
 - Linux
---

I've been working on a fun challenge related to Tailscale as I find it helps when doing research on possible digital artifacts and their behaviour. When I originally came across the files in the netmap-cache folder on Linux it was shortly after an install. I have now found that it appears this folder is periodically cleared/updated in some fashion. I have not tested if this is also consistent with Windows. So it is possible that those netmap-cache files may also exist on Windows installs.

What I noticed in the syslog for Linux is the following:

```syslog
[SNIPPED] tailscaled[3403]: updating netmap in disk cache
```

At the moment, I'm not sure what kicks this off or if it actually results in the netmap-cache files changing. I have not had a chance to test this closely or dig into the source code.

[https://github.com/tailscale/tailscale/blob/ca79c1e09b4ba34a0b8f071835e1383ba397bdf0/ipn/ipnlocal/diskcache.go](https://github.com/tailscale/tailscale/blob/ca79c1e09b4ba34a0b8f071835e1383ba397bdf0/ipn/ipnlocal/diskcache.go)

Just for reference, this is the previous post about netmap-cache - [https://ogmini.github.io/2026/07/13/Examining-Tailscale-Artifacts-Part-7.html](https://ogmini.github.io/2026/07/13/Examining-Tailscale-Artifacts-Part-7.html)
