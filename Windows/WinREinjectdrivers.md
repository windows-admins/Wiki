---
title: Inject drivers into WinRE
description: This page will explain why this might be necessary and show how to do it. 
published: true
date: 2024-12-16T22:15:11.370Z
tags: windows, drivers, winre
editor: markdown
dateCreated: 2024-09-29T21:11:01.024Z
---

# WinRE Inject drivers
If you're on this page, you probably already know what you want to do, you just don't know how. This post will use [this script](https://github.com/MHimken/WinRE-Customization), which is explained in [this blog](https://manima.de/2023/01/modify-winre-patches-drivers-and-cve-2022-41099/#adding-drivers).

## Mission statement
There are several situations where you might want to add drivers to a [WinRE image](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference) - not to be confused with Win**P**E. Here are the most common ones, during a recovery scenario (where you'd need WinRE to recognize a specific device). You will notice that these drivers are usually related to storage, display, or ethernet. 

- Hardware has been added that requires drivers to be available (perhaps an external display for a purpose-built device).
- UEFI has been changed from AHCI to RAID, but the device fails to reset. Some devices use this by default (e.g. Dell).
- You're trying to troubleshoot a network connection, but the NIC is not available.
- Autopilot reset fails due to "missing" storage.
- Wipe fails due to "missing" storage.

## How to inject drivers
1. Get [the script](https://github.com/MHimken/WinRE-Customization)
2. Download your drivers. If you don't have any, the [WinPE drivers](https://www.dell.com/support/kbdoc/en-us/000107478/dell-command-deploy-winpe-driver-packs) from Dell is a good place to start. Be sure to unzip whatever format you have so that \*.inf files are available.
3. (optional, but recommended) Find out if your WinRE partition requires an extended size. If `diskmgmt.msc` shows less than 1GB, I recommend adding `-RecoveryDriveSizeInGB 2GB` along with the next step.
4. Run a PowerShell with administrative privileges and pass the necessary details to the script. For example `.\Patch-WinRE.ps1 -FilesDriver C:\Temp\WinPE11.0-Drivers-A05-TPKY4\winpe\x64` (don't forget step 3.)

That's it, let the script do its work and check the log. Adding all the WinPE drivers from Dell for Windows 11 takes about 50MB of extra space and runs for less than 3 minutes. The log starting with `Patch_` will tell you if everything went as planned.

