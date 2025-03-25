---
layout: post
title: David Cowen Sunday Funday Challenge - SSH Artifacts in Windows 11 
author: 'ogmini'
tags:
 - sunday funday
 - challenge
---

David Cowen has posted his weekly Sunday Funday challenge at his [blog](https://www.hecfblog.com/2025/03/daily-blog-786-sunday-funday-32325.html) and it is related to his previous challenge on SSH Artifacts in Linux systems. I had [posted](https://ogmini.github.io/2025/03/22/Beyond-Sunday-Funday-SSH-Artifacts-in-Windows-11.html) that looking at SSH Artifacts in Windows would be a natural extension and here we are. 

This week is a bit crazy, so I'll be working on this over a few days and doing small updates. This post will compile all the work for submission. 

## Challenge

Test what artifacts are left behind from SSHing into a Windows 11 or 10 system using the native SSH server. Bonus points for tunnels. 

## Test Setup

I am performing testing using three Hyper-V VMs

**Windows 11 Client**  
Windows 11 Pro 24H2 26100.3476     
Default installation
OpenSSH_for_Windows_9.5p1, LibreSSL 3.8.2   
![SSH Version](/images/ssh-challenge-windows/SSH-version-windows.png)       

**Windows 11 Server**   
Windows 11 Pro 24H2 26100.3476       
Default installation w/SSH Server Feature       
OpenSSH_for_Windows_9.5p1, LibreSSL 3.8.2  
![SSHD Version](/images/ssh-challenge-windows/SSHD-version-windows.png)   

**Debian-Server**   
Debian GNU/Linux 12   
Default installation w/SSH Server 
OpenSSH_9.2p1 Debian-2+deb12u5, OpenSSL 3.0.15 3 Sep 2024
![SSH Version](/images/ssh-challenge-windows/SSH-version-debian.png)   

### SSH Testing Steps

1. Open SSH connection from Client to Debian-Server using "standard" username/password authentication
2. Explore for artifacts
3. Open SSH connection from Client to Windows-Server using "standard" username/password authentication
4. Explore for artifacts
5. Add/Enable SSH Public Key to Windows-Server
6. Open SSH connection from Client to Windows-Server using the private key
7. Explore for artifacts

### SSH Tunnel Testing Steps

1. Open Local Port Forwarding from Client to Windows-Server using the private key
2. Explore for artifacts 

## Artifact Theories

I believe the artifacts will be similar to those found in Linux since both use OpenSSH. The paths will differ, but I expect to see a known_hosts file along with any public and private keys. Unlike Linux, Windows does not use systemd for logging; instead, it collects event logs which can be viewed in Event Viewer. It will be interesting to compare them and see if they both log similar information. 

**MORE TO COME**

