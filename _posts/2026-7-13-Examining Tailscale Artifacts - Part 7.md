---
layout: post
title: Examining Tailscale Artifacts - Part 7
author: 'ogmini'
tags:
 - Tailscale
 - Linux
---

Continuing to look at Tailscale artifacts, this time on a Linux machine. Paths change but the artifacts are very similar to the Windows client.

| Windows | Linux |
| --- | --- |
| %ProgramData%\Tailscale | /var/lib/tailscale |

There appears to be one notable difference in that the `profile-data/*/netmap-cache` folder has files.

![linux netmap-cache](/images/tailscale/netmap-cachhe1.png)

![windows netmap-cache](/images/tailscale/netmap-cachhe2.png)

As the folder name suggests, these files do appear to be cached information about the network and the filenames appear to be hex-encoded ASCII. For example:

| Filename | Decoded |
| --- | --- |
| 646e73 | dns |
| 66696c746572 | filter |
| 706565722d6e4335626656733169323231434e54524c | peer-nC5bfVs1i221CNTRL |

Not sure why the Windows client doesn't leverage this or maybe my tests haven't triggered the caching. I'd have to dig into their source code to see if it provides any hints. In my limited testing/research, the following file types exist:

| Prefix | Filetype | Notes |
| --- | --- | --- |
| 757365722d | user- | Contains information about users that are a part of the tailnet. One file per user. Files also exist for tags. |
| 706565722d | peer- | Contains information about the peers/endpoints that are a part of the tailnet. |
| 646572706d6170 | derpmap | |
| 737368 | ssh | |
| 73656c66 | self | |
| 66696c746572 | filter | |
| 646e73 | dns | |
| 6d736963 | msic | |

There is definitely more to dig into here. From quick analysis, you could use this to easily rebuild the network map of the tailnet. This would include:

- Users
- Endpoints
- Which user owns the endpoint
- Routing rules

I am intrigued to see how similar/different the Android client is to the Linux client. This is a really useful digital artifact to have as you would need access to the owner's Tailscale control panel to typically get all this information. More to come as there is a ton to dig into and test...
