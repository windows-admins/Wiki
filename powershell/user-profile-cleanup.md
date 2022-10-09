---
title: User Profile Cleanup
description: 
published: true
date: 2022-10-05T15:30:36.253Z
tags: 
editor: markdown
dateCreated: 2022-10-05T15:30:28.608Z
---

PowerShell can be used to easily automate user profile cleanup, via CIM or WMI, in order to delete both the profile folders themselves, along with the registry information associated with them.

## CIM

```powershell
Get-CimInstance -ClassName Win32_UserProfile |
    Where-Object -FilterScript {
        ($_.Special -eq $false) -and ($_.LocalPath -eq 'C:\Users\USERNAME')
    } |
    Remove-CimInstance
```

## WMI

```powershell
Get-WmiObject -Class Win32_UserProfile |
    Where-Object -FilterScript {
        ($_.Special -eq $false) -and ($_.LocalPath -eq 'C:\Users\USERNAME')
    } |
    Remove-WmiObject
```
