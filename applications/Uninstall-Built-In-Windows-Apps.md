---
title: Uninstall Built-In Windows Apps
description: This article will explain how to officially remove built-in apps from Windows 10 and up.
published: false
date: 2023-06-14T20:35:01.250Z
tags: intune, windows, store
editor: markdown
dateCreated: 2023-06-14T20:33:27.072Z
---

# How to uninstall built-in Windows apps
The only official way to remove built-in Windows apps is using the "Uninstall" assignment with Microsoft Intune. 
## Find the apps you want to remove
To do this, you first need to figure out the AppID or Name of the application you want to remove.

### Method 1 - Use the App Installer (aka Winget)

`winget search "Search term"`
### Method 2 - Use the store app
## Remove the app using Intune