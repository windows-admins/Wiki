---
title: Inject drivers into WinRE
description: This page will explain why this might be necessary and show how to do it. 
published: false
date: 2024-12-16T22:09:52.437Z
tags: windows, drivers, winre
editor: markdown
dateCreated: 2024-09-29T21:11:01.024Z
---

# WinRE Inject drivers
If you're on this page, you're most likely already aware of what you want to do, you just don't know how. This entry will make use of [this script](https://github.com/MHimken/WinRE-Customization) which is explained in [this blog](https://manima.de/2023/01/modify-winre-patches-drivers-and-cve-2022-41099/#adding-drivers).

## Mission statement
There are multiple situations where you might want to add drivers to a [WinRE image](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference) - not to be confused with Win**P**E. Here are the most common ones, during a recovery scenario (where you'd need the WinRE to recognize a specific device). You will notice that these drivers are usually related to storage, display, or network. 

- Hardware was added that requires drivers to be available (maybe an external display for a purpose built device).
- The UEFI was switched from AHCI to RAID, but resetting the device fails since. Some devices do this by default (Dell e.g.)
- You might want to troubleshoot a network connection, but the NIC isn't available.
- Autopilot reset fails, due to "missing" storage.
- Wipe fails, due to "missing" storage.

## How to inject drivers
1. Get [the script] (https://github.com/MHimken/WinRE-Customization)
2. Download your drivers. If you don't have any, the [WinPE drivers](https://www.dell.com/support/kbdoc/en-us/000107478/dell-command-deploy-winpe-driver-packs) from Dell is a good place to start. Be sure to unzip whatever format you have so that \*.inf files are available.
3. (optional, but recommended) Find out if your WinRE partition requires an extended size. If `diskmgmt.msc` shows less than 1GB, I recommend adding `-RecoveryDriveSizeInGB 2GB` along with the next step.
4. Run a PowerShell with administrative privileges and pass the necessary details to the script. For example `.\Patch-WinRE.ps1 -FilesDriver C:\Temp\WinPE11.0-Drivers-A05-TPKY4\winpe\x64` (don't forget step 3.)

That's it, let the script do its work and check the log. Adding all of the WinPE drivers from dell for Windows 11 takes up about 50MB of additional space.

