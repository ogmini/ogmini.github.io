---
layout: post
title: SSH Artifacts in Windows 11 - Part 1
author: 'ogmini'
tags:
 - sunday funday
 - challenge
---

Continuing from yesterday's [post](https://ogmini.github.io/2025/03/25/David-Cowen-Sunday-Funday-SSH-Windows.html), we are testing for SSH artifacts when connecting to a Debian OpenSSH Server and a Windows 11 OpenSSH Server. Please refer back to the main [post](https://ogmini.github.io/2025/03/25/David-Cowen-Sunday-Funday-SSH-Windows.html) for full details as this post will only talk about the tests and results. 

### Exploring for Artifacts

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

** Success **
![password success](/images/ssh-challenge-windows/ssh_logs_success.png)

** Failure **
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

I'm out of time today; but I'll expand on this tomorrow. There is a lot more going on in the logs compared to Debian. This makes sense though as OpenSSH is sort of being shoe horned on top of Windows. It would be appear that impersonation is a part of that process. 