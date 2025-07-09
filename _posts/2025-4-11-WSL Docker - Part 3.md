---
layout: post
title: David Cowen Sunday Funday Challenge - Docker Containers on WSL Artifacts - Part 3
author: 'ogmini'
tags:
 - Sunday-Funday
 - Challenge
---

Not much time today either and there is so much to look at. I was able to examine the `com.docker.backend.exe.log` file in a little more detail. This log is obviously meant for troubleshooting docker; but it contains a wealth of information with timestamps.

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

Unfortunately, that is all the time I have for today.  
