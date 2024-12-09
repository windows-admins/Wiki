---
title: Troubleshooting
description: 
published: true
date: 2024-12-09T20:06:16.580Z
tags: powershell, autopilot, esp
editor: markdown
dateCreated: 2022-09-30T11:20:40.828Z
---

It's pretty simple, just launch PowerShell, install, and execute the `Get-AutopilotDiagnostics` script.

## The Steps
1. Press Shift + F10 to launch a command prompt
2. Run `powershell.exe`
3. Run `Set-ExecutionPolicy -Scope 'Process' -ExecutionPolicy 'Unrestricted'`
4. Run `Install-Script -Name Get-AutopilotDiagnosticsCommunity`
5. Run `Get-AutopilotDiagnosticsCommunity.ps1 -Online`

### The Output

![ye9gwlcbio.png](/ye9gwlcbio.png)