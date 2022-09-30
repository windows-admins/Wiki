---
title: Troubleshooting
description: 
published: true
date: 2022-09-30T12:20:08.607Z
tags: powershell, autopilot, esp
editor: markdown
dateCreated: 2022-09-30T11:20:40.828Z
---

# Troubleshooting

It's pretty simple, just launch PowerShell, install, and execute the `Get-AutopilotDiagnostics` script.

## the steps
1. Press Shift + F10 to launch a command prompt
2. Run `powershell.exe`
3. Run `Set-ExecutionPolicy -Scope 'Process' -ExecutionPolicy 'Unrestricted'`
4. Run `Install-Script -Name Get-AutopilotDiagnostics`
5. Run `Get-AutopilotDiagnostics.ps1 -Online`
