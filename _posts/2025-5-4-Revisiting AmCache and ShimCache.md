---
layout: post
title: Revisiting ShimCache/AmCache
author: 'ogmini'
tags:
 - DFIR
 - ShimCache
 - AmCache
---

I came across a great [writeup](https://www.cybertriage.com/blog/shimcache-and-amcache-forensic-analysis-2025/) from Chris Ray over at Cyber Triage on ShimCache and AmCache. I made a [post](https://ogmini.github.io/2025/04/20/Beyond-Sunday-Funday-Shimcache-Amcache-MUICache-Prefetch.html) back on 4/20 about wanting to revisit various Windows Artifacts including ShimCache and AmCache. I really like Chris's writeup because it is clear, organized, and easy to read. I would highly suggest reading it even just as a refresher.

My favorite information from the writeup are the various limitations and caveats that they point out about using these digital artifacts in an investigation. They also specifically outline the forensic utility of each digital artifact. It is exactly these limitations and caveats that all investigators and researchers must be cognizant of in order to avoid confusion and mistakes.

If you don't read the article for some terrible reason. The biggest takeaway/TLDR:

ShimCache and AmCache provide evidence of EXISTENCE of an entry at a location. They should NOT be used to provide evidence of EXECUTION due to their complex behavior.

Finally, a big thanks to Andrew Rathbun for his continued work on maintaining KapeFiles so responsively. Everytime I submit something, I somehow learn something. My [pull request](https://github.com/EricZimmerman/KapeFiles/pull/1031) for a new Module got merged earlier today.

## References

- [https://www.cybertriage.com/blog/shimcache-and-amcache-forensic-analysis-2025/](https://www.cybertriage.com/blog/shimcache-and-amcache-forensic-analysis-2025/)
