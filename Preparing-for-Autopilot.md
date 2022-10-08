---
title: Preparing for Autopilot
description: 
published: true
date: 2022-10-08T18:53:47.332Z
tags: 
editor: markdown
dateCreated: 2022-10-08T18:36:39.937Z
---

# Intro

Autopilot requires some sigificant shifts in processes and logistics. It is a complete shift in device management. Getting your Department and existing machines ready can be a sigificant challenge. This guide will hopefully prepare you for the shift.

In order to get the most out of this post you must have a precursory knowledge of
- What is Autopilot
- Why Autopilot is right for your Business
- The Difference between Hybrid AD Join and Azure AD Join
- (optional) Creating App registrations in Azure AD

## New Device Registration
Autopilot is a factory to user solution. In order to make full and proper use of autopilot you must speak with your PC vendor so that new devices are purchased Autopilot registered and ready. This process is usually straightforward. Most manufacturers such as Dell or HP offer this service for free. Most VARs offer this service for under $10 a machine. Most VAR represenatives know what Autopilot is. I've experienced a few completely waffle on how to proceed with the process.
Below is a list of vendors with expected pricing and difficulty with registration. Please be aware this is a community run list. It holds no weight or expectation of accuracy.

------
 Dell  | Easy |$0
 Pc Connections | Easy |$5-8
 Data Center Warehouse | Easy |$0-10
 GHA-Associates | difficult - Rep didn't understand the process| Unknown
 CDW |Easy? | $25
 
 ----
## Existing device registration
There are many ways to register your existing devices such as 

- The Get-WindowsAutopilotInfo script
- Gathering them from CM
- using the AutopilotConfigurationFile.json
- Having your existing vendor upload already purchased devices

Let's break down each of them

### Get-WindowsAutopilotInfo
The Get-WindowsAutopilotInfo script is a tool that can be used to collected the nessisary information for autopilot as a csv or upload it directly to autopilot with the -online flag.
To install the script
```powershell
Install-Script Get-WindowsAutopilotInfo
```

To collect CSV
```powershell
Get-WindowsAutopilotInfo -OutputFile
```

To transmit online
```poweshell
Get-WindowsAutopilotInfo -Online
```
To transmit online without Interactive login
```powershell
Get-WIndowsAutopilotInfo -Online -Appid -AppSecret
```
If you do use this method please becareful to limit your apps
permissions.

### Gathering from CM
Someone Please fill In

### AutopilotConfigurationFile.json
An offically supported method of autopiloting existing devices is to collect and place a json file in `C:\windows\provisioning\autopilot` With the name `AutopilotConfiguration.json` on a device before a device is powered on and enters the Out of Box Experience. Microsoft has a great guide [here](https://learn.microsoft.com/en-us/mem/autopilot/existing-devices) on grabbing the autopilot configuration file from your tennant.
You'll need to add this to your Image
This can be done with OSD/MDT
 - [Microsoft Create a CM package](https//learn.microsoft.com/en-us/mem/autopilot/existing-devices)
 - [peter vanderwoude Offline Windows Autopilot Deployment](https//www.petervanderwoude.nl/post/offline-windows-autopilot-deployment-profile/)
 
 A few community members have built One and Done tools. This can be handy if you need to get a branch office / client reimaged to a known good state and running on Autopilot and don't have SCCM at your disposal.
 [Ben from Intune.Training Intune USB Creator](https://github.com/tabs-not-spaces/Intune.USB.Creator)
 [awlnx WindowsAutopilotPrep](https://github.com/awlnx/WindowsAutopilotPrep)

Although this method is offically supported. It's less preferred. It still can be your best option in many cases but there can be issues with Autopilot profile reassignment down the road and your devices autopilot profile name on the hardware tab will be Autopilot-offlineGUID rather than a nice profile name to target. This will only register the device for future Autopilot use if your profile has Convert all targeted devices on.
### Vendor Registration
Here is a list of known vendors who,if you purchased your devices through them, can help you import your existing devices.

- example.com