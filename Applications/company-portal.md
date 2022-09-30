---
title: Deploying Store apps during ESP
description: 
published: true
date: 2022-05-27T23:54:52.748Z
tags: 
editor: markdown
dateCreated: 2022-05-27T23:53:49.294Z
---

# Deploying Store apps during ESP

when deploying a store app during the ESP stage of autopilot using the offline version of the app is preferred, as this will install the app directly and prevent the store from updating all the provisioned apps, which can cause unpredictable delays during the ESP

we will use the company portal app as an example

## Enable offline apps

in the Microsoft Store for Business
1. click on manage
1. click on settings
1. in the shop tab, enable "show offline apps"


![enable_offline_companyportal.png](/enable_offline_companyportal.png)

## Adding the offline company portal

1. search the store for company portal
1. set the license type to offline
1. click on "get the app"

![companyportal-offline.png](/companyportal-offline.png)

Once the app syncs to intune, you can assign it to your autopilot devices groups.


https://docs.microsoft.com/en-us/microsoft-store/distribute-offline-apps#download-an-offline-licensed-app