---
layout: post
title: Registry Hive - Revisiting Documentation
author: 'ogmini'
tags:
 - Registryhive
---

After yesterday's [post](https://ogmini.github.io/2025/06/16/RegistryHive-Binary-Template.html), I went back to reread what I consider the bible on the regf file format written by Joachim Metz. You can find it at [https://github.com/libyal/libregf/blob/main/documentation/Windows%20NT%20Registry%20File%20(REGF)%20format.asciidoc](https://github.com/libyal/libregf/blob/main/documentation/Windows%20NT%20Registry%20File%20(REGF)%20format.asciidoc). I find it is very useful to go back and reread documentation AFTER getting hands-on with the subject matter. You often can connect dots that you hadn't previously and previously innocuous data will leap out at you as being very important.

It answered some of my questions and confusion from yesterday. It was remnant data... This appears to be stuff that various tools will ignore. Probably for good reason as it is hard to verify and use it. I would argue it could come in handy looking for traces of evidence or other areas to look at. 

I did notice that the Data Types [section](https://github.com/libyal/libregf/blob/main/documentation/Windows%20NT%20Registry%20File%20(REGF)%20format.asciidoc#value_data_types) doesn't talk about the types found in the `settings.dat` files. Maybe something I can contribute.

