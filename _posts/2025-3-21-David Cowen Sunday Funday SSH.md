---
layout: post
title: David Cowen Sunday Funday Challenge - SSH Artifacts 
author: 'ogmini'
tags:
 - Sunday-Funday
 - Challenge
---

David Cowen has posted his weekly Sunday Funday challenge at his [blog](https://www.hecfblog.com/2025/03/daily-blog-779-sunday-funday-31625.html) and I'm going to take a stab at it today.

## Challenge

What are all of the artifacts left behind on a Linux system (both server and client) when someone authenticates via SSH and creates a SSH Tunnel.

## Test Setup

I am performing testing using two Hyper-V VMs

**Debian-Client**  
Debian GNU/Linux 12
Default installation

**Debian-Server**
Debian GNU/Linux 12
Default installation w/SSH Server  

### SSH Testing Steps

1. Open SSH connection from Debian-Client to Debian-Server using "standard" username/password authentication
2. Explore for artifacts
3. Create SSH Private/Public Keys
4. Add/Enable SSH Public Key to Debian-Server
5. Open SSH connection from Debian-Client to Debian-Server using the private key
6. Explore for artifacts

### SSH Tunnel Testing Steps

1. Open Local Port Forwarding from Debian-Client to Debian-Server using the private key
2. Explore for artifacts

### Exploring for Artifacts

To find potential artifacts I'll be using the `find` command to look for recently modified files in the filesystem. An example of the command would be:

`find / -type f -newermt "2025-3-21 8:00:00" ! -newermt "2025-3-21 9:00:00"`

I'll also be using the `find` command combined with `stat` to look for newly created files in the filesystem. An example of the command would be:

`find / -type f -exec stat --format='%W %n' {} \; | awk '$1 >= 1742571004 && $1 < 1742571600'`

Debian 12 many other modern Linux distros use systemd for logging instead of the traditional .log files. I'll be using `journalctl` to examine those logs. An example of the command line would be:

`journalctl --since "2025-3-21 8:00:00" --until "2025-3-21 9:00:00"`

## SSH Test Results

### Standard Username/Password Authentication

Using the `find` command to look for any modified/created files results in hits on:

- /home/[user]/.ssh/known_hosts
- /home/[user]/.ssh/known_hosts.old
- /var/log/journal/[identifier]/system.journal

![Modified](/images/ssh-challenge/1-modified.png)

![Created](/images/ssh-challenge/1-created.png)

Funnily enough, this didn't pick up the modification on the .bash_history file. This makes sense though, since the modified timestamp will constantly be updated as I type bash commands so the act of me running the `find` command would have made the file fall outside of my query range.

#### .bash_history

Examining the .bash_history file with nano shows the specific ssh command used which is `ssh ogmini@debian-server`.

[https://linuxconfig.org/how-to-manage-bash-history](https://linuxconfig.org/how-to-manage-bash-history)  

![bash_history](/images/ssh-challenge/1-bash_history.png)

#### known_hosts

The known_hosts file contains records for the Hostname, IP, Key Type, and Key. In our case, the Hostname and IP are obscured or rather hashed as the default setting for HashKnownHosts is yes. This can be changed on a system-wide basis by editing the /etc/ssh/ssh_config file or on a user basis by editing the ~/.ssh/config file. You should keep this hashed for obvious reasons.

[https://linux-audit.com/ssh/audit-ssh-configurations-hashknownhosts-option/](https://linux-audit.com/ssh/audit-ssh-configurations-hashknownhosts-option/)  

![known_hosts](/images/ssh-challenge/1-known_hosts.png)

#### systemd

Examining the System Journal on Debian-Client shows no interesting entries or artifacts. The hit for `find` was showing entries for other processes running on the system not related to SSH. The System Journal on Debian-Server does show entries related to the SSH connection from Debian-Client.

- Connection and password accepted from 172.22.182.21 for the user "ogmini"
- Session is created
- Disconnect command received
- Session is closed and cleaned up

![journalctl-server](/images/ssh-challenge/1-serverjournalctl.png)

#### Active Connections

Using the `ss` command we can also see active connections. This is only useful in the moment but is one way to see any active SSH connections. Active connections will be present on both the client and server.

![active connection](/images/ssh-challenge/activeconnection.png)

The `who` and `w` commands are also useful to run on the server. Again, this is only useful in the moment but can show us active user sessions with some detail. Little tidbit of history concering the TTY or "Teletypewriter" column which can tell us what type of terminal is connecting. "pts" lets us know its a pseudo terminal or SSH/terminal emulator. We can see what IP the user is logged in from along with login time and how long they've had the connection open.

![who](/images/ssh-challenge/who.png)  

![w](/images/ssh-challenge/w.png)

### SSH Private/Public Key

One important note, this test was conducted after authenticating to the same SSH Host using a username/password. As such, no modifications were made to the known_hosts file.

#### .bash_history - Private/Public Key

Again, the .bash_history file shows the ssh commands being executed. You can see me creating the keys using ssh-keygen and sending my public key to the server. Bad testing on my part, the name/comment for my key is the same as the server. So the command to connect with the public key to the host is very similar to the standard username/password. The only difference is the single quotes encircling the name/comment.

`ssh ogmini@debian-server` vs ssh `'ogmini@debian-server'`

![bash_history2](/images/ssh-challenge/2-bash_history.png)

#### Private/Public Keys

You can see the name/comment in the public key.

![public key](/images/ssh-challenge/2-publickey.png)

Additionally, the Private/Public keys are typically stored in ~/.ssh location on the Debian-Client.

![keys](/images/ssh-challenge/2-keys.png)

The Debian-Server saves the Public Key in the authorized_keys file located at ~/.ssh for the relevant user. The authorized_keys file could also be located at /root/.ssh for system-wide management.

![authorizedkeys](/images/ssh-challenge/2-authorizedkey.png)

#### systemd - Private/Public Key

Examining the System Journal on Debian-Client shows no interesting entries or artifacts. The System Journal on Debian-Server does show entries related to the SSH connection from Debian-Client.

- Connection and public key accepted from 172.22.182.21 for the user "ogmini"
- Session is created
- Disconnect command received
- Session is closed and cleaned up

![journalctl-server2](/images/ssh-challenge/2-serverjournalctl.png)

#### Active Connections - Private/Public Key

Using the `ss` command we can also see active connections. This is only useful in the moment but is one way to see any active SSH connections. Active connections will be present on both the client and server.

![active connection](/images/ssh-challenge/activeconnection.png)

The `who` and `w` commands are also useful to run on the server. Again, this is only useful in the moment but can show us active user sessions with some detail. We can see what IP the user is logged in from along with login time and how long they've had the connection open.

![who](/images/ssh-challenge/who.png)

![w](/images/ssh-challenge/w.png)

## SSH Tunnel Test Results

Very similar artifacts to the SSH tests. The .bash_history file will show the command being executed. The System Journal on the server will show the connection being established and cleaned up.

- Connection and public key accepted from 172.22.182.21 for the user "ogmini"
- Session is created
- Disconnect command received
- Session is closed and cleaned up

![journalctl-server3](/images/ssh-challenge/3-serverjournalctl.png)

Using the `ss` command we can also see active connections. This is only useful in the moment but is one way to see any active SSH connections. Active connections will be present on both the client and server. One thing to note, we can see the local port that is being forwarded which in this case is 8080. Pay attention to the Process column.

![active connection](/images/ssh-challenge/3-activeconnection.png)

The `who` and `w` commands are also useful to run on the server. Again, this is only useful in the moment but can show us active user sessions with some detail. We can see what IP the user is logged in from along with login time and how long they've had the connection open.

![who](/images/ssh-challenge/who.png)

![w](/images/ssh-challenge/w.png)

## Conclusion

For a default installation of Debian 12 the following files/locations could be of interest when trying to track SSH connections:

| File/Location | Server/Client | Notes |
|---|---|---|
| ~/.bash_history | Client | Shows history of bash commands. Limited by configurations. No timestamps. |
| ~/.ssh/known_hosts | Client | Hashed Hostnames/IPs of trusted remote hosts. |
| systemd logs | Server | Logs on the server will show details about connections. Nothing on the client. |
| Private Key | Client | Typically stored at ~/.ssh. |
| Public Key | Both | Typically stored at ~/.ssh and possibly /root/.ssh on the server. |

The .bash_history file can help determine what hosts have been connected to using SSH by the user. Configuration settings will determine how far back this history file goes back. One downside is the absence of any timestamps for the commands. Using the known_hosts file can possibly identify what hosts are trusted assuming they are not hashed.

Assuming the Private key isn't passphrase protected, having the Private/Public keys are a big boon to any investigation. They would allow the investigator to connect to the remote host.

Specifically for Linux systems running systemd, the systemd logs can provide information about clients connecting with SSH.

In the moment, the following commands can be useful to investigate SSH connections:

| Command | Server/Client | Notes |
|---|---|---|
|ss|Both|Shows active SSH connections and also forwarded ports from an SSH Tunnel|
|who|Server|Shows active user sessions|
|w|Server|Shows active user sessions and a little more information than `who`|
