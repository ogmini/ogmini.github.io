---
layout: post
title: RDCMan - Importance of DPAPI Activity
author: 'ogmini'
tags:
 - DFIR
 - DPAPI
 - RDCMan
---

Previously, I [posted](https://ogmini.github.io/2025/05/27/RDCMan-Verifying-DPAPI-Activity.html) about logging the DPAPI activity and its relevance to RDCMan. I did not go into why and how this information would be useful.

## Evidence of Execution

EventID 4688 gives us our evidence of execution for the RDCMan executable. It includes the path to the executable, which user executed it, and when.

Linking this back to EventID 16385 can show the use of saved credentials and more evidence of execution and possibly a successful connection.
