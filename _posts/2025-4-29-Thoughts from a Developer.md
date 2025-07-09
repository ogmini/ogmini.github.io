---
layout: post
title: Thoughts from a Developer on the Truth in Data Podcast
author: 'ogmini'
tags:
 - Musings
---

I've been listening/watching Hexordia's Truth in Data Podcast on Youtube and the latest [episode](https://www.youtube.com/watch?v=Yy0c9FThsGQ) really resonated with my personal reasons for exploring DFIR. Jessica Hyde, Kim Bradley, and Debbie Garner interviewed Alexis Brignoni about open source code and how it relates to forensics. I'm going to pick out a few tidbits that jumped out at me. I do highly encourage everyone to give it a listen/watch. It is also on various podcast platforms in addition to Youtube.

## Benefits of Open Source

At [9:07](https://youtu.be/Yy0c9FThsGQ?t=547), Alexis talks about one of benefits of going open source is the ability for others to check your work. He also touches lightly upon its impact on being able to validate/verify the findings of a tool. There are of course ways to validate/verify the findings of closed source tools; but it can make the process more labor intensive. If you are able to read/understand the code, you can see exactly what digital artifacts it is looking at and how it is interpreting it. You might even be able contribute to the project with patches/changes/improvements. During one of my classes at Champlain, we had a pretty big discussion about the pros/cons of forensic tools that are open or closed source. I still contend that open source tools are preferable to closed source whenever possible. This should be its own post for the future though...

## Mindset of the Suspect / Coder

At [24:07](https://youtu.be/Yy0c9FThsGQ?t=1447), they talk about the usefulness of understanding the mind of a coder. As investigators, the importance of understanding the mindset of your suspect can be critical in an investigation. What is the suspects motivation? What is their background? All of these can be correlated back to a coder/developer who is writing an application that you want to extract digital artifacts from. What frameworks did they use for this program? Do they follow any coding conventions? These questions are much easier to answer if you have a coding/development background. You'll know shortcuts that they might take in storing information in data structures because you've done it before. I'll even go so far as to say realizing the difference between a coder writing an application for Microsoft versus a small solo project will help you find and parse those digital artifacts. Someone working for Microsoft will have to follow various coding conventions and rules. A solo coder can do whatever they want and that could include saving data in the oddest places in the oddest ways.

## Developer Gap

At [25:32](https://youtu.be/Yy0c9FThsGQ?t=1532), Jessica asks Debbie about the consideration about coding knowledge when hiring in the field. Again, I would highly encourage you to listen to the whole podcast. I will say that this gap is exactly what drew my interests to the DFIR field. I want to bring my background in development and apply it to DFIR. I've always wondered, why can't we just ask the developers how/why things are stored the way they are. Alternatively, could we ask them to add in useful DFIR capability or logging without compromising security and privacy. Understanding how to code is such an important and useful skill. With the pace at which technology changes you cannot solely rely on a tool that was developed ten years ago and only gets updates every quarter. Just to give a personal example from my research into Windows Notepad. If a closed source tool was being used to analyze the state files it would have likely broken or possibly reported erroneous data when the version changed to 11.2407.9.0. Contrasted to an open source tool and someone with some coding experience could see why it broke or reported erroneous data. They could even possibly fix the issue.  
