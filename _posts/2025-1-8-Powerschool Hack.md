---
layout: post
title: Powerschool Hack
author: 'ogmini'
tags:
 - News
---

Yesterday news hit that Powerschool was hacked resulting in a threat actor stealing student and teacher data. Powerschool provides SIS platform software to K-12 schools. Bleeping Computer has a good article on the incident - [https://www.bleepingcomputer.com/news/security/powerschool-hack-exposes-student-teacher-data-from-k-12-districts/](https://www.bleepingcomputer.com/news/security/powerschool-hack-exposes-student-teacher-data-from-k-12-districts/).

As an IT professinal in the Higher Education world, this scenario is one of my fears. Whenever we have to integrate with a vendor application they often require lots of student data that solidly falls into the PII realm. Everything from something as simple as birthdates all the way up to student financial aid data and medical data. Just as an example, Powerschool is partnered with and integrates with [SNAP](https://www.powerschool.com/company/partners/snap-health-center/) to provide school nurses with all the data they need.  

I'm interested to see the incident report from Crowdstrike which should be released on January 17, 2022 and hopefully its released publicly. From what has been released publicly, it appears a Powerschool maintenance user account was compromised allowing the threat actor to export data from the systems. Evidently, this account is typically used by support to assist clients. A good reminder to disable or remove accounts that are not needed for regular business. A support account like this could be enabled when needed during troubleshooting.

Mass exports of data out of the system should be something that is carefully monitored and controlled. Reading between the lines, it also sounds like Powerschool paid the ransom to the threat actor to ostensibly delete the data.
