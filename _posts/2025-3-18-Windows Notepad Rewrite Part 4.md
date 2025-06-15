---
layout: post
title: Windows Notepad - Rewrite / AI Part 4
author: 'ogmini'
tags:
 - DFIR
 - Windows-Notepad
---

Progress has been made since [Part 3](https://ogmini.github.io/2025/03/16/Windows-Notepad-Rewrite-Part-3.html) in relation to the network traffic and API calls. As we discovered earlier, Windows Notepad makes API calls to apsaiservices.microsoft.com using TLSv1.3. I've now successfully decrypted the calls and I'm making progress in understanding the traffic. 

## Network Traffic

I used mitmproxy and Wireshark to decrypt and capture the traffic from Windows Notepad to the Microsoft API. Peter Girnus has a very good writeup on how to use these tools at [https://www.petergirnus.com/blog/decrypting-https-traffic-with-mitmproxy-amp-wireshark](https://www.petergirnus.com/blog/decrypting-https-traffic-with-mitmproxy-amp-wireshark).

The network traffic starts with a few handshakes to establish the TLSv1.3 connection followed by a POST to /v1/notepad-cowriter/rewrite that contains a query JSON object. This is followed by a GET that contains the result JSON object. This is visible in Flow view for mitmproxy. 

![mitmproxy](/images/rewrite/mitmproxy.png)   

Bringing Wireshark into the mix gives us more visiblity into the packets in a little bit of an easier view compared to mitmproxy. 

![Wireshark](/images/rewrite/WiresharkFlow.png)   

### Post / Request
Diving into specific packets, we start with Packet 592 and examine the Header and Body.

![Header](/images/rewrite/Wireshark-HeaderSent.png)   

Some header information to call out specifically:

|Header|Notes|
|---|---|
|x-clientappversion| Contains the version of Windows Notepad which is 11.2412.16.0 in our case|
|ms-cv|Evidently the [Microsoft Correlation Vector](https://github.com/microsoft/CorrelationVector). Something to read up on later|
|x-clientappname|Application making the call which is Microsoft.WindowsNotepad. Can other apps call this API?|

![Body](/images/rewrite/Wireshark-BodySent.png)  

The body consists of a JSON object with the following Key/Value pairs:

|Key|Value|
|---|---|
|choiceCount|Appears to always be 3. This is the number of choices requested back from the API|
|format|Has the following possible values: Default, Paragraph, List, Business, Academic, Marketing, Poetry|
|length|Has the following possible values: Longer, Shorter, Write Similar|
|prompt|The text that was in Windows Notepad that is to be edited/changed|
|tone|Has the following possible values: Default, Formal, Casual, Inspirational, Humor, Persuasive|

### Get / Response
Packet 1261 is a response back with another JSON object with the following Key/Value pairs:

![Body](/images/rewrite/Wireshark-BodyReply.png)  

|Key|Value|
|---|---|
|texts|Contains an array of response texts|
|id|Internal ID # for the prompt transaction|
|status|Succeeded. May also have a status for a failed call? Possibly when out of AI credits or some "bad" prompt"|
|expectedEnd|NULL|
|retryAfter|NULL|
|freeCredits|Amount of AI Credits left|

## Conclusion

Nothing too wild or out of the ordinary when it comes to API calls. It is what I would expect them to send and recieve back. Going back to a previous post, none of this appears to be stored locally in any files. Checking the memory for any artifacts is another question all together. 

This Microsoft Correlation Vector sounds very interesting from my quick reading of the GitHub page. It sounds like it is primarily intended to assist in troubleshooting and debugging. Though same requirements usually translate very well into incident investigations. 

I still need to dig deeper into the PCAP file from Wireshark to see if there are any other interesting artifacts. Realistically, no one is really going to be running a MITM type proxy logging all their traffic to even catch this. 

Lastly, the conclusion to the story of Whiskers the cat will never be made public.