---
title: Win32 App Testing
description: 
published: true
date: 2022-10-08T17:39:51.701Z
tags: 
editor: markdown
dateCreated: 2022-04-15T22:42:22.195Z
---

# Intro

A guide to Intune application deployment troubleshooting.

In order to get the most out of this post you must have a precursory knowledge of

- Creating applications in Intune

## Intune application deployment and it's parts

In order to appropriately troubleshoot Intune Win32app issues we must understand how to devide and conquer the problem. We need to establish a big picture understanding of how apps are delivered and installed on each machine.

![win32-delivery-flow.png](/win32-delivery-flow.png)

1. The computer does an application check on an hourly basis for any new apps that are required or an if an application installation is triggered via Company Portal.

2. Sidecar (the Intune Management Extension) triggers and downloads the application via Delivery Optimization.

3. The application is stored and decrypted in the Intune cache in `C:\Program Files (x86)\Microsoft Intune Management Extension\Content`.

4. The Intune Management Extension service runs the application installer as a child process.

5. The installer waits for the program to finish, checks the exit code (although this hardly matters execpt for a reboot condition), then checks to see if the detection rules are met.

## Common issues inherent to the application deployment workflow.

- The Intune service is 32-bit. Any application install by default will run as 32-bit. If the application is expecting a 64-bit environment and does not switch automatically it can cause the application to fail. If your using a PowerShell script to create / modify / access uninstall keys it will create the key under `SOFTWARE\WOW6432NODE\Microsoft\...` instead of `SOFTWARE\Microsoft\...` This can be solved by calling `cmd` or `powershell` in the `%SystemRoot%\sysnative` path.

> `%SystemRoot%\sysnative\WindowsPowerShell\v1.0\powershell.exe -executionpolicy bypass -file .\foo.ps1`

OR

> `%SystemRoot%\sysnative\cmd.exe /c setup.exe`


- Network security rules: Intune is built on top of Azure and has a wide range of endpoints and IP addresses that it needs access to. It is not uncommon for application deployments or Autopilot to fail due to a restrictive network. You'll need to work with your security team to make sure all endpoints are allowed through. If you are having issues with a large number of applications or autopilot deployments, try them off your corporate network.

- Delivery Optimization is commonly blocked by some security / network teams. If DO is blocked then it should be disabled through a CSP policy. Intune will try for quite some time to download an application unsucessfully before switching to CDN.

## Validating your application installations

When deploying an application through Intune it's in your best interest to test an application as close to how Intune will deploy the application as possible. For a user context this means that  unless you plan on calling the sysnative path in intune you should test and run your application from a 32-bit powershell session.

If you're running the app as SYSTEM, there are some extra considerations. You need to test the application from a SYSTEM context. The system account does not function as a full user account and that can be a problem for many applications. It's also not an account that you can just RunASUser and pop in credientials for. To run an application as SYSTEM you'll need a special program to do so. The most widely accepted and used is [psexec](https://learn.microsoft.com/sysinternals/downloads/psexec) from Microsoft's SysInternals.

To create the session you need for application testing launch psexec as an administrator and create an interactive system shell process. (s= system, i = interactive, d = don't show psexec as a seperate window). Examples below:

Command Line

> `psexec.exe -sid CMD`

Powershell x64

> `psexec.exe -SID powershell.exe`

Powershell x86

> `psexec.exe -SID C:\Windows\SysWOW64\WindowsPowerShell\v1.0\powershell.exe`

From this session you can better test any issues that occur due to running as SYSTEM as any interactive prompts or error messages will display in the interactive session.

## Logging

The Intune Management Extension keeps a thorough log of its actions in `%ProgramData%\Microsoft\IntuneManagementExtension\Logs\IntuneManagementExtension.log`.

The logs, while human readable, can be more easily parsed with CMTrace from Configuration Manager.

In the logs you should be able to see the following:

- download
- decryption
- waiting for installer exit
- detection sucess or fail
- toast message sending
- inventory checking

After your have validated your package in the same context as Intune, checking the IME logs should provide you with critical clues on why your application might be failing.

## Resetting the IME registry

When testing Win32 app deployments in Intune, the IME only tries 3 times per 24 hours, when testing this quickly becomes very tedious.

You can Reset the "Global Re-evaluation Scheme" by removing the application from the IME's registry.

> This should be done with some caution as you're removing the known state of the application from Sidecar.
{.is-warning}

The key can be found at:
> `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\IntuneManagementExtension\Win32Apps\{SID}\{App GUID}`

There are many ways to get a user's SID that I won't cover here. An easy reliable way is to use [psgetsid](https://learn.microsoft.com/sysinternals/downloads/psgetsid) from SysInternals. The app GUID can be found in the application's URL in Intune.

The code example below resets the state of ALL app deployments, forcing the IME service to resync everything.
> 
> This should not be run on a production device.
{.is-danger}

### Code
```powershell
#SCRIPT PROVIDED BY ANDREW JIMENZ (PATCHMYPC)
Stop-Service -Name IntuneManagementExtension
Get-Item -Path registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\IntuneManagementExtension\win32App* |
    Remove-Item -Force -Recurse
Restart-Service -Name IntuneManagementExtension
```
