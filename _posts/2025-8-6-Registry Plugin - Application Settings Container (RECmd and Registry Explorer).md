---
layout: post
title: Registry Plugin (RECmd and Registry Explorer) - Application Settings Container
author: 'ogmini'
tags:
 - Registryhive
 - RegistryPlugins
 - Registry-Explorer
 - RECmd
---

I've been researching the `settings.dat` file that you will often find associated with various Windows packaged apps that might be distributed via the Windows Store or .msix files. You can see prior posts at [https://ogmini.github.io/tags.html#Registryhive](https://ogmini.github.io/tags.html#Registryhive). Andrew Rathbun had suggested adding support to RECmd/Registry Plugins to be able to read these values. I've finally carved out some time to figure it out and I have a working Registry Plugin. Still need to finish handling some of the lesser used Value Types before I submit the PR.

Just to give an idea of why/how this could be useful. By default Registry Explorer will give you this information without the plugin when examining the `settings.dat` for Windows Notepad. If you know what you are looking at you can convert everything in the Data column.

![No Plugin](/images/registry/RegistryPlugin-Normal.png)

With the plugin, that information is converted into an easily readable format.

![Plugin](/images/registry/RegistryPlugin-ApplicationSettingsContainer.png)

As you can see, there can be some potentially useful artifacts for Windows Notepad such as:

- RecentFiles
- WebAccountId

Other `settings.dat` or Application Settings Containers for various Windows packaged apps could also contain useful digital artifacts. Hopefully this plugin will make them much easier to parse and find.
