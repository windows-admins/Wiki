---
title: .NET Requirements
description: 
published: true
date: 2022-09-30T11:28:29.641Z
tags: configmgr
editor: markdown
dateCreated: 2022-04-09T21:23:34.171Z
---

[.NET version requirements (Microsoft Docs)](https://docs.microsoft.com/mem/configmgr/core/plan-design/configs/site-and-site-system-prerequisites#net-version-requirements)

It is recommended that **all site system servers** run the latest version of .NET 4.8.

To easily check what version of .NET 4 is installed, run the following PowerShell command:

```powershell
(Get-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full").Release
```

The value of “Release” should be ≥ 528040.
