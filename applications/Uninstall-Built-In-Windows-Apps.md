---
title: Uninstall Built-In Windows Apps
description: This article will explain how to officially remove built-in apps from Windows 10 and up.
published: false
date: 2023-06-26T19:52:03.804Z
tags: intune, windows, store
editor: markdown
dateCreated: 2023-06-14T20:33:27.072Z
---

# How to uninstall built-in Windows apps
The only official way to remove built-in Windows apps is using the "Uninstall" assignment with Microsoft Intune. 
## Find the apps you want to remove
To do this, you first need to figure out the AppID or Name of the application you want to remove. Both methods can be run without being an administrator on the target machine.

### Method 1 - Use the App Installer (aka Winget)
*Winget* should be automatically available on Windows for this purpose. You can easily search for an application by doing `winget search "APPNAME"`. Here's an example of what this looks like.
![2023-06-26_21-19-34.png](/2023-06-26_21-19-34.png)
From this, you copy the ID and continue with the next step.
### Method 2 - Use the store app
The *Microsoft Store* App provides similar functionality. Using the store provides the ability to search for the app in the OS display language. For example 'Company Portal' becomes "Unternehmensportal" using a de-DE language.

Once you found the app you're looking for, use the 'share' funtionality and select "copy link". The ID you need is in the URL, that is now in your clipboard. 
The URL for the Company Portal is "https://www.microsoft.com/store/productId/9WZDNCRFJ3PZ" so the ID would be ***9WZDNCRFJ3PZ***
![microsoftstore-sharelink.png](/microsoftstore-sharelink.png)
## Remove the app using Intune
To remove a store app using Intune, you just need to add a new application 