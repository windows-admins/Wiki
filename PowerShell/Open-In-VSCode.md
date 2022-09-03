---
title: Open Files in VSCode from PowerShell
description: How to open files from the terminal, or PS window in VSCode.
published: true
date: 2022-05-14T18:34:13.501Z
tags: powershell, vscode
editor: markdown
dateCreated: 2022-05-14T18:34:10.071Z
---

Open Files in VSCode from PowerShell

When you first install VSCode on a machine, there is a small blurb you might not notice as part of the configuration. That is the action "Add code to path". 

As a result, this allows you to open the contents of a file directly from a PowerShell or Terminal Window. 

```powershell
code myfile.ps1
```

This will cause VSCode to open up the file `myfile.ps1` in VSCode. 

This can be very PowerFull when used in conjunction with environment variables! For example if you want to launch and edit your PowerShell profile, you can run the following. 

```powershell
code $PROFILE
```

This will then open up your powershell profile using VSCode. 