---
layout: post
title: Windows Store/UWP Application Data - Versioning
author: 'ogmini'
tags:
 - UWP
---

While rereading the Microsoft Documentation on App Settings located [https://learn.microsoft.com/en-us/windows/apps/design/app-settings/store-and-retrieve-app-data#versioning-your-app-data](https://learn.microsoft.com/en-us/windows/apps/design/app-settings/store-and-retrieve-app-data#versioning-your-app-data) I took note of the ability to version application data. This is an optional feature and not a requirement but would be very useful if a program made changes to the structure of how application data is stored. Quoting the documentation:

> You can optionally version the app data for your app. This would enable you to create a future version of your app that changes the format of its app data without causing compatibility problems with the previous version of your app. The app checks the version of the app data in the data store, and if the version is less than the version the app expects, the app should update the app data to the new format and update the version.

The obvious action after reading this is to test and see how the version shows up in the `settings.dat`! Again, I made a quick change to my test application:

~~~ c#
public async Task SetAppDataVersionAsync()
{
    ApplicationData appData = ApplicationData.Current;

    uint desiredVersion = 1337;

    await appData.SetVersionAsync(desiredVersion, new ApplicationDataSetVersionHandler(SetVersionHandler));
}
~~~

This results in the following:

![VK](/images/registry/applicationdata_versioning.png)

In short, if a Windows Store/UWP application utilizes the versioning feature in its Application Data you will see a VKCell with a Name of "Version" and a DataOffset equal to the version number which in my case was "1337". Remember, this is optional so ths VKCell may not exist. If anything, its existence points to a very forward thinking application and may be useful in interepreting the Application Data as behaviour may change between verisons. 