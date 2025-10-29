---
layout: post
title: Packaged App - Registry Hives
author: 'ogmini'
tags:
 - Registryhive
 - UWP
 - MSIX
 - Helium
---

Over the last few months, I've been doing collaborative research with [reece394](https://github.com/reece394) over on GitHub. They made a recent feature request on the Registry Hunter repo [https://github.com/Velocidex/registry_hunter/issues/21#issuecomment-3427299524](https://github.com/Velocidex/registry_hunter/issues/21#issuecomment-3427299524) which encapsulates everything we've both researched so far very well. Mari Degrazia also has a very good write up over at [https://www.zerofox.com/blog/the-registry-hives-you-may-be-msix-ing-registry-redirection-with-ms-msix/](https://www.zerofox.com/blog/the-registry-hives-you-may-be-msix-ing-registry-redirection-with-ms-msix/) about the growing importance of these artifacts. Chris Ray over at Cyber Triage has also mentioned it previously at [https://www.cybertriage.com/blog/ntuser-dat-forensics-analysis-2025/](https://www.cybertriage.com/blog/ntuser-dat-forensics-analysis-2025/).

Up to this point, I've been loading each hive manually into tools like Registry Explorer or even Regedit. It works... but is rather inconvenient to have to dive into each one to check for Most Recently Used (MRU) entries. It is hard to easily see the BIG picture without lots of clicking. Maybe something to work on...

Various tools like KAPE can already easily collect all the various registry hives. At that point I can see two options:

1. Merge them all into one hive and leverage existing tools such as Registry Explorer
2. Parse all the hives and output/merge them into one CSV
3. Write another tool that would take in multiple hives and display them as if they were one

I'm trending to number one since it would only result in a small change to most workflows. The one BIG downside to this approach is I'm not sure how you would indicate to the investigator WHERE that key came from.
