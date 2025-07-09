---
layout: post
title: Beyond Sunday Funday - SSH Artifacts in Windows 11 
author: 'ogmini'
tags:
 - DFIR
 - Research
---

Another successful entry/win to one of David Cowen's Sunday Funday Challenges involving SSH in Linux [https://www.hecfblog.com/2025/03/daily-blog-785-solution-saturday-32225.html](https://www.hecfblog.com/2025/03/daily-blog-785-solution-saturday-32225.html). This of course begs the question, what artifacts are left by SSH on Windows 11 or other Windows flavors! Windows uses OpenSSH for the client and server implementations [https://learn.microsoft.com/en-us/windows-server/administration/openssh/openssh-overview](https://learn.microsoft.com/en-us/windows-server/administration/openssh/openssh-overview) and this should make some of this research pretty straightforward when looking for artifacts specific to OpenSSH. Just as a quick example, the known_hosts file acts the same and is in essentially the same location as on Linux.

What will be interesting is what is logged differently and to where. Will the Windows Event Logs on a client machine store anything of use? I have to assume that it will store successful logins via SSH on the server machine.

My testing plan is going to be the same as before just swapping out the Debian 12 VMs for Windows 11 VMs. One acting as the client and the other as the server.
