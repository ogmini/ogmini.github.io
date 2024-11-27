---
layout: post
title: Microsoft Store Apps - Challenge in validating previous versions
author: 'ogmini'
tags:
 - DFIR 
---

> Have you ever needed a previous version of an application from the Microsoft Store?

During my research on [Windows Notepad](https://github.com/ogmini/Notepad-State-Library), I diligently kept track of the versions; but made one novice mistake. In digital forensics and IT as a whole, knowing what version of the software you're dealing with makes a huge impact on understanding artifacts or troubleshooting issues. The novice mistake I made was to not keep VMs or backups of the previous versions. I kept my testing VM up to date with the latest release and tested for changes for the format. I had taken it for granted that I would be able to download previous versions...

I am lucky that I started researching Windows Notepad when I did since I have seen changes in the behavior of the state files. The overall format hasn't changed, but the information that is kept and stored in the various sections has been altered. I will be writing a more detailed blog post and documentation at a later date expanding on these changes. This blog post will be focusing on my search on how to get previous versions of Microsoft Store Apps. 

I know that I tested on the following versions:

- 11.2402.22.0
- 11.2404.10.0
- 11.2407.9.0
- 11.2408.12.0
- 11.2409.9.0

Quick searching brought up [https://store.rg-adguard.net/](https://store.rg-adguard.net/) that is an "Online link generator for Microsoft Store". It potentially allows you to download previous versions of Microsoft Store Apps. Instructions on how to use the site can be found at [https://woshub.com/how-to-download-appx-installation-file-for-any-windows-store-app/](https://woshub.com/how-to-download-appx-installation-file-for-any-windows-store-app/). 

The following versions are able to be retrieved:

- 11.2407.9.0
- 11.2408.12.0
- 11.2409.9.0

Keen-eyed readers will notice something amiss between these two lists.

| Versions Tested | Versions Retrieved |
| --------------- | ------------------ |
| 11.2402.22.0 | ? |
| 11.2404.10.0 | ? |
| 11.2407.9.0 | 11.2407.9.0 |
| 11.2408.12.0 | 11.2408.12.0 |
| 11.2409.9.0 | 11.2409.9.0 |

Why can this tool only retrieve some previous versions? Alas, there is no information that I can find on how this tool works or what it leverages. My best guess is that it relies on some undocumented API calls to generate the Microsoft download links. It definitely cannot be relied on to get all previous versions that have existed. Any other tools such as [https://github.com/edgarchinchilla/ms-store-pkg-downloader](https://github.com/edgarchinchilla/ms-store-pkg-downloader) you find are probably leveraging rg-adguard's own exposed API.

Further searching reveals the Windows Package Manager tool or [WinGet](https://github.com/microsoft/winget-cli). Handy little system administration tool. Unfortunately, this was also a deadend as Microsoft Store Applications do not appear to report previous versions. It is able to get previous versions of other applications such as VS Code which have a different installer. This is confirmed by a maintainer of the WinGet project [https://github.com/microsoft/winget-cli/discussions/2086](https://github.com/microsoft/winget-cli/discussions/2086).

![WinGet Information for Windows Notepad](/images/msstore/WindowsNotepad-1.png)

![WinGet Information for VS Code](/images/msstore/VSCode-1.png)

Take note of the Version and Installer Type differences.

![WinGet Information for Windows Notepad - Versions](/images/msstore/WindowsNotepad-2.png)

![WinGet Information for VS Code - Versions](/images/msstore/VSCode-2.png)

This is where my searching came to an end leaving us with an uncomfortable resolution. 

__There is no good way to get all previous versions of a native Microsoft Store Application.__

I'm tempted to write up something to constantly poll [https://store.rg-adguard.net/](https://store.rg-adguard.net/) for releases and archive them. There was a feature request to add the capability of downloading previous version from the Microsoft Store that was rejected [https://github.com/microsoft/winget-cli/issues/4472](https://github.com/microsoft/winget-cli/issues/4472) and later reposted on the msstore-cli repository [https://github.com/microsoft/msstore-cli/issues/40](https://github.com/microsoft/msstore-cli/issues/40). Hopefully something changes. It is important that digital forensics researchers and investigators have access to previous versions of software. We need to be able to do regression testing in order to prove the validity of evidence.

![And on that bombshell](https://media1.tenor.com/m/lDojqc8WeegAAAAd/bombshell-clarkson.gif)

## References
- [https://store.rg-adguard.net/](https://store.rg-adguard.net/)
- [https://woshub.com/how-to-download-appx-installation-file-for-any-windows-store-app/](https://woshub.com/how-to-download-appx-installation-file-for-any-windows-store-app/)
- [https://github.com/edgarchinchilla/ms-store-pkg-downloader](https://github.com/edgarchinchilla/ms-store-pkg-downloader)
- [https://github.com/microsoft/winget-cli](https://github.com/microsoft/winget-cli)
- [https://github.com/microsoft/winget-cli/discussions/2086](https://github.com/microsoft/winget-cli/discussions/2086)
- [https://github.com/microsoft/winget-cli/issues/4472](https://github.com/microsoft/winget-cli/issues/4472)
- [https://github.com/microsoft/msstore-cli/issues/40](https://github.com/microsoft/msstore-cli/issues/40)