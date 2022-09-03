---
title: Intune App Testing
description: 
published: true
date: 2022-05-28T10:41:50.615Z
tags: 
editor: markdown
dateCreated: 2022-04-15T22:42:22.195Z
---

# Intro

lorem ipsum


## resetting the IME registry

when testing win32 app deployments in intune, the intune management extension only tries 3 times per 24 hours, when testing this quickly becomes very tedious.

the code example below resets the state of app deployments, forcing the IME service to resync everything.
> 
> this should not be run on a production device
{.is-danger}


### Code
```powershell

#SCRIPT PROVIDED BY ANDREW JIMENZ(PATCHMYPC)
Stop-Service -Name IntuneManagementExtension
Get-Item -Path registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\IntuneManagementExtension\win32App* | Remove-Item -Force -Recurse
Restart-Service -Name IntuneManagementExtension
```

## Running as system

When deploying scripts or apps, you can choose between executing the action as either the logged in user or as the system.
![script-context.png](/script-context.png)

if you want to test out an app install or a script as the system account, you will need to download [Psexec](https://docs.microsoft.com/en-us/sysinternals/downloads/psexec) first
open a cmd prompt as an administrator
> c:\psexec.exe -sid cmd

this will open a cmd that runs as the system account. which will make testing silent installs very easy
