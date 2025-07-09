---
layout: post
title: Windows Notepad - Markdown Support
author: 'ogmini'
tags:
 - Research
 - Windows-Notepad
---

I picked a great time to go on vacation. Looks like my test system got the update (11.2504.62.0) for Windows Notepad that adds support for Markdown.

![Markdown](/images/windowsnotepad/markdown.png)

![Markdown Settings](/images/windowsnotepad/markdown_settings.png)

From a quick perusal, behavior of the TabState file has changed in relation to unsaved tabs. Tabs that do NOT take advantage of the Markdown Formatting continue to use the Unsaved Buffer Chunks. Tabs that DO take advantage of the Markdown Formatting do not use the Unsaved Buffer Chunks and just saved the unsaved content in the Content.

![Markdown Unsaved](/images/windowsnotepad/markdown_unsaved.png)

![No Markdown Unsaved ](/images/windowsnotepad/ubc.png)

Also, looks like there is a new option in the [Configuration More Options Block](https://github.com/ogmini/Notepad-State-Library?tab=readme-ov-file#more-options-block) that indicates formatted or unformatted. 1 = Unformatted and 2 = Formatted. Needs more testing before I settle on a name and update the documentation. Still don't know what the first two are though.

This change sort of sucks as you can't replay back all the Unsaved Buffer Chunks for a formatted tab as they don't exist. It is just a content buffer of the current view.

I have not examined the `settings.dat` or any other files. I would hazard a guess that we'll see another key in the `settings.dat` for this setting.
