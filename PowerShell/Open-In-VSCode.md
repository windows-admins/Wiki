---
title: Open Files in VSCode from PowerShell
description: How to open files from the terminal, or PS window in VSCode.
published: true
date: 2022-09-29T22:10:23.479Z
tags: powershell, vscode
editor: markdown
dateCreated: 2022-05-14T18:34:10.071Z
---

## Files

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

## Folders

The same commands work to open folders so:

```powershell
code myfolder
```

Will open the entire folder in PowerShell.

## Playing with Windows

So VS Code has some funky switches we can play with too!

Open myfile.ps1 in the last active window:

```powershell
code myfile.ps1 -r
```

Open myfile.ps1 and go to line 10:

```powershell
code -g myfile.ps1:10
```

Add myfolder to the workspace open in the last active window (or current window if run from the VS Code Terminal!):

```powershell
code -a myfolder
```

For full documentation on the command line switches see: https://code.visualstudio.com/docs/editor/command-line