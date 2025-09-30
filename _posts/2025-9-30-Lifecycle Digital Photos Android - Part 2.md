---
layout: post
title: Lifecycle of a Digital Photo on a Android Pixel 7 - Part 2
author: 'ogmini'
tags:
 - Android
 - Google-Photos
 - EXIF
 - Timestamps
---

Continuing from the previous [post](https://ogmini.github.io/2025/09/27/Lifecycle-Digital-Photos-Android.html), I took some photos while the phone was in Airplane Mode to control when the photos were uploaded to Google Photos. I waited some time and disabled Airplane Mode and let the photos upload. I also wanted to see what happened if I just placed an image in the `/storage/self/primary/DCIM/Camera` location. Is there any sort of differentiation by Google Photos of images taken by the camera versus images just placed in the folder.

## Google Takeout

I pulled a Google Takeout for just the Google photos an examined what we got back.

![Takeout Structure](/images/android-photos/takeout_structure.png)

At the moment the three JSON files are empty. While the "Photos from 2025" folder contains all the photos and an associated JSON file. This JSON file contains various metadata attached to the image. This information is seperate from the EXIF metadata.

![Delayed Photo](/images/android-photos/delay-upload-photo.png)

Above is the JSON metadata for a standard photo taken by the Camera while Airplane Mode was enabled. Airplane Mode was disabled at a later time.

| Fields | Notes |
| --- | --- |
| creationTime | Generally matches up with the time the photo was uploaded to Google Photos. |
| photoTakenTime | This matches up with when the photo was taken. We will delve deeper into this in Part 3 |
| geoData | More testing is needed for this one. I uploaded the photos at the same location as they were taken. |
| geoDataExif | GPS Coordinates taken from the EXIF data. We will delve deeper into this in Part 3 |
| deviceType | This is potentially interesting and rather generic. |
| androidPackageName | What package created/took the photo. |

![Image](/images/android-photos/upload-image.png)

Above is the JSON metadata for just a random image placed into the `/storage/self/primary/DCIM/Camera` location.

| Fields | Notes |
| --- | --- |
| geoData | No information. |
| deviceType | This is potentially interesting and rather generic. |
| androidPackageName | What package created/took the photo. |

Take note that of the differences of the androidPackageName. This requires more testing but it appears that you can tell if the Camera took an image or otherwise. I did download the image via the Chrome browser on the phone.

I'm a little surprised at the geoData being all 0s for the random image. I had been theorizing that this field held the GPS data for where the phone was when it uploaded to Google Photos. Once again, more testing is needed.

## Next Steps/Questions

Start pulling/putting the information we have from Part 1 and Part 2 together. See if there are any correlations. In general, more testing is needed. We also haven't looked at the EXIF data yet.
