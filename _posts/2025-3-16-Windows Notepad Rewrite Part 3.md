---
layout: post
title: Windows Notepad - Rewrite / AI Part 3
author: 'ogmini'
tags:
 - DFIR
 - windows-notepad
---

Short post today, continuing my research on Windows Notepad Rewrite [https://ogmini.github.io/2025/03/14/Windows-Notepad-Rewrite-Part-2.html](https://ogmini.github.io/2025/03/14/Windows-Notepad-Rewrite-Part-2.html)

## WebAccountId

Still not sure what this seeming 17 character string maps back to. When you sign out, the value of the WebAccountId key is set to 0. Signing back in with the same account results in the same WebAccountId value. This isn't surprising but I wanted to test and verify this behavior.

I think I'll need to spin up a second test MS365 account just to see if that gives me any more clues.

I'll probably also spin up some test code projects to interface with the Graph API and see if I can retrieve a WebAccountId. 
- [https://learn.microsoft.com/en-us/windows/uwp/security/web-account-manager](https://learn.microsoft.com/en-us/windows/uwp/security/web-account-manager)
- [https://github.com/MicrosoftDocs/windows-dev-docs/blob/docs/uwp/security/web-account-manager.md](https://github.com/MicrosoftDocs/windows-dev-docs/blob/docs/uwp/security/web-account-manager.md)

Maybe I should try my hand at this "vibe coding" to do this part.

