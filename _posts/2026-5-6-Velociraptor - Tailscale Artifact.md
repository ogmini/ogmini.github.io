---
layout: post
title: Velociraptor - Tailscale Artifact
author: 'ogmini'
tags:
 - Tailscale
 - Velociraptor
---

Submitted a Tailscale artifact to the Velociraptor [Artifact Exchange](https://docs.velociraptor.app/exchange/?query=) that decodes and parses the configuration file, shows the logs, and grabs/uploads any partial file transfers to the machine. Tailscale has been used in the past in various attacks by threat actors such as:

- [https://cyble.com/blog/new-malware-campaign-abusing-rdpwrapper-and-tailscale-to-target-cryptocurrency-users/](https://cyble.com/blog/new-malware-campaign-abusing-rdpwrapper-and-tailscale-to-target-cryptocurrency-users/)
- [https://www.cisa.gov/news-events/cybersecurity-advisories/aa23-320a](https://www.cisa.gov/news-events/cybersecurity-advisories/aa23-320a)

The configuration file surfaces information about the Tailscale accounts, connected Tailnets, and unattended mode/account. These could help identify malicious Tailnet connections. Looking at the logs can potentially identify other machines on the Tailnet. There is also the possibility to recover partial file transfers that didn't complete. I still need to write up the blog post about that.
