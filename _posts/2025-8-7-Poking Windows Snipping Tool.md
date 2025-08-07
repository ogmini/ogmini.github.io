---
layout: post
title: Poking Windows Snipping Tool aka Screen Sketch
author: 'ogmini'
tags:
 - Snipping-Tool
 - Registryhive
 - RegistryPlugins
---

While working on the registry plugin to read the `settings.dat` files associated with various Windows packaged apps [https://ogmini.github.io/2025/08/06/Registry-Plugin-Application-Settings-Container-(RECmd-and-Registry-Explorer).html](https://ogmini.github.io/2025/08/06/Registry-Plugin-Application-Settings-Container-(RECmd-and-Registry-Explorer).html), I did some testing against some other applications such as [Snipping Tool](https://apps.microsoft.com/detail/9mz95kl8mr0l?hl=en-US&gl=US) and [Photos](https://apps.microsoft.com/detail/9wzdncrfjbh4?hl=en-us&gl=US). Much like Windows Notepad, these are default software on Windows 11. To put it bluntly, the plugin has already proved useful to me in looking for the various digital artifacts that these might leave behind.

I'll be continuing to research them and will be documenting my findings in the future. As a preview, the Snipping Tool can be configured to automatically save screenshots and video recordings to a default path. The user can change the path where they are saved and that path is saved in the `settings.dat` file as a GUID. That GUID can be found in the associated `UserClasses.dat` which will translate the GUID into the folder path. Even more interesting, it appears to keep old paths. For example, I set the path to C:\MyFolder and later change it to C:\MySecretFolder. Both of these will exist in the `UserClasses.dat`. This needs to be tested more extensively to understand and document its behaviour.
