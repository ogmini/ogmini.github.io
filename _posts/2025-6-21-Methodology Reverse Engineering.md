---
layout: post
title: My Methodology for Reverse Engineering Binary Files
author: 'ogmini'
tags:
 - Registryhive
---

I realized today that I was pretty vague in my previous post about HOW I personally go about reverse engineering binary files such as `settings.dat` and the previous tab/window state files for Windows Notepad. I would love to hear from others about their methodology or any tips/tricks they have. I feel like this is somewhat of an inexact science with many different paths to get the same result. Some people may try to decompile or reverse engineer the executable that is writing to the binary file. Others may perform various tests and note what changes are seen in the binary files. What follows below is my methodology:

1. Research
2. Examine
3. Test
4. Isolate
5. Repeat

## Research

In the case of the `settings.dat` file, I knew these were all associated with UWP or Windows Apps. So I ventured on over to Microsoft Learn to see what I could find in the developer documentation. At the same time, I looked for previous work by others that might give me insight or hints. I got lucky in this case and found how the `settings.dat` file is used along with how to program my own testing application.

Generically, this step involves reading documentation related to what you are trying to reverse engineer. Any little hint/tip could help in your understanding. Sometimes, you hit a goldmine and other times you find nothing.

## Examine

Fire up your hex editor of choice and look at the binary file. Look for strings that might give any hints at what might be stored in the file. Are there any patterns that appear to repeat themselves? Anything that looks like timestamps? What has changed or is changing? What hasn't changed.

In the case of the `settings.dat` file, I created a test application that just saved a string to the `settings.dat` file. This let me look for that string in the file and gave me an area to focus on. I make some assumptions about what the various hex values that are present might mean and stand for.  

## Test

Build up some theories on what this file may contain. Make changes to settings in the application you are working with and see what happens.

In the case of the `settings.dat` file, building off those assumptions I test them using a template file with highlighting. This template file at the moment will only work for the specific file.

## Isolate

Narrow down your focus by controlling as many variables as possible. If possible, allowing only one variable to change at a time will make it much easier to examine the binary file.

In the case of the `settings.dat` file, we need to isolate the change by using my test application to write a second string to the `settings.dat` file or even write a different length string. Each of these will need to be tested.

## Repeat

As you figure out each piece, repeat back at the Examine step. Don't be afraid to start on a variable and skip it to work on another. Often times, one variable is linked with another in some way that you won't discover until later. Take very good notes about what you changed and what happened. I find it is helpful to keep a history of all the binary files.

In the case of the `settings.dat` file, every assumption is tested and verified by making a change to the test application to see if the template file still holds true. Repeat until done.

## General Tips

- Using different Hex Editors can be handy.
  - ImHex more readily displays uLEB128 values while 010 Editor does not.
  - 010 Editor more readily displays FILETIME while ImHex does not.
- Code up your own test applications.
  - While looking at Windows Notepad, I had a simple brute force program to help me identify CRC32 Hash checks.
- Always go back and read documentation. Try to put yourself in the shoes of the developer. What would need to be stored to make this a useful file?
- Structures will be repeated in binary files.

## Conclusion

Hopefully someone finds this a little helpful. This isn't an insurmountable challenge. Granted, the more complex a binary file is the longer this might take. The ease of creating tests and being able to isolate variables is also key to the difficulty.

I would love to hear any tips/techniques/tricks/critiques from people. I'm still learning and every little bit helps.
