---
layout: post
title: Investigating Visual Studio Code - Part 3
author: 'ogmini'
tags:
 - DFIR
 - Visual Studio Code
---

Continuing from [Part 2](https://ogmini.github.io/2025/02/16/Investigating-Visual-Studio-Code-Part-2.html), the existence of the History folder threw me for a bit of a loop as I use other tools such as Git or TFS to handle versioning and history. There is a setting under Workbench related to Local History that is enabled by default. You can see the options in the screenshot below. 

![Settings - Local History](/images/visualstudiocode/Settings-LocalHistory.png)

I would heavily suggest people turn this feature off or at least setup exclude patterns. People already regularly leak secrets by accident via Pull Requests or Commits such that scanners exist to catch this - [https://gitleaks.io/](https://gitleaks.io/). The Local History feature by default would be saving copies of files that may contain secrets like passwords, private keys, etc. In the scenario below, I'll illustrate the potential danger and usefulness of this feature for DFIR.

## Scenario

I created a text file named "SecretConfig.txt" using Visual Studio Code and saved it to the Desktop. The contents of the file were "SuperDuperSecretPassword: CHANGEME" as seen in the screenshot below:

![Initial Password](/images/visualstudiocode/InitialSecret.png)

I reopened Visual Studio Code and edited the file to change the SuperDuperSecretPassword to "NoOneW1llGuessThis!". I closed Visual Studio Code without saving the file. 

I again reopened Visual Studio Code, saved the changes to the file, and closed Visual Studio Code. 

![Middle Password](/images/visualstudiocode/MiddleSecret.png)

I again reopened Visual Studio Code, changed the SuperDuperSecretPassword to "B3tt3rPassw0rd!", and saved the file.

![Last Password](/images/visualstudiocode/FinalSecret.png)

## Artifacts

Refer back to [Part 2](https://ogmini.github.io/2025/02/16/Investigating-Visual-Studio-Code-Part-2.html) and [Part 1](https://ogmini.github.io/2025/02/15/Investigating-Visual-Studio-Code.html) for specifics on files and file locations. 

As we were making changes to the "SecretConfig.txt" file, a file was created in the Backup folder at the following path:

`%appdata%\Code\Backups\1740273425117\file\777d2835`

The name of the folder 1740273425117 is a Unix Timestamp and actually corresponds to the time of when Visual Studio was first launched. I have yet to determine how the name of the file 777d2835 is determined. The contents is very interesting though.

![Backup Contents](/images/visualstudiocode/Backup.png)

We can see the filepath, a JSON object, and contents of the file. 

If we zip over to the History folder at the conclusion of our Scenario we find the following path:

`%appdata%\Code\User\History\file\~6a68f661`

I have yet to determine how the name of the folder is determined. This folder contains files related to the Local History of our "SecretConfig.txt" and we see the following files:

- entries.json
- t75U.txt
- Em27.txt
- SwdK.txt

Examining `entries.json` shows us a handy JSON file with with entries that chronologically order the other files with timestamps and IDs. There is also a handy source for the first entry that lets us know the file was created by Visual Studio Code. 

![entries.json](/images/visualstudiocode/Entries-JSON.png)

Opening those files reveals the change history of this file overtime as seen below.

![t75u.txt](/images/visualstudiocode/t75U.png)

![Em27.txt](/images/visualstudiocode/Em27.png)

![SwdK.txt](/images/visualstudiocode/SwdK.png)

I feel like it is pretty clear how this would be useful to investigators and how it could be leveraged by an attacker. I still have more to research on Visual Studio Code. Namely those SQLite databases and looking for more interesting related artifacts. 