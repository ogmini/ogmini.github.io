---
layout: post
title: Homelab Part 1 - The Current Setup        
author: 'ogmini'
tags:
 - tools
 - homelab
---

I've always run a few personal "servers" at home running simple services like Plex, file storage, etc. When I started my Master's Degree, I wanted to setup a server to run Hyper-V so that I could keep all my coursework contained, backed up, and I could easily spin up VMs for exploration. Utilizing [tailscale](https://tailscale.com/) allowed me to access these VMs anytime, anywhere giving me the ability to easily work on assignments while on vacation or travelling. 

## Hardware Specs

- Asrock DeskMini X300 - [https://www.asrock.com/nettop/AMD/DeskMini%20X300%20Series/index.asp](https://www.asrock.com/nettop/AMD/DeskMini%20X300%20Series/index.asp)
- AMD Ryzen 5700G
- Noctua NH-K9a-AM4 Heatsink
- 64GB DDR4-3200 Crucial SODIMM
- 2TB Western Digital Blue NVMe
- 2 x 2TB Western Digital Blue

## Software

- Hyper-V Server 2019 (RIP standalone Hyper-V server)
- Veeam Backup & Replication Community Edition 

## Lessons Learned

Thank god I had configured and setup Veeam. I somehow managed to corrupt one of my VHDX files and was easily able to restore it using Veeam. Anyone thinking about setting up a Homelab should decide on their backup strategy. 

When I first setup the server, I only had 32GB and that was definitely not enough for me. I tended to run multiple VMs at the same time with each VM having at least 8GB or more. I'll talk about it in future posts; but I liked replicating the course labs in my own environment when possible. For one class, I setup my own Splunk instance and loaded the [BOTS v1 Dataset](https://github.com/splunk/botsv1). 

Tailscale is amazing and something I consider almost as important as your backup strategy. 

Your Homelab doesn't need to be expensive, complex, or a rackmount. My little deskmini is a tiny box running with a "laptop" power brick. It fits on the shelf next to my printer hooked up to a UPS. 

## Next Iteration

As I continue my journey, this little server is starting to show its limitations. 

- Memory limitations - I want more than 64GB
- Network limitations - one Gigabit NIC isn't enough. I want multiple NICs.
- CPU limitations - I want more cores 

Part 2 will delve into the next iteration. 
