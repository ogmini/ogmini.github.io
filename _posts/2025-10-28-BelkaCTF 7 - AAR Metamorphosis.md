---
layout: post
title: BelkaCTF 7 - AAR Metamorphosis
author: 'ogmini'
tags:
 - CTF
 - Belkasoft
 - Writeups
---

![Task](/images/BelkaCTF7/Task15.png)

The suspect's body started acting weird... People(Is our suspect people?) often use health monitoring apps/devices connected to their phone. Heading back over to ALEAPP, I check out the Installed Apps (Vending) [https://forensafe.com/blogs/Android_InstalledApps.html](https://forensafe.com/blogs/Android_InstalledApps.html) and do a search for "health" to see what we find. It is important to remember that this search won't always work as there is no requirement for the package name to make sense. I got lucky here.

![com.xiaomi.hm.health](/images/BelkaCTF7/Task15-1.png)

From here, I start poking around the `\data\data\com.xiaomi.hm.health\databases` folder that is associated with the application. After opening various database files in DB Browser for SQLite, I find an interesting one called `origin_db_ef2042f8ec65801481fb688859d4e279` which has a table named "HEALTH_RATE" with TIME and HR columns. Appears that TIME is a timestamp and HR would be Heart Rate. Scrolling though the records a drop in Heart Rate is noticed at 1752756817 or 2025-07-17 12:53:57 UTC.

![Heartrate](/images/BelkaCTF7/Task15-2.png)

## Thoughts

Our suspect effectively died with a heart rate of 10 bpm. Belkasoft X is able to parse this artifact natively. Shortly after the CTF, support for this artifact was also added to ALEAPP by [its5q](https://github.com/its5Q) via [https://github.com/abrignoni/ALEAPP/pull/584](https://github.com/abrignoni/ALEAPP/pull/584).
