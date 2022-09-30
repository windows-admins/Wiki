---
title: Troubleshooting
description: 
published: true
date: 2022-09-30T12:17:33.592Z
tags: powershell, autopilot, esp
editor: markdown
dateCreated: 2022-09-30T11:20:40.828Z
---

# Troubleshooting Autopilot

It's pretty simple, just open up the CMD, launch powershell and download and execute the autopilotdiagnostics script

## the steps
> 1. Shift + F10
> 1. Enter `PowerShell`
> 1. Enter `Set-ExecutionPolicy -Scope 'Process' -ExecutionPolicy 'Unrestricted'`
> 1. Enter `Install-Script Get-AutopilotDiagnostics`
> 1. Enter `Get-AutopilotDiagnostics.ps1 -Online`