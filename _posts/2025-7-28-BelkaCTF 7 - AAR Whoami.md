---
layout: post
title: BelkaCTF 7 - Whoami
author: 'ogmini'
tags:
 - CTF
 - Belkasoft
 - Writeups
---

![Task](/images/BelkaCTF7/Task1.png)

The first task was a nice warmup in which we are provided a RAM dump from the ATC tower terminal computer which has been compromised with malware. We must determine the username and hostname of the machine. I found multiple ways to solve this challenge using both Volatility 3 and MemProcFS.

## Approach 1

In this approach, we can identify a process that has likely been launched by the user and retrieve its environment variables to determine the username and hostname.

### Volatility 3 - Approach 1

Running pslist will give us a list of processes.

~~~ cmd
vol -f BelkaCTF_7_CASE250722_KTSOAERO.mem windows.pslist
~~~

This nets us a likely candidate with a notepad process with a PID of 5316.

![PID 5316](/images/BelkaCTF7/Task1-1.png)

Using this PID, we grab the environment variables.

~~~ cmd
vol -f BelkaCTF_7_CASE250722_KTSOAERO.mem windows.envars --pid 5316
~~~

![envars](/images/BelkaCTF7/Task1-2.png)

COMPUTERNAME=TSO-ATC-CT412  
USERNAME=award

### MemProcFS - Approach 1

This time we go to the `name` folder of the mounted memory image and look for a process that has likely been launched by the user and retrieve its environment variables. Again, we identify `notepad.exe-5316` as a likely candidate. Opening the win-environment.txt file shows us the same environment variables as before.

![envars-memprocfs](/images/BelkaCTF7/Task1-3.png)

COMPUTERNAME=TSO-ATC-CT412  
USERNAME=award

#### Bonus Approach

We can also navigate to the `sys` folder and open the computername.txt file. The username can be found by going to the `sys\users` folder and opening the users.txt file.

## Approach 2

Grabbing a user's wallpaper is always a good first step as it can gain you some insight into the user or even system. Many companies will have a IT controlled wallpaper that may display information that is useful for their support staff. These can often display things such as IP, Username, and Computer Name.

### Volatility 3 - Approach 2

By running the windows.info plugin we can tell that the computer was running Windows 10.0.19041. This lets us know to look at the `HKCU\Control Panel\Desktop\Wallpaper` registry location for the path to the wallpaper image file. Visit [https://www.winhelponline.com/blog/find-current-wallpaper-file-path-windows-10/](https://www.winhelponline.com/blog/find-current-wallpaper-file-path-windows-10/) for more details.

~~~ cmd
vol -f BelkaCTF_7_CASE250722_KTSOAERO.mem windows.info
~~~

![windows.info](/images/BelkaCTF7/Task1-5.png)

Next we run the hivelist plugin to determine the offset of the ntuser.dat hive as 0xaa8e0c17d000. This offset is used with the printkey plugin to display registry keys and values that we are interested in.

~~~ cmd
vol -fBelkaCTF_7_CASE250722_KTSOAERO.mem windows.registry.hivelist

vol -f BelkaCTF_7_CASE250722_KTSOAERO.mem windows.registry.printkey --key "Control Panel\Desktop" --offset 0xaa8e0c17d000
~~~

![regkey](/images/BelkaCTF7/Task1-6.png)

Using the filescan plugin we can determine the offset for the location of the file as: 0xe6892a262770  \Users\award\WP_TSO-ATC-CT412.JPG.

~~~ cmd
vol -f BelkaCTF_7_CASE250722_KTSOAERO.mem windows.filescan
~~~

This is followed by using the dumpfiles plugin to dump the file at the offset.

~~~ cmd
vol -f BelkaCTF_7_CASE250722_KTSOAERO.mem windows.dumpfiles --virtaddr 0xe6892a262770
~~~

![wallpaper-volatility](/images/BelkaCTF7/Task1-7.png)

### MemProcFS - Approach 2

By looking at the `sys` folder and opening version.txt we know that the computer was running Windows 10.0.19041. This lets us know to look at the `HKCU\Control Panel\Desktop\Wallpaper` registry location for the path to the wallpaper image file. Visit [https://www.winhelponline.com/blog/find-current-wallpaper-file-path-windows-10/](https://www.winhelponline.com/blog/find-current-wallpaper-file-path-windows-10/) for more details.

We can grab the NTUSER.dat file from the `forensic\files\ROOT\Users\award` folder and examine it with Registry Explorer.

![ntuser](/images/BelkaCTF7/Task1-4.png)

We find a path to the wallpaper image of `C:\Users\award\WP_TSO-ATC-CT412.JPG` and navigating to that location in the forensic folder finds us the wallpaper image. Helpfully, it is in the same location as the NTUSER.dat file.

![wallpaper](/images/BelkaCTF7/Task1-wallpaper.JPG)

## Thoughts

I am starting to really like using MemProcFS. It feels much faster to just mount the memory image and browse through everything on the hunt. I would encourage you to give it a try if you haven't already. It would be interesting to run tools such as KAPE against the forensic folder. As you can see above, both can accomplish the same tasks.
