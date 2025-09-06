---
layout: post
title: BelkaCTF 7 - AAR Party Van
author: 'ogmini'
tags:
 - CTF
 - Belkasoft
 - Writeups
---

![Task](/images/BelkaCTF7/Task13.png)

Skipping Task 12 for now as I am focusing on the ones I was able to solve during the CTF. In this task, I've been provided with a wireless network capture with encrypted traffic. I need to retrieve the password for the wireless network in order to decrypt the traffic.

First thing I do is load up the capture file in Wireshark to find the SSID of the wireless network. I can see the first packet showing the broadcast of the SSID as "PINEAPPLENET" from a TP Link device.  

![SSID](/images/BelkaCTF7/Task13-1.png)

I also retrieve the MAC Address of the TP Link device as 68:ff:7b:68:53:be. Using a website such as [https://maclookup.app/](https://maclookup.app/), I can verify that this MAC Address is assigned to TP Link. This isn't strictly needed to solve the task; but it provides another level of verification as we'll soon see.

![MAC](/images/BelkaCTF7/Task13-2.png)

Knowing the SSID, I now start looking at the Android device and leverage ALEAPP again. Under the Wi-Fi Profiles, the "PINEAPPLENET" SSID is found and it has a password of "G0P1n3APL!". The MAC Address listed under the DefaultGwMacAddress also matches the one found earlier.

![Profile](/images/BelkaCTF7/Task13-3.png)

## Thoughts

I dislike the official solution for this task as it does nothing to verify that the Wi-Fi Profile for "PINEAPPLENET" matches the network capture. They just go off the SSIDs and say that the other one has the word "GUEST" in it and is most likely not the home network of the suspect.
