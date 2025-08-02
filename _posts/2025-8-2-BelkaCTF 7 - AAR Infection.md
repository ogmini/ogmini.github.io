---
layout: post
title: BelkaCTF 7 - AAR Infection
author: 'ogmini'
tags:
 - CTF
 - Belkasoft
 - Writeups
---

![Task](/images/BelkaCTF7/Task4.png)

We need to find the email address of the person who spread the malware to the ATC machine. Time to start looking for emails!

## MemProcFS / Autopsy

You have two options here involving manually browsing the forensic folder in the mounted MemProcFS drive or feeding that folder into Autopsy. I'm going to detail using Autopsy; but those paths will directly map to the path in the forensic folder. After letting Autopsy do its thing you should end up with a similar view. Checking the E-Mail Messages under the Data Artifacts nets us some *.eml files and we find that the one we are looking for referencing the epxlorer.exe and giving a file size of 1,111,552 bytes.

![Autopsy](/images/BelkaCTF7/Task4-2.png)

![Autopsy - Email](/images/BelkaCTF7/Task4-3.png)

Checking the metadata for this artifact shows us the path to the file in the mounted MemProcFS drive.

~~~ cmd
M:\forensic\files\ROOT\Users\award\AppData\Local\Microsoft\Windows Live Mail\Posteo (ale 254\Inbox\ffffe6892a90cdc0-3FAF4916-00000007.eml
~~~

![Autopsy - Metadata](/images/BelkaCTF7/Task4-1.png)

If you were to manually check that folder in the mounted drive you would find two *.eml files and opening them in Notepad would net you the same information as above.

![Forensic Folder](/images/BelkaCTF7/Task4-4.png)

![Forensic Folder - Email](/images/BelkaCTF7/Task4-3.png)

## Volatility 3

Running the filescan plugin and looking for any *.eml files results in some hits.

~~~ cmd
vol -f BelkaCTF_7_CASE250722_KTSOAERO.mem windows.filescan
~~~

I'm going to cut out checking all of the hits and the one we are interested in is located at offset 0xe6892a6898e0. We next dump the files and open up `file.0xe6892a6898e0.0xe6892aa128b0.DataSectionObject.3FAF4916-00000007.eml.dat` in Notepad.

~~~ cmd
vol -f BelkaCTF_7_CASE250722_KTSOAERO.mem windows.dumpfiles --virtaddr 0xe6892a6898e0
~~~

Within that email, we find the some text referring to `epxlorer.exe` having a size of 1,111,552 bytes.

![Volatility - Email](/images/BelkaCTF7/Task4-3.png)

## Thoughts

Part of me still thinks this Task should have been before [Task 3/Sample](https://ogmini.github.io/2025/08/01/BelkaCTF-7-AAR-Sample.html). One thing that this Task really put into light is that the ability of MemProcFS to output a "forensic\files" folder can be very handy for browsing and analysis with other tools like Autopsy. It can speed up analysis compared to examining filepaths with filescan, dumping them with dumpfiles, and examining the files. I still have not had a chance to play around with [MemProcFS Analyzer](https://github.com/LETHAL-FORENSICS/MemProcFS-Analyzer) which was recommened to me by Kevin Pagano. I will definitely be using this CTF image as a test image.
