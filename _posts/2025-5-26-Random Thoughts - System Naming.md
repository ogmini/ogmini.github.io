---
layout: post
title: Random Thoughts - System Naming
author: 'ogmini'
tags:
 - Musings
---

Had a lot of time to think during the Memorial Weekend holiday. My mind wandered to the naming scheme I have for system at home/work. It has changed over time from just leaving it at the default to funny names to role based names. How do you name your systems? Are your servers named after their roles? Are your computers named after their serial numbers? Are they completely randomized?

There could be security implications for role based naming for systems. The cyber kill-chain starts with the reconnaissance stage during which an attacker will try to enumerate your network. Maybe they'll steal existing documentation or scan with various tools. Role based names for your systems just provide another way to determine what each server does. As an example, let us say you have the following servers:

- US-DC-1
- US-DC-2
- JP-DC-1
- UK-DC-1

One could guess that these are all domain controllers for different regions/countries. The US appears to have failover, redundant, or load balanced domain controllers.

Naming your systems with their serial number is another practice. It gives you/your IT team a wealth of information while also giving an attacker the same. It is trivial to lookup the hardware configuration/model with a serial number. You'll get warranty information and maybe information about vulnerable BIOS versions that need to be patched.

This is not to dissuade one from using these naming schemes as they are incredibly useful from an IT standpoint. Honestly, an attacker will find this information via other means and naming your systems something completely random won't stop them from figuring it out. It may drive your IT administrators insane though.

Maybe don't name your server something like "PII SECURE SERVER".
