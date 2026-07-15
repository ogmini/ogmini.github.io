---
layout: post
title: Tailscale Security Bulletin (TS-2026-009) and Detection 
author: 'ogmini'
tags:
 - Tailscale
 - Linux
---

A friend alerted me to a security bulletin [https://tailscale.com/security-bulletins#ts-2026-009](https://tailscale.com/security-bulletins#ts-2026-009) released by Tailscale about a vulnerability related to Tailscale SSH. I was still running a vulnerable version in my testing environment and wanted to test this out and see what kind of artifacts it leaves behind. Would be useful for anyone wanting to check their logs for potential exploitation.

First, a quick video of exploitation. If you want a "detection rule" via grep or journalctl feel free to scroll down to the bottom.

<video width="960" height="540" controls>
 <source src="https://ogmini.github.io/images/ts-2026-009/Tailscale_(TS-2026-009)_Example.mp4" type="video/mp4">
 Your browser does not support the video tag
</video>

![Login 1](/images/ts-2026-009/login-1.PNG)

![Login 2](/images/ts-2026-009/login-2.PNG)

In the video, I'm using Putty on Windows to open an SSH session with my tailscale node. I login with the user `-i` which doesn't exist. It immediately drops me a the root login and I verify that by running `whoami`.

The first question I asked myself is if can I see this login attempt in the tailscaled logs? They will not show up in the SSH server (Dropbear in this case) logs as the Tailscale client is handling all of this. I check the syslog file at `/var/log/syslog` and see the following:

```syslog
2026-07-15T13:33:34.535160-04:00 DietPiHomeLab tailscaled[592]: magicsock: disco: node [/OSRQ] d:81766ca7a1b911d6 now using 192.168.0.205:41641 mtu=1360 tx=398c82f127ad
2026-07-15T13:33:34.685284-04:00 DietPiHomeLab tailscaled[592]: control: NetInfo: NetInfo{varies=true ipv6=false ipv6os=true udp=true icmpv4=false derp=#1 portmap=active-UMC link="" firewallmode="ipt-default"}
2026-07-15T13:33:34.685378-04:00 DietPiHomeLab tailscaled[592]: magicsock: endpoints changed: [REDACTED_IP]:41647 (portmap), [REDACTED_IP]:1720 (stun), [REDACTED_IP]:41641 (stun4localport), 192.168.0.212:41641 (local)
2026-07-15T13:33:34.733485-04:00 DietPiHomeLab tailscaled[592]: magicsock: disco: node [/OSRQ] d:81766ca7a1b911d6 now using 192.168.0.1:41641 mtu=1360 tx=f2dfd1d933fd
2026-07-15T13:33:35.371241-04:00 DietPiHomeLab tailscaled[592]: ssh-session(sess-20260715T172750-97360649bb): Session complete
2026-07-15T13:33:48.019352-04:00 DietPiHomeLab tailscaled[592]: ssh-conn-20260715T173345-1d65e5d9b8: handling conn: 100.85.179.88:50798->-i@100.105.171.109:22
2026-07-15T13:33:48.029603-04:00 DietPiHomeLab tailscaled[592]: ssh-conn-20260715T173345-1d65e5d9b8: starting session: sess-20260715T173348-860d5964ce
2026-07-15T13:33:48.029694-04:00 DietPiHomeLab tailscaled[592]: ssh-session(sess-20260715T173348-860d5964ce): handling new SSH connection from [REDACTED_USER_EMAIL] (100.85.179.88) to ssh-user "root"
2026-07-15T13:33:48.029784-04:00 DietPiHomeLab tailscaled[592]: ssh-session(sess-20260715T173348-860d5964ce): access granted to [REDACTED_USER_EMAIL] as ssh-user "root"
2026-07-15T13:33:48.029855-04:00 DietPiHomeLab tailscaled[592]: audit: SSH login: user=root uid=0 from=100.85.179.88 ts_user=[REDACTED_USER_EMAIL] node=outsider.[REDACTED_TAILNET].ts.net.
2026-07-15T13:33:48.030395-04:00 DietPiHomeLab tailscaled[592]: ssh-session(sess-20260715T173348-860d5964ce): starting pty command: [/usr/sbin/tailscaled be-child ssh --login-shell=/bin/bash
2026-07-15T13:33:48.030642-04:00 DietPiHomeLab tailscaled[592]: daemon --uid=0 --gid=0 --groups=0 --local-user=root --home-dir=/root --remote-user=[REDACTED_USER_EMAIL] --remote-ip=100.85.179.88 --has-tty=true --tty-name=pts/0 --force-v1-behavior --shell]
2026-07-15T13:33:55.537832-04:00 DietPiHomeLab tailscaled[592]: control: NetInfo: NetInfo{varies=false ipv6=false ipv6os=true udp=true icmpv4=false derp=#1 portmap=active-UMC link="" firewallmode="ipt-default"}
```

The following syslog entry in particular shows us the passing of the `-i` as the username called out by the security bulletin:

```syslog
2026-07-15T13:33:48.019352-04:00 DietPiHomeLab tailscaled[592]: ssh-conn-20260715T173345-1d65e5d9b8: handling conn: 100.85.179.88:50798->-i@100.105.171.109:22
```

![Who is -i?](/images/ts-2026-009/who_is_-i.png)

Subsequently, I can see a login from root:

```syslog
2026-07-15T13:33:48.029694-04:00 DietPiHomeLab tailscaled[592]: ssh-session(sess-20260715T173348-860d5964ce): handling new SSH connection from [REDACTED_USER_EMAIL] (100.85.179.88) to ssh-user "root"
```

Now that I know what to look for, I can write a simple grep/journalctl command to search for line that match the pattern. This could probably be converted to a Sigma rule or other detection rule; but, I don't have a testing environment spun up to test and validate them.

```bash
grep -E 'tailscaled[[0-9]+]: ssh-conn-[^:]+: handling conn: .*->-i@' /var/log/syslog
```

```bash
journalctl -u tailscaled | grep -E 'ssh-conn-[^:]+: handling conn: .*->-i@'
```

As a quick sidenote, this will log the user -i as the first user listed in the passwd file. I verified this and was able to log in as a different user if I placed them at the top fo the passwd file.