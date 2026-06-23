---
layout: post
title: Examining Tailscale Artifacts - Part 5
author: 'ogmini'
tags:
 - Tailscale
 - MacOS
---

It has been awhile since I've posted, currently attending IRC234 put on by Cybervance for CISA. Had a little time after the first day and decided to poke around Tailscale logs on MacOS. Just a warning, this post will be very light on details.

Going back to [Part 1](https://ogmini.github.io/2026/04/21/Examining-Tailscale-Artifacts.html), I looked at logs stored by the Windows client and focused a little on artifacts related to file transfers. MacOS stores them in the System Log and you can retrieve them using the sysdiagnose command. Instructions can also be found on Tailscale's support page at [https://tailscale.com/docs/features/logging?tab=macos](https://tailscale.com/docs/features/logging?tab=macos).

On the Windows client we saw the following line:

```txt
2026-04-21T15:02:44.495-04:00: localapi: [PUT] /localapi/v0/file-put/
```

On MacOS we see:

```txt
default	2026-06-22 21:18:42.990417 -0400	IPNExtension	localapi: [PUT] /localapi/v0/file-put/
```

Similar lines with some information added to support the System Log format. Namely, the Type and Process. Later in the log, I see the public IP for the computer I am transferring the file to:

```txt
default	2026-06-22 21:18:43.204547 -0400	IPNExtension	magicsock: disco: node [qZssQ] d:69d545be08f4adda now using [redacted]:41644 mtu=1360 tx=9f2ea8ed384c
```

This was a blind spot in my earlier testing since all the tailscale clients were on the same internal network. I'll need to go back and take a look at this.
