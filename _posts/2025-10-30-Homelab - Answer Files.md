---
layout: post
title: Homelab - Windows Answer files (unattend.xml/autounattend.xml)
author: 'ogmini'
tags:
 - Homelab
---

Recently stumbled upon something to make my life easier as I find myself spinning up VMs on Hyper-V constantly to perform testing and validation. I prefer to have completely clean installs and tend to not reuse a VM for later testing. This can result in a sort of "blindspot" to finding artifacts on an actually "used" computer. This post isn't about that though. When creating my VMs I have always made use of [Answer files](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/update-windows-settings-and-scripts-create-your-own-answer-file-sxs?view=windows-11) to automate the installation and configuration to reduce human error and increase repeatability. You can use [https://schneegans.de/windows/unattend-generator/](https://schneegans.de/windows/unattend-generator/) to generate different Answer files.

I had a small collection of Answer files that I would need to integrate into the installation ISO file for windows. This isn't a hard process but can be time consuming. This resulted in me having a collection of different ISO files for each Answer file. Rather unwieldy and wasteful for storage. There is an option that I stumbled upon to "Download .xml warpped in .iso file" that is REALLY handy. Now I can just have a collection of small (380ish kb) ISO files which can be used alongside the standard Windows installation ISO file. When setting up a VM, I just have to mount both ISO files and away we go. This works because the installer will look for a `autounattend.xml` on any media such as USB keys, optical media, etc.

This also works for VirtualBox, VMware Workstation, Proxmox VE, Hyper-V, and Parallels Desktop. Step by step instructions can be found at [https://schneegans.de/windows/unattend-generator/usage/](https://schneegans.de/windows/unattend-generator/usage/.)
