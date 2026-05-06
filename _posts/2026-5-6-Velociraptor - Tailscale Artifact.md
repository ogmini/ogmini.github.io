---
layout: post
title: Velociraptor - Tailscale Artifact
author: 'ogmini'
tags:
 - Tailscale
 - Velociraptor
---

Submitted a Tailscale artifact to the Velociraptor [Artifact Exchange](https://docs.velociraptor.app/exchange/?query=) that decodes and parses the configuration file and also grabs the logs. Tailscale has been used in the past in various attacks by threat actors such as:

- [https://cyble.com/blog/new-malware-campaign-abusing-rdpwrapper-and-tailscale-to-target-cryptocurrency-users/](https://cyble.com/blog/new-malware-campaign-abusing-rdpwrapper-and-tailscale-to-target-cryptocurrency-users/)
- [https://www.cisa.gov/news-events/cybersecurity-advisories/aa23-320a](https://www.cisa.gov/news-events/cybersecurity-advisories/aa23-320a)

The configuration file surfaces information about the Tailscale accounts, connected Tailnets, and unattended mode/account. These could help identify malicious Tailnet connections. Looking at the logs can potentially identify other machines on the Tailnet.
