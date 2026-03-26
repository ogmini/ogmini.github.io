---
layout: post
title: Credential Phishing
author: 'ogmini'
tags:
 - IncidentResponse
 - Phishing
---

Came across this credential harvesting site located at hxxps://login[.]thehead-agency[.]com. I believe it has been taken down already. It was setup to mimic the standard Microsoft login page. Just fair warning, I am no expert at this sort of thing. Any feedback/critiques would be greatly appreciated on how you would investigate this. I also did this with very little time (~30 minutes) for setup and very spur of the moment.

The domain was registered via Tencent with redacted registrant information:

![registration.png](/images/thehead-agency/registration.png)

It used Cloudflare for the DNS:

![cloudflare.png](/images/thehead-agency/cloudflare.png)

Record was created on 2025-11-06 20:07:25 UTC

## Login Flow

What follows is me playing with the phishing site on a isolated VM. I regret that I wasn't running any traffic captures as this was very spur of the moment. And any further analysis is now not possible since the site appears to be down now. I did take a video and grab the HTML source of the various pages. I have not looked at the source extensively; but I'll call out some quick things I noticed.

First, screenshots of the flow:

Human Verification
![human.png](/images/thehead-agency/human.png)

Notice the URL with the email of test@test.com. 

Login Username
![username.png](/images/thehead-agency/username.png)

The username was populated automatically with test@test.com

Account Selection
![account.png](/images/thehead-agency/account.png)

To be clear, test@test.com is NOT a real email address as far as I know. It is funny that it purports to pick up a work and personal account. After selecting one you are prompted for the password.

Password
![password.png](/images/thehead-agency/password.png)

Entering a bad password still results in "success" and it quickly redirects you to:

hxxps://login[.]thehead-agency[.]com/common/login

Stolen
![stolen.png](/images/thehead-agency/stolen.png)

After that, you are unceremoniously dumped at a page that looks like the stereotypical https://office.com. It actually is still a fake and the login button at the top right sends you right back to the beginning. I regret not getting the source for this page.

![final.png](/images/thehead-agency/final.png)

## Quick Analysis of the Pages

- Credentials get posted to "urlPost":"https://login.thehead-agency.com/common/login"
- References legitimate javascript libraries from https://aadcdn.msauth.net

I ended up reporting this to Cloudflare Abuse.