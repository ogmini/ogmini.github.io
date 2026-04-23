---
layout: post
title: Examining Tailscale Artifacts - Part 3
author: 'ogmini'
tags:
 - Tailscale
---
Real short post today. I was able to verify that the LocalUserID is the SID on the computer for the user account Tailscale is being run under. This shows up in a few spots in the server-state.conf.

![whoami](/images/tailscale/sid.png)

We can see this SID in the following lines.

- "_current/S-1-5-21-306847822-306330466-671967995-1000": "profile-617d",
- "LocalUserID": "S-1-5-21-306847822-306330466-671967995-1000",

I created another local user on the computer and signed into Tailscale with another account. This results in more lines being added to the server-state.conf. There are now two "_current/[SID]: "profile-[ID]" lines for each local user. Each record under _profiles references the SID. With this information you can see which local accounts on the computer are using what tailscale accounts.
