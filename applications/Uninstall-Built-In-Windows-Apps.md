---
title: Uninstall Built-In Windows Apps
description: This article will explain how to officially remove built-in apps from Windows 10 and up.
published: false
date: 2023-06-26T20:28:22.409Z
tags: intune, windows, store
editor: markdown
dateCreated: 2023-06-14T20:33:27.072Z
---

# How to uninstall built-in Windows apps
The only official way to remove built-in Windows apps is using the "Uninstall" assignment with Microsoft Intune. 
## Find the apps you want to remove
To do this, you must first find out the AppID or name of the application you want to remove. Both of the following methods can be performed without being an administrator on the target computer. In both cases, be aware of the following limitations. Some of these limitations can be worked around by using the methods below.

> Specific Microsoft Store apps may not be displayed and available in Intune. Common reasons an app doesn't appear when searching within Intune include the following:
{.is-warning}
> - The app is not available in your region
> - The app is not available if there is an age restriction
> - The app is a paid app, which is not supported
> - The app is an Android app
> - The app is a Microsoft Store for Business app that is not available publicly in the consumer store
> 
> Source:  https://learn.microsoft.com/en-us/mem/intune/apps/store-apps-microsoft#add-and-deploy-a-microsoft-store-app
{.is-warning}


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
To remove a store app using Intune, you just need to add a new application (see https://learn.microsoft.com/en-us/mem/intune/apps/store-apps-microsoft#add-and-deploy-a-microsoft-store-app) and assign it as uninstall to a group in your desired scope (device/user).

> Remember to select the install behaviour that matches the installation type for that application. System will uninstall the application for everyone using the target device. User will only uninstall it for a targeted user. **This selection can only be changed when the application is added. **
{.is-info}

## Addtional Considerations
The following should be considered as-is, meaning they quickly change and might be inaccurate.
### Removing built-in apps through other means
Over the years a lot of scripts have been released - even updated to this day, that claim to somehow debloat the operating system. Some of the built-in applications will not be available through either of these methods. ***This is intentional and it might cause problems later on.***

Especially removing apps like the store app itself is a very bad idea as it will completely break an admins control of applications through Intune. 