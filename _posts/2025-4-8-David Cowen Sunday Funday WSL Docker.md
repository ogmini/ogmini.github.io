---
layout: post
title: David Cowen Sunday Funday Challenge - Docker Containers on WSL Artifacts
author: 'ogmini'
tags:
 - Sunday-Funday
 - Challenge
---

Another week, another David Cowen Sunday Funday challenge posted at his [blog](https://www.hecfblog.com/2025/04/daily-blog-800-sunday-funday-4625.html) and it is about looking for artifacts left behind by using Windows Subsystem for Linux (WSL) to run a docker container.

We'll see how far I can get with testing. As always, this post will serve as the merged repository for later posts on this topic.

## Challenge

What artifacts are left behind when running a docker container using Ubuntu WSL (which I believe is the default standard. Bonus points for artifacts that reflect interactions between the container and the host.

## Test Setup

This is a pretty wide open challenge as there are no specifications on what docker container to run or even version of WSL. So, we get to make those choices and figure out what we want to test and how we're going to test it.

### OS Setup

Windows 11 24H2
WSL 2 [https://learn.microsoft.com/en-us/windows/wsl/compare-versions](https://learn.microsoft.com/en-us/windows/wsl/compare-versions)
Docker Desktop v4.40.0
Kali Linux Container

We'll spin up our standard W11 VM and make sure WSL2 is configured and enabled. Next, we'll install Docker Desktop and make sure that it is using WSL2. Last step is to get the Kali Linux Container up and running.

[https://umatechnology.org/run-kali-linux-on-windows-10-in-docker/](https://umatechnology.org/run-kali-linux-on-windows-10-in-docker/)

### Tests

1. Look for artifacts left by setup/installation
2. Create network taffic
3. Save files
4. TBD

## Findings

Some locations of interest that have already surfaced:

| Folder Location | Comments |
| --- | --- |
| `%localappdata%\Docker\log` | Contains logs |
| `%localappdata%\Docker\wsl` | Contains vhdx files related wot WSL and Containers |
| `%userprofile%\.docker\contexts\meta` | |

The `com.docker.backend.exe.log` file is of great interest as we can see all the events going on with containers and so forth. For example the line:

> [2025-04-10T20:27:47.539553700Z] [com.docker.backend.exe.ports] exposer.Add(de1d1ec90a81dff7dc48705fbf17c81ec6c4717da6df1056f65360e8049aacd9, /great_matsumoto, [])

Shows us the running of a container which got the name of great_matsumoto.

Starting a container with the command "docker run -it kalilinux/kali-rolling /bin/bash" in PowerShell results in many records. I'm picking out a few of interest:

> [2025-04-11T22:31:20.393454600Z] [com.docker.backend.exe.apiproxy] >> POST /v1.48/containers/create
>
> [2025-04-11T22:31:20.511789100Z] [com.docker.backend.exe.ipc] (0a2f01e1-5) b53e1d8e-DockerApiProxyBackendClient C->S BackendAPI POST /usage/image: {"agent":"Cli","attachStderr":true,"attachStdin":true,"attachStdout":true,"cmd":["/bin/bash"],"containerID":"...","image":"kalilinux/kali-rolling","openStdin":true,"restartPolicyName":"no","stdinOnce":true,"tty":true}
>
> [2025-04-11T22:31:20.519049400Z] [com.docker.backend.exe.apiproxy] >> GET /containers/423c1dc2da5d64e8d5b974fe06191ef59940327b3ba1ecd6d38ddcce5e6ee1d4/json
>
> [2025-04-11T22:31:20.531683100Z] [com.docker.backend.exe.apiproxy] >> GET /v1.48/images/kalilinux/kali-rolling/json
>
> [2025-04-11T22:31:21.149288500Z] [com.docker.backend.exe.ports] exposer.Add(423c1dc2da5d64e8d5b974fe06191ef59940327b3ba1ecd6d38ddcce5e6ee1d4, /pensive_jang, [])

Later, I stop the container and we get the following log:

> [2025-04-11T22:32:56.809549200Z] [com.docker.backend.exe.ports] exposer.Remove(423c1dc2da5d64e8d5b974fe06191ef59940327b3ba1ecd6d38ddcce5e6ee1d4)

Finally, I delete the container:

> [2025-04-11T22:33:04.816347800Z] [com.docker.backend.exe.apiproxy] >> DELETE /containers/423c1dc2da5d64e8d5b974fe06191ef59940327b3ba1ecd6d38ddcce5e6ee1d4?force=true&v=true

Some useful things to point out. The ID of the container that was created is "423c1dc2da5d64e8d5b974fe06191ef59940327b3ba1ecd6d38ddcce5e6ee1d4" in this case. This allows you to quickly find any related logs by searching for taht ID. We only ever see the auto-generated name of the container, "pensive_jang" once in the log.

The `monitor.log` also contains similar log messages to the `com.docker.backend.exe.log`.
