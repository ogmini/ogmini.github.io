---
layout: post
title: BSides NYC 0x05 - Contribute to Learn-Building DFIR Expertise Through Open Source
author: 'ogmini'
tags:
 - Talk
 - BSidesNYC
---

What follows is the "written" version of my talk at BSides NYC 0x05. I still find writing a better way to disseminate information. Talks are also fun for the person to person interaction. This writeup removes some of the fluff from the talk. 

[Powerpoint Slides](/files/BSidesNYC 0x05 Presentation.pptx) 

## Graduation Leads to Questions

After I graduated in 2023 with my Masters in Digital Forensics Science from Champlain College I was left with a few questions.

### Where do I go from here?

I wanted more information and knowledge about DF/IR. I wanted something that could combine my background in software development in a way that I could directly contribute back to the DF/IR field. 

### How can I really learn DF/IR?

There are many great ways to learn DF/IR such as books, courses, certifications, and CTFs. My coursework at Champlain College spanned so many topics from mobile forensics, malware, to legal considerations. It really served to bust the doors wide open on the world of DF/IR. I wanted to do it all and learn it all. I also started participating in Digital Forensic centered CTFs like BelkaCTF and the Magnet Virtual Summit CTF. There are many others; but, I've found it hard to always make the time to participate. They are a really great way to get exposure to realistic scenarios and datasets. 

### How can I network?

Networking is so important in any field or job. Attending conferences like BSides NYC is a great option. I'm very "shy" in real life and find still find it hard to approach people unless I've already made some connection with them. Another way of networking is starting to blog or make posts on sites like LinkedIn, Medium, or Youtube. There is a popular challenge called the [Zeltser Challenge](https://zeltser.com/2010-retrospective-on-my-security-blog/) coined by a friend named David Cowen. Who actually put this challenge out again to people in 2025 and I decided to give it a shot. It has not been easy. I have learned a lot though.

### How can I start contributing to the DF/IR field?

Open Source! Coming from a programming background, I had contributed to non-DF/IR related projects in the past. Why not contribute to DF/IR related projects?

## Open Souce

Many popular tools are open source or at least have a way to contribute to them. Such examples include:

- KAPE Files - which is the repository of targets/modules that make KAPE the tool it is. There is also the large collection of Zimmerman Tools from Eric Zimmerman.
- LEAPPS - the various LEAPPS such as ALEAPP, ILEAPP, etc from Alexis Brignoni
- Velociraptor - Developed by Michael Cohen/Rapid7 accepts contribution VQL Artifacts to its Artifact Exchange.  
- Wireshark - used in Network Traffic Analysis
- Ghidra - Reverse engineering

I imagine many of you have heard of least one of the above. If not, you know of other open source projects that you use. The question becomes, how many of you have contributed to them?

There are under 60 contributors to ALEAPP. KapeFiles and Velociraptor have a little over 115. This is a small group of individuals. You could be one of them.

### Transparent

Open source is transparent. You can see how and why the tool works. This is great for learning about artifacts. Where they come from, how they can be parsed or read, and how they should be understood. 

### Customizable and Agile

These projects can be updated or modified to handle one off or quickly evolving situations. You don't have to wait for a vendor to find the development to make an update. With enough knowledge, you could make the changes required in an open source project to add support or new features. 

### Collaboration

Open source projects offer a great way to collaborate with people around the world that you might not normally have the chance to work with. It is a great way to break out of your local bubble of coworkers or contacts.

## How Do I Get Involved

There are many different ways to contribute to open source projects but I'm going to focus on three.

### Documentation / Discussion

Documenting how to use a tool is always needed. The features and options need to be explained. Example use cases are also a great thing to have. 

You can also have discussions with other contributors about new features. You can help others with their questions. You can ask questions and get your own answers. I will add the caveat that you should also do as much of your own research as possible before asking a question. 

### Testing / Validation

Help test and validate tools by running them and documenting the results on your own datasets. Make sure that it works es expected. You can even collaborate with other contributors by helping them test and validate their work. More eyes the better.

### Pull Request

The final big one. Making a direct contribution to the repository. This could be in the form of fixing an issue or adding new functionality. It should go without saying that this requires doing the other two first. You must document, discuss, and test. 

## My A-Ha Moments

Going back a little, I started with my own project on Windows Notepad which resulted in publishing a tool and research to GitHub. The research grew out of collaborating with a few others across the world who were also doing similar research. It was at this point that I realized contributing to other projects would be a great way to answer my earlier questions I had after graduating. Since that time, I've conntributed to a few other projects that I had used or wanted to learn how to use.

- KAPE Files - I contributed Targets and Modules for things like ChatGPT and Windows Notepad.
- Velociraptor - I had open an issue to make a feature suggestion which resulted in adding support for LEB128 and ULEB128 to the binary parser.
- ALEAPP - An plugin that actually grew out of a BelkaCTF image which had IMAP email artifacts that were not initially supported. 
- CTF Challenges - Very recently I contributed some CTF Challenges to the BSides NYC CTFd. 

During all of this, I have had the pleasure of collaborating with DF/IR professionals around the world. I would never have been able to make these connections otherwise. 

Also, contributing to these projects required me to engage with the tools at a much deeper level than just using the tool. You have to fully understand the artifacts and their nuances to make those contributions. 

## Tips To Get Started

You don't need to be a developer to get started. It can be handy and will be a useful skill to have in DF/IR. I would encourage you to at least pick up some Python.

Often documentation is an area that is out of date or in need of improvement. This is crucial part of any project and a great place to start for anyone. In order to write good documentation, you need to know how to use the tool! That requires learning it and using it to get experience. I would heavily encourage the submission of documentation to the project itself and not to just write a post on how to use the tool. 

You can also document the existing plugins that parse out artifacts. This could just be linking to existing research and posts. If they don't exist, that provides an opportunity to do that research, provide the writeup, and add that documentation.

Another thing you can do is participate in discussions and read issues and pull requests. Make sure to read CLOSED issues and pull requests. There is a lot to be learned from things that have already been done, asked, or explained. You might see someone post a question and you know the answer. Conversely, you might learn something new from someone else's question and answer. If you see the same question asked over and over again you could write an FAQ.

One last tip is helping to validate and test tools on various datasets. You can create and document your own datasets to run the tool against. You can try to replicate someone else's results. This is so vital when new versions of software come out. Things change and the original author of the plugin might not be aware. Reach out to them and let them know. You could even help them update the plugin. This is just another way of learning and networking. 

## Call To Action

Get in there, pick an open source project that you use a lot, want to learn, or find interesting. Do the work and hit that "New Pull Request" button. That is all it takes to start contributing, learning, and being part of the community. 

Two opportunities that I'd like to highlight is the call by Andrew Rathbun to help to add documentation to the various KAPE File plugins. As I alluded to earlier, this could just be adding links to existing writeups and posts. 

The other one is contributing to the Artifact Exchange for Velociraptor. 