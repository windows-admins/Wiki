---
title: Deploying Store Apps During ESP
description: 
published: true
date: 2023-08-21T18:37:09.563Z
tags: autopilot, intune, windows, mdm, windows store
editor: markdown
dateCreated: 2022-05-27T23:53:49.294Z
---

> **NOTE: The Microsoft Store for Business ~~will be~~ was [retired in Q1 2023](https://techcommunity.microsoft.com/t5/windows-it-pro-blog/evolving-the-microsoft-store-for-business-and-education/ba-p/2569423)!**
See [Update to Endpoint Manager integration with the Microsoft Store on Windows](https://techcommunity.microsoft.com/t5/windows-it-pro-blog/update-to-endpoint-manager-integration-with-the-microsoft-store/ba-p/3585077)
For more information about the replacing functionality follow this link: 
**[App-Installer](/intune/App-Installer)**
{.is-info}


When deploying a store app during the ESP stage of Autopilot, using the offline version of the app is preferred. This will install the app directly, and prevent the store from updating all of the provisioned apps, which can cause unpredictable delays during the ESP process.

We will use the Company Portal app as an example.

## Enable Offline Apps

In the Microsoft Store for Business

1. Click on Manage
2. Click on Settings
3. In the Shop tab, enable "Show offline apps"

![enable_offline_companyportal.png](/enable_offline_companyportal.png)

## Adding the offline Company Portal app

1. Search the store for "Company Portal"
2. Set the license type to "Offline"
3. Click on "Get the app"

![companyportal-offline.png](/companyportal-offline.png)

Once the app is synced to Intune, you can assign it to your Autopilot device groups.

When you assign the offline app don't forget to change the license type to device

![licensetype.jpg](/licensetype.jpg)

Source: ["Download and offline licensed app"](https://docs.microsoft.com/microsoft-store/distribute-offline-apps#download-an-offline-licensed-app)

