---
layout: post
title: BelkaCTF 7 - AAR API Key
author: 'ogmini'
tags:
 - CTF
 - Belkasoft
 - Writeups
---

![Task](/images/BelkaCTF7/Task6.png)

Coming off the previous [task](https://ogmini.github.io/2025/08/05/BelkaCTF-7-AAR-Command-and-Control.html), we need to determine the API Key that the malware is using. Working off previous knowledge from the "Command and Control" task, we can skip some analysis steps/options. For completions sake, some other ways to find the hardcoded API Key might be to:

- Run strings on the malware and see if you can find a hardcoded API Key
- Dynamic malware analysis
  - Memory analysis
- ANY.RUN

## PEStudio / dnSpy

Refer back to [https://ogmini.github.io/2025/08/05/BelkaCTF-7-AAR-Command-and-Control.html](https://ogmini.github.io/2025/08/05/BelkaCTF-7-AAR-Command-and-Control.html) on using PEStudio/dnSpy to analyze the malware executable.

We will now examine the section of code that I had skipped previously. The malware author had attempted some obfuscation which would have hampered static analysis using strings. This is a classic example of XOR obfuscation using a hardcoded decryption key stored in the `text` variable. The array of integers is the encrypted API Key.

![dnSpy - Obfuscation](/images/BelkaCTF7/Task6-1.png)

I promptly dumped this code snippet into Visual Studio because I am far too lazy to do the XOR operations by hand. I added a console writeline to the end which resulted in this code:

~~~ C#
using System.Text;

int[] array = new int[]
    {
        16,
        23,
        104,
        28,
        6,
        16,
        105,
        20,
        16,
        30,
        28,
        119,
        105,
        96,
        41,
        78,
        92,
        75,
        6,
        77,
        19,
        27,
        117,
        123,
        126,
        69,
        40,
        99,
        120,
        71,
        48,
        81,
        78,
        126,
        74,
        65,
        68,
        116,
        60,
        113,
        13,
        110,
        58,
        111,
        64,
        127
    };
int num = 0;
StringBuilder stringBuilder = new StringBuilder();
string text = "(&^%4&^%$*&6";
foreach (int num2 in array)
{
    int num3 = (int)text[num];
    char value = (char)(num2 ^ num3);
    stringBuilder.Append(value);
    num = (num + 1) % text.Length;
}
Console.WriteLine(stringBuilder.ToString());
~~~

Running the code results in the API Key being written to the console.

![API Key](/images/BelkaCTF7/Task6-2.png)

## ANY.RUN

Uploading the malware to ANY.RUN for analysis provides the following reports:

- [https://app.any.run/tasks/ea338711-8bf4-49a1-b882-acacf01dee84](https://app.any.run/tasks/ea338711-8bf4-49a1-b882-acacf01dee84)
- [https://any.run/report/cd899279753d920be99e6ebeae756cc2e8efa651aa29a724174942adc52b262a/ea338711-8bf4-49a1-b882-acacf01dee84](https://any.run/report/cd899279753d920be99e6ebeae756cc2e8efa651aa29a724174942adc52b262a/ea338711-8bf4-49a1-b882-acacf01dee84)

We can see the Telegram Tokens found in the screenshot below.

![ANY.RUN](/images/BelkaCTF7/Task6-3.png)

## Thoughts

Again, this was a fun task as a developer as we get to rip someone's code apart. Many different ways to solve this and I would say the various automated malware analysis tools like ANY.RUN are probably the easiest/fastest and possibly safest. They will not get everything and very well written malware may not result in much information.
