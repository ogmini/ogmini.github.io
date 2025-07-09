---
layout: post
title: Microsoft Edge - AutoFill Database
author: 'ogmini'
tags:
 - DFIR
 - Microsoft-Edge
 - AutoFill
---

I follow Phill Moore's great [This Week in 4n6](https://thisweekin4n6.com/) weekly roundup as it is an awesome way to learn and keep up to date with various cybersecurity topics. I highly encourage you to follow his weekly roundups. His work is greatly appreciated. Maybe we should bring back webrings?

While reading the Week 18 roundup, I came across an article from Reliance Cyber [https://www.reliancecyber.com/technical/the-good-the-bad-and-the-ugly-of-edges-autofill-databases/](https://www.reliancecyber.com/technical/the-good-the-bad-and-the-ugly-of-edges-autofill-databases/) about the Autofill database in Microsoft Edge. It served as a good reminder that more than just usernames, passwords, and addresses could be present in the database. I found it very interesting that they were able to find the prompts from ChatGPT! I like that they talk a little bit about WHY this information may end up in the database. As a developer, it is very easy to forget to use the relevant HTML attributes and this article serves as a good reminder why I shouldn't forget them. This is a definite weakness to applications like Microsoft Forms, Google Forms, etc that allow users to easily create forms. Many don't use those attributes and people might ask for a credit card # or SS# in a standard textfield.

I am intriged to know if tools like NirSoft's [BrowserAutoFillView](https://www.nirsoft.net/utils/web_browser_autofill_view.html) misses grabbing any information from the database. Reliance Cyber provides some queries that can be used in tools like DB Browser for SQLite. Maybe something worth testing. I'd need a good dataset though...
