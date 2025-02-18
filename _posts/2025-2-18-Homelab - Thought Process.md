---
layout: post
title: Homelab Part 3 - Thought Process        
author: 'ogmini'
tags:
 - tools
 - homelab
---

![5900xt](/images/5900xt.png)

The [new Homelab server](https://ogmini.github.io/2025/01/18/Homelab-Next-Iteration.html) is up and running. I've actually been using it for the [Belkasoft Course](https://ogmini.github.io/2025/02/04/Belkasoft-Windows-Forensics-Passed.html), [Belkasoft CTF](https://ogmini.github.io/2025/02/11/Magnet-CTF.html), and [Magnet CTF](https://ogmini.github.io/2025/02/12/Magnet-CTF-Pre-Analysis.html).  

Controversial choice, I've disabled the RGB and will be disabling the RGB on the motherboard when I get the chance. 

## Decisions...

The new build had the following criteria:

1. Budget - Under $800
2. Memory - At least 128GB
3. Core Count - As much as possible

This hardware doesn't make me money and is for my own learning and enjoyment. Budget was foremost and I felt comfortable earmarking $800. I knew I wanted at least 128GB since my old Homelab server only had 64GB and I was feeling the limitation. More cores more better. 

I ended up going with AMD because of the core count and I'm not currently confident with P/E cores for use with Virtual Machines. The budget didn't allow for the AM5 platform and I ended up going with the older AM4 platform with a 5900XT CPU which nets me 16 cores/32 threads.  

I reused the following parts from older systems:
- EVGA CLC280 AIO
- Nvidia GTX 1070
- 2TB Western Digital Blue NVMe

The biggest decision was between continuing to run Hyper-V vs Proxmox. I've stuck with Hyper-V because I'm comfortable with it and have the backup infrastructure using Veeam already setup. I've always liked the ability to easily run and move Hyper-V virtual machines on pretty much any Windows system. Theoretically, I could share one of my virtual machines with anyone and they wouldn't need any special software. I will end up playing with Proxmox at some point on an older system. 