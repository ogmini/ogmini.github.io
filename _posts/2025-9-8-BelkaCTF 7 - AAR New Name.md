---
layout: post
title: BelkaCTF 7 - AAR New Name
author: 'ogmini'
tags:
 - CTF
 - Belkasoft
 - Writeups
---

![Task](/images/BelkaCTF7/Task14.png)

The recovered WPA password can be used to decrypt the captured traffic. My steps differed from the official writeup.

In order to decrypt the traffic we need to add the decryption keys by going to Settings -> Preferences -> Protocols -> IEEE 801.22 and edit the decryption keys. This lets us add the "wpa-pwd" of "G0P1n3APL!:PINEAPPLENET". After that, you click apply and the traffic should be decrypted.

![wpa-pwd](/images/BelkaCTF7/Task14-1.png)

Next steps are to get a good idea of what traffic is being sent. I like to take a look at Protocol Hierarchy Statistics.

![protocol hierarchy](/images/BelkaCTF7/Task14-2.png)

There appears to be a lot of SMB2 traffic and it makes up 64% of the bytes! Might be worth taking a deeper look at this as it appears someone was transferring files back and forth.

I also take a look at the Conversations to see which endpoints are talking with each other. This can potentially help filter traffic. In the screenshot below, 15MB of data has been sent from 192.168.10.98 to 192.168.10.1.

![conversations](/images/BelkaCTF7/Task14-3.png)

Either way, lots of SMB traffic and Wireshark is able to export the objects for further analysis. File -> Export Objects -> SMB. It is pretty obvious that there is a shared folder on 192.168.10.1 and 192.168.10.98 transferred files to that folder.

![export](/images/BelkaCTF7/Task14-4.png)

We get a bunch of images and files and save them for further analysis. Highlighting a few interesting ones:

- client.conf - has some interesting text and a certificate in it. Makes mention of a "spacecraft"
- photos - our suspect is really bad at taking photos and keep getting their finger in the shot
- IMG_0313.JPG - photo of a weird hand holding a coffee cup with a weird name on it.

![coffee](/images/BelkaCTF7/Task14-5.jpg)

## Thoughts

Fun little challenge. Lucky that our suspect wasn't using SMBv3 or something with more security. If you are interested in how to handle SMBv3 in Wireshark the following presentation is a good starting point - [https://www.snia.org/sites/default/files/SDCEMEA/2019/SMB3seminar/Aptel-New_SMB3_features_in_Wireshark.pdf](https://www.snia.org/sites/default/files/SDCEMEA/2019/SMB3seminar/Aptel-New_SMB3_features_in_Wireshark.pdf)

I knew that the client.conf would come in handy later. Unfortunately, I wasn't able to make use of it during the CTF.
