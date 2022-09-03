---
title: ConfigMgr .NET Requirements
description: 
published: true
date: 2022-04-18T17:12:12.520Z
tags: configmgr
editor: markdown
dateCreated: 2022-04-09T21:23:34.171Z
---

[.NET version requirements (Microsoft Docs)](https://docs.microsoft.com/mem/configmgr/core/plan-design/configs/site-and-site-system-prerequisites#net-version-requirements)

It is recommended that *all site system servers* run the latest version of .NET 4.

To easily check what version of .NET 4 is installed, run the following PowerShell command:

```powershell
(Get-ItemProperty "HKLM:\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full").Release
```

The value of “Release” should be ≥ 528040.

This is a test edit of the page.