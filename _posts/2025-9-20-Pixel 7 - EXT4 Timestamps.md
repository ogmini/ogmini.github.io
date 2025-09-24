---
layout: post
title: Pixel 7 - Timestamps / EXT4
author: 'ogmini'
tags:
 - Android
 - Root
---

As I was poking around the filesystem of my Pixel 7 using adb shell, I was interested in examining the timestamps associated with the various files. In particular, I was looking at the image files taken by the built in camera and I wanted to see if I could correlate the filesystem timestamps with the EXIF timestamps. It appears that Android 16 uses some flavor of EXT4 and I set about researching EXT4 timestamps and came across this handy article - [https://righteousit.com/2024/09/04/more-on-ext4-timestamps-and-timestomping/](https://righteousit.com/2024/09/04/more-on-ext4-timestamps-and-timestomping/). The next question is if this translates over to Android 16.

I attempted to replicate similar steps as in the above article using the following commands:

- `stat`
- `debugfs`
- `istat`

Unfortunately, the binaries for `debugfs` and `istat` were not present and the `stat` command does not show the BirthTime or CreationTime. This is where I stopped my journey down this rabbit hole. My next steps are to get a DD image of the partition and see if I can mount it on a system with debugfs. Or parse it manually if I'm insane. Honestly, I don't know if I should be going down this rabbit hole at the moment. I may revisit this in the future.

## References

- [https://righteousit.com/2024/09/04/more-on-ext4-timestamps-and-timestomping/](https://righteousit.com/2024/09/04/more-on-ext4-timestamps-and-timestomping/)
- [https://www.forensicfocus.com/articles/linux-timestamps-oh-boy/](https://www.forensicfocus.com/articles/linux-timestamps-oh-boy/)
- [https://www.hashimtech.com/blogs/mobile-forensics/android-mobile-forensics/android-file-system](https://www.hashimtech.com/blogs/mobile-forensics/android-mobile-forensics/android-file-system)
