---
layout: post
title: Magnet Virtual Summit 2025 CTF - AAR "DAdataTA"
author: 'ogmini'
tags:
 - CTF 
 - Challenges
 - Writeups
---

![Stegosaurus running CLI commands](/images/memes/steghide.jpg)   
That stegosaurus hid something from us!

Continuing with my writeups on my "fails" or the ones I just couldn't figure out in the timeframe alloted. I want to talk about how I went about trying to solve the challenge and where I went wrong. This should help me in the future by highlighting weaknesses and areas for improvement. Each post will focus on just one "fail" challenge. You can find all my writeups [here](https://ogmini.github.io/ctf).

## DAdataTA

Title: DAdataTA      
Description: What does the file sucess.txt.txt contain?

This challenge was under the Windows 11 section and worth 75 points making it the most difficult challenge. 

### My Process

- I knew from looking at the PowerShell command history that sdelete.exe had been run on the file so recovering its contents in a standard fashion would not be easy or straightforward. [https://ogmini.github.io/2025/02/25/AAR-Out-of-the-Ordinary.html](https://ogmini.github.io/2025/02/25/AAR-Out-of-the-Ordinary.html) 
- Was a copy kept somewhere else? Is it possible the text editor kept a cache of the contents?
    - Searching for other text files proved unfruitful. 
- DAdataTA hint seems to point towards information being contained in something else. 
    - Is this pointing at some sort of data stream thing? sdelete.exe would have thoroughly wiped that file.
- Analyze the Tab State files for Windows Notepad using my [Windows Notepad Parser](https://github.com/ogmini/Notepad-State-Library)
    - I doubted this would give me anything as the more recent versions of Windows Notepad removed keeping the full contents of any files opened in the Tab State files. Sidenote: I need to finish that regression testing and documenting the version changes. [https://ogmini.github.io/2024/11/27/Microsoft-Store-Older-Versions.html](https://ogmini.github.io/2024/11/27/Microsoft-Store-Older-Versions.html)
    
#### Details from Windows Notepad Parser

This was the first time I've been able to run my Windows Notepad Parser against artifacts that I didn't create and knew nothing about and this gave me a wonderful opportunity to see what it could tell me. After extracting the relevant files from the image, my tool output the following CSV files:

- FileTabs.csv
- StateTabs.csv
- WindowStateTabs.csv

##### FileTabs.csv

![FileTabs.csv](/images/DAdataTA/FileTabs.png)   

Examining the CSV file using Timeline Explorer reveals that four different files have been opened with Windows Notepad along with their filepaths. Unfortunately, the only other interesting piece of information we can recover is the position of the cursor and any selected positions. This can be seen in the screenshot below where the Selection Start/End Index for the sucess.txt.txt file is 11 and 11. The other three files are 0 and 0. We can potentially extrapolate this to mean that the first three files were only opened and examined or read and the cursor was never moved. The sucess.txt.txt file had text typed into it that was 11 characters long and subsequently saved to the disk resulting in the cursor being left at position 11 or the end of the file. As we'll see in the solution below, this proved to be true.  

![FileTabs.csv - Selection](/images/DAdataTA/Selection.png)   

##### StateTabs.csv

![StateTabs.csv](/images/DAdataTA/StateTabs.png) 

The StateTabs.csv file confirms the Selection Start/End Index.

##### WindowStateTabs.csv

![WindowStateTabs.csv](/images/DAdataTA/WindowStateTabs.png) 

Looking at the WindowStateTabs.csv file, the highest Sequence Number indicates which state is the latest. Initially they had three files open with the sucess.txt.txt tab being active. We can tell this by looking at the Tabs List and the Active Tab. The list uses 0 based indexing. Later, they opened the GC365HN.gpx file and it became the active tab. Additionally, the Windows Notepad window was  

![PartialReconstruction](/images/DAdataTA/PartialReconstruction.png) 

### Actual Solution

The finale of KALI LINUX! The third and final challenge building upon the [first](https://ogmini.github.io/2025/02/24/AAR-A-Shadow-of-the-Real-Thing.html) and [second](https://ogmini.github.io/2025/02/25/AAR-Out-of-the-Ordinary.html). I was doomed from the start. All three of the Windows 11 Challenges that I failed to solve required knowledge or looking at the Kali Linux artifact.

During my [pre-analysis](https://ogmini.github.io/2025/02/12/Magnet-CTF-Pre-Analysis.html), I had made note of the crow.jpg file as being seemingly out of place and rather random. Another missed link. From the [Out of the Ordinary](https://ogmini.github.io/2025/02/25/AAR-Out-of-the-Ordinary.html) challenge, it is known that [steghide](https://steghide.sourceforge.net/) was used to hide the sucess.txt.txt file in the crow.jpg image. The password used is not known though or readily apparent. There is an important.pdf with the phrase "ihateruth" located on the system and this ended up being the password to recover the file. The recovered sucess.txt.txt file contained the phrase "marywuzehere" and being 11 characters long matched my findings using Windows Notepad Parser. 

### Lessons Learned

1. Other artifacts can help verify findings. It was really nice to see that the answer was 11 characters long. 
2. Look at your pre-analysis notes and make note of what you haven't used. Remember the [Duck Test](https://en.wikipedia.org/wiki/Duck_test). 
3. Don't get fixated! I got fixated on Windows 11 and continuted to ignore the Kali Linux artifact.
4. Mistakes can compound and later challenges will often rely on previous knowledge.
