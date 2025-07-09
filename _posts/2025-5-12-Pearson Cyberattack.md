---
layout: post
title: Pearson - Cyberattack
author: 'ogmini'
tags:
 - News
---

![Legacy Data](/images/memes/only_legacy_data.png)

Another company gets bit by the trope of exposing Git secrets... Bleeping Computer has a news article at [https://www.bleepingcomputer.com/news/security/education-giant-pearson-hit-by-cyberattack-exposing-customer-data/](https://www.bleepingcomputer.com/news/security/education-giant-pearson-hit-by-cyberattack-exposing-customer-data/). This is such a big and sadly common threat vector that multiple tools and scripts exist to scan for these leaks. GitHub has a whole section dedicated to [secret scanning](https://docs.github.com/en/code-security/secret-scanning/introduction/about-secret-scanning) on its documentation about secure coding. Some tools hosted on GitHub itself include:

- [https://github.com/gitleaks/gitleaks](https://github.com/gitleaks/gitleaks)
- [https://github.com/awslabs/git-secrets](https://github.com/awslabs/git-secrets)
- [https://www.gitguardian.com/](https://www.gitguardian.com/)
- [https://github.com/trufflesecurity/trufflehog](https://github.com/trufflesecurity/trufflehog)

Many of these tools can be directly integrated into your workflows.

This isn't the first time Pearson has been hit by hackers. Back in 2018 they had a data breach and were later fined by the SEC for downplaying the impact. [https://www.bleepingcomputer.com/news/security/education-giant-pearson-fined-1m-for-downplaying-data-breach/](https://www.bleepingcomputer.com/news/security/education-giant-pearson-fined-1m-for-downplaying-data-breach/). One wonders if they learned from the last time...

Pearson's catalog of products has grown over time. Back in 2022, Pearson purchased Credly which is a widely used digital credentialing solution. They are the owners of Pearson VUE the testing center for many certifications and credentials from ISC2, CompTIA, and many more. Many eTextbooks are hosted by their platform. They [mention](https://plc.pearson.com/en-GB/news-and-insights/news/cyber-security-incident) that they "believe the actor downloaded largely legacy data". One argues if you are storing data it is important or required to be kept by laws and regulation. Is "legacy" data not important or sensitive? Data lifecycle management is part of cybersecurity. "Legacy" data shouldn't just be kept around.
