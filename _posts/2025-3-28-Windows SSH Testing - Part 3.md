---
layout: post
title: SSH Artifacts in Windows 11 - Part 3
author: 'ogmini'
tags:
 - sunday funday
 - challenge
---

Continuing from yesterday's [post](https://ogmini.github.io/2025/03/27/Windows-SSH-Testing-Part-2.html), we are looking at how to tell if someone is connected via SSH. I did not have time to look at SSH Tunnels. Please refer back to the main [post](https://ogmini.github.io/2025/03/25/David-Cowen-Sunday-Funday-SSH-Windows.html) for full details as this post will only talk about the tests and results. 

### Active Connections

While an SSH connection is active lets see what is visible. Unlike Debian, the Windows alternative to the `who` and `w` command shows us no useful information. By looking at the "Users" tab in Task Manager, we can see all users accounts that are currently logged into the computer. In the screenshot below, we only see the "User" that we logged into the server Windows machine and no "User" account for the SSH connection.

![users](/images/ssh-challenge-windows/users.png)

Running `netstat` shows us the active SSH connection on port 22.

![netstat](/images/ssh-challenge-windows/netstat.png)

If we look at the "Processes" tab in Task Manager, there is an sshd process and when there is an active SSH connection it will have 4 child processes as in the first screenshot below. One of the sshd processes is the parent one and so for every active SSH connection there will be:

- 1 Console Window Host process
- 2 sshd processes
- 1 Windows Command Processor process

The second screenshot below shows what happens when there are two active SSH connections.

![sshd-connected](/images/ssh-challenge-windows/sshd_connected.png)

![sshd-connected-multi](/images/ssh-challenge-windows/sshd_connected_multi.png)

A nicer hierarchical view is given by user Process Explorer.

![process_explorer](/images/ssh-challenge-windows/process_explorer.png)