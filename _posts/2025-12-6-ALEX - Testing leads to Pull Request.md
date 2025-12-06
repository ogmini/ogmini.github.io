---
layout: post
title: Trying out ALEX 2.0 - Rooted Device Support Leads to Pull Request
author: 'ogmini'
tags:
 - Android
 - Python
 - ALEX
---

Christian Peter announced [https://www.linkedin.com/posts/christian-peter-49b4ab182_alex-aleapp-forensics-activity-7401344010373451776-Il3H](https://www.linkedin.com/posts/christian-peter-49b4ab182_alex-aleapp-forensics-activity-7401344010373451776-Il3H) the addition of support for rooted Android devices for ALEX - available at [https://github.com/prosch88/ALEX](https://github.com/prosch88/ALEX). I wanted to give the new functionality a try to see how it works as ALEX has been a great tool when doing research and testing.

My test setup:

- Windows 11 23H2 (I need to update this testing system)
- Pixel 7 (root)
- Python 3.14

Right off the bat, Filesystem Backup Method 1 resulted in an empty zip file for me. Which was a little puzzling and we'll come back to it. Filesystem Backup Method 2 worked as expected and results in a tar file. I did not test the Physical Backup as my test device is encrypted and I was more curious about Method 1 failing.

Checking the logs outputted by ALEX didn't really provide any useful information about the failure. Though, one can surmise that it was a non-fatal error or not caught. As far as ALEX was concerned it succeeded. I proceeded to edit the devicedump.py script [https://github.com/prosch88/ALEX/blob/main/alex/devdump.py#L132](https://github.com/prosch88/ALEX/blob/main/alex/devdump.py#L132) to show me the output of executing the remote script that was being used to pull the files. After line 132, I inserted:

``` sh
print(line) #debug out
```

This resulted in the following being printed to the console:

``` cmd
b'/data/local/tmp/devicedump.sh[4]: \r: inaccessible or not found\n'
b"/data/local/tmp/devicedump.sh[7]: syntax error: unexpected 'do\r'\n"
```

This had me stumped for a bit and I needed a night of sleep to get a clearer head. Learning new things and pushing boundaries of what I already know here. This is the exact reason for this blog and diving headfirst into new areas. Those of you well versed in shell scripts might already see the problem. I proceeded to write a quick test script just to verify that "do" was supported (it would be incredibly weird if it wasn't) and this would also help me remove as many variables from the problem as possible. The test script was as follows:

``` sh
#!/system/bin/sh

echo "Test Loop"

for i in 1 2 3 4 5; do
     echo "i = $i"
done

echo "Finish"
```

This gave me the same errors and I concluded there must be some "invisible" character messing stuff up... Windows vs Unix line endings! Low and behold, since I was writing the script on a Windows 11 machine there were CRLF instead of LF at the end of each line. ALEX must be having the same issue being run on a Windows 11 machine.

One option is to use sed to remove the CR and just leave the LF behind. As a quick test, I did this and it fixed the issue!

``` cmd
sed -i  's/\r$//' test.sh
```

The actual fix which I'll submit shortly should be at [https://github.com/prosch88/ALEX/blob/main/alex/devdump.py#L90](https://github.com/prosch88/ALEX/blob/main/alex/devdump.py#L90). That line should be changed to:

``` python
with tempfile.NamedTemporaryFile("w", delete=False, newline="\n") as f:
```

If you're interested in seeing the submitted issue for some reason. You can find it at [https://github.com/prosch88/ALEX/issues/1](https://github.com/prosch88/ALEX/issues/1). The PR is at [https://github.com/prosch88/ALEX/pull/2](https://github.com/prosch88/ALEX/pull/2)

Definitely learned a lot diving into the codebase for ALEX. I'm very comfortable with C# and less so with Python.
