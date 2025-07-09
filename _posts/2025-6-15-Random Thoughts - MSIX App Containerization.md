---
layout: post
title: Random Thoughts - Implications of MSIX App Containerization
author: 'ogmini'
tags:
 - Musings
---

More of a thought exercise post on how UWP/Windows App SDK impacts the digital artifacts. It is Father's Day today so a really quick post. UWP was deprecated in favor of the Windows App SDK - [https://github.com/microsoft/WindowsAppSDK](https://github.com/microsoft/WindowsAppSDK). One of the key features is [MSIX](https://learn.microsoft.com/en-us/windows/msix/overview) which offers some nice features such as app containers. Quoting Microsoft here:

> Apps that are packaged using MSIX can be configured to run in a lightweight app container. The app's process, and its child processes, run inside the container, and are isolated using file system and registry virtualization. For more info, see MSIX AppContainer apps.
>
> All AppContainer apps can read the global registry. An AppContainer app writes to its own virtual registry and application data folder, and that data is deleted when the app is uninstalled or reset. Other apps don't have access to the virtual registry or virtual file system of an AppContainer app.

This is why we see the existence of `User.dat` and `UserClasses.dat` in very specific locations for each application.

In the course of writing this post and double checking via searching. I found a post talking about this registry redirection at [https://www.zerofox.com/blog/the-registry-hives-you-may-be-msix-ing-registry-redirection-with-ms-msix/](https://www.zerofox.com/blog/the-registry-hives-you-may-be-msix-ing-registry-redirection-with-ms-msix/). I always seem to find these articles AFTER I've figured most of it out. Makes sense though, as my search terms are more refined at that point.

The post doesn't make mention of the file redirection and why we will see those other folders such as Settings, LocalState, AppData, and others. Gratned, not every application will leverage these features. Something to look into more and document. I'm tempted to just write a test Windows App SDK as a test bed.
