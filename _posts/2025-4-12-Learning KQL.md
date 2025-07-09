---
layout: post
title: Exploring KQL
author: 'ogmini'
tags:
 - KQL
---

Started looking into Kusto Query Language (KQL) in part due to a previous post/challenge on [Cloud Log Delays](https://ogmini.github.io/2025/04/02/David-Cowen-Sunday-Funday-Cloud-Log-Delays.html). I would consider myself proficient with writing SQL queries and it has been a very useful skillset in my current career. The ability to quickly query large structured databases to report on or search for information has been a boon. Being able to whip up a query to answer a question about students—like "Which students are enrolled in all the courses required for their major but have not yet completed any elective courses, and what is their expected graduation date based on their current course load?"—never fails to elicit stares of awe.

KQL is used to retrieve and analyze large datasets, mainly in Azure Data Explorer, Azure Monitor Logs, and Microsoft Sentinel. It’s designed for read-only queries and is optimized for performance, especially on time-series data like logs and telemetry. More information can be found at Microsof's documentation [https://learn.microsoft.com/en-us/kusto/?view=microsoft-fabric](https://learn.microsoft.com/en-us/kusto/?view=microsoft-fabric). For a person who knows SQL, I have found the [SQL Cheat Sheet](https://learn.microsoft.com/en-us/kusto/query/sql-cheat-sheet?view=microsoft-fabric) to be very useful.

Some things that I really like about KQL:

- Handling time with commands such as between, ago, bin.
- The pipe | chaining makes KQL queries much easier to read
- Handling of dynamic columns/JSON. This is incredibly hard/impossible to do in SQL.
- Have I mentioned how it handles time and queries with time based criteria?

I'm hopin to give the [https://detective.kusto.io/](https://detective.kusto.io/) a whirl at some point. I wonder if it is something I could do together with my kid. I leave you with this terrible KQL joke query given to me by ChatGPT:

> let humor = "Why did the SQL query go to therapy?";  
> let punchline = "It had too many joins!";
> print humor, punchline
