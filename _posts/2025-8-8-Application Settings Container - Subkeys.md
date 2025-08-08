---
layout: post
title: Application Settings Container - Subkeys!
author: 'ogmini'
tags:
 - Registryhive
 - RegistryPlugins
---

Yesterday, as I was diving into the Photos application in Windows with my registry plugin I noticed that there were subkeys to the LocalState key. I had not expected to see subkeys as nothing in the Microsoft [documentation](https://learn.microsoft.com/en-us/windows/apps/design/app-settings/store-and-retrieve-app-data) had led me to believe this was possible. I just ASSUMED it wasn't possible. In the screenshot below, you can see an "Exp" subkey and subkeys to that subkey.

![Subkeys](/images/registry/subkeys-localstate.png)

One immediate consequence was the need to update the registry plugin to handle subkeys. It was very obvious to see that they followed the same format as the key. That change has been incorporated and the Pull Request is just awaiting approval. The developer side of me of course wanted to see how this is pulled off in the code. Turns out it is pretty simple and you just need to create a container.

![Code](/images/registry/subkeys.png)
