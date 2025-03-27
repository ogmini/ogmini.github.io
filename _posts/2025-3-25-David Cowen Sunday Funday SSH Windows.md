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

## Exploring for Artifacts

I'll be using the built in Windows Event Viewer to search for logs and Process Monitor to look for any registry keys or files that have been touched during testing. 

## SSH Test Results

### Standard Username/Password Authentication to Debian OpenSSH Server

As expected and similarly to the [testing](https://ogmini.github.io/2025/03/21/David-Cowen-Sunday-Funday-SSH.html) we did on Linux, the client leaves behind very few artifacts. We don't even have the .bash_history to fall back on. Windows Command Prompt doesn't keep a history across sessions unless you delve into memory forensics in the moment. 

#### known_hosts

The only artifact we have is the known_hosts file which is located at `%userprofile%\.ssh`. Unlike Debian which hashes the hostname and IP by default, Windows does not so we can get more information from the file.

![known_hosts](/images/ssh-challenge-windows/known_hosts.png) 

#### Event Viewer

On the client Windows 11 machine, there are no relevant logs to be found in Event Viewer.

#### systemd

On the Debian machine, we again see logs relating the connection of the user. Nothing that specifically identifies the connecting machine as Windows 11. 

![systemd](/images/ssh-challenge-windows/systemd.png)

#### Active Connections

Using Process Monitor on the client Windows 11 Machine, we can see network traffic related to the SSH connection. This could also be done using net stat and other tools.

![network traffic](/images/ssh-challenge-windows/network.png)

### Standrd Username/Password Authentication to Windows OpenSSH Server

The artifacts on the client Windows 11 machine are the same as when connecting to the Debian machine. 

#### Event Viewer

On the server Windows 11 machine, there are relevant logs to be found in the Event Viewer. 

Under Applications and Services Logs -> OpenSSH -> Operational we find log entries related to the password attempts by the user. Interestingly enough, Failure and Success are both logged with EventID 4.

**Success**

![password success](/images/ssh-challenge-windows/ssh_logs_success.png)

**Failure**

![password failure](/images/ssh-challenge-windows/ssh_logs_fail.png)

The next logs are found under Windows Logs -> Security and some will start to look familiar as being related to the normal user logon process.

![ssh security logs](/images/ssh-challenge-windows/ssh_security_logs.png)

![ssh security logs full](/images/ssh-challenge-windows/ssh_security_full.png)

We can see the following EventIDs are related to a user connecting to the machine:
- 4717
- 4648
- 4624
- 4672
- 4718
- 4798
- 4634

What is interesting is that the account is initially logged in with a temporary user account named "sshd_4568" in this case. I'm unsure how the numbers at the end are determined. This is followed by the system assigning it impersonation privileges in EventID 4672. Next the actual user is logged on. At logoff, we see both the "sshd_4568" and the actual user account Logoff with EventID 4634.

