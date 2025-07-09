---
layout: post
title: MSLab - Part 1
author: 'ogmini'
tags:
 - Homelab
---

I've had a few free moments to test out [MSLab](https://github.com/microsoft/MSLab) and it seems very promising. By just downloading the scripts, two ISOs, and modifying 2 lines in a configuration script I was able to spin up a virtual network with a Server 2025 Domain Controller and two Windows 11 client machines that are already joined to the domain. When I'm done with the lab, I can just run the cleanup script and it removes all the VMs from Hyper-V. Redeploying the exact same lab again just requires running the deploy script with the appropriate configuration.

I'm still exploring how the client machines can be pre-configured or have software installed on them during deployment. MSLab leverages the unattend.xml file for a lot of heavy lifting. This might be a possible avenue for running various commands. [https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-deployment-runsynchronous](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-deployment-runsynchronous)

![Lab](/images/mslab/part-1-lab.png)

Another option might be to leverage Ansible after the VMs have been created to finish configuration.

## Overall Steps

![profit](/images/mslab/profit-meme.jpg)

1. Download all the installation ISOs you will need. You will need Windows Server 2022/2025 to spin up the Domain Controller. The Evaluation version worked fine for me.
2. Run 1_Prereq.ps1 which will download required tools and install them.
3. Run 2_CreateParentDisk.ps1 and provide it the Windows Server ISO. This will create the Domain Controller VHDX file for the lab and do some cleanup on the scripts. A ParentDisks folder will be created which holds the base VHDX files to laeter be used to create the VMs.
4. In the ParentDisks folder, run CreateParentDisk.ps1 to create more VHDX files for the clients or other servers. This is how you create the VHDX files for Windows 11.
5. Edit LabConfig.ps1 to specify how you want Hyper-V to be setup and what VMs to create. You will be specifying what VMs to create and what VHDX files they will use as their base.
6. Run Deploy.ps1 and wait
7. When you are done, run Cleanup.ps1 to remove the VMs from Hyper-V.
