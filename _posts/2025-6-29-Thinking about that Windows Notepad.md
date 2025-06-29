---
layout: post
title: Thinking about that Windows Notepad
author: 'ogmini'
tags:
 - research
 - windows-notepad 
---

Driving, especially on long road trips, is when I do some of my best thinking. The 6 hour trip home gave me a lot of time to think about that Windows Notepad. My mind wandered and contemplated about a few topics:

1. Why the change in behaviour of unsaved buffer chunks for Markdown support? Pros/Cons?
2. More evidence that I was right about the More Options/Option Count.
3. There should be no code changes required to support this change.

## Why the change?

At first, it confused me that the Markdown Tabs do not use the unsaved buffer chunks. It would be difficult to properly support Markdown as the unsaved buffer chunks would not be able to place the Markdown tags in a clean fashion. What is fun, we see the return of the Content bytes for the unsaved Tabs which disappeared in [11.2408.12.0](https://ogmini.github.io/2025/04/24/Windows-Notepad-Version-Changes-(11.2407.9.0).html). Granted, this is just for the Markdown Tabs. 

## More Options / Option Count

Early on, there had been discussion if this byte was a version or count. I personally settled on count as it made the mose sense for ease of reading the correct number of later bytes for options. We are now up to 3 along with 3 more options. I still have not figured out what the previous 2 options are but it appears the last option is related to Markdown. It will be 1 for unformatted and 2 for Markdown formatted. I'm trending to calling this option "Tab Format" just in case they introduce more formats in the future. 

## No Code Changes

I'm going to pat myself on the back for this one. There should be no codes changes required to the binary templates or libary to support this change. I have not tested; but I wrote everything to handle a dynamic number of Options based on the Options Count. 

Now that I'm back home I can put some actual work and testing into this new feature for Windows Notepad. Expect more posts in the future with heavy testing and validation. 