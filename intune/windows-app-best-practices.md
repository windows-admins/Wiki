---
title: Windows App Best Practices
description: 
published: true
date: 2024-03-07T08:43:46.951Z
tags: 
editor: markdown
dateCreated: 2022-04-16T14:56:13.347Z
---

# Line Of Business apps
This app type should generally be avoided because:

- It conflicts with win32 apps during autopilot 
- Lacks a lot of features that win32 have
- Doesn't seem to be actively developed anymore

# Windows app (Win32) 
This app deployment method is preferred for pretty much all windows apps in Intune.

## Prepare Win32 app content for upload

Before you can add a Win32 app to Microsoft Intune, you must prepare the app by using the [Microsoft Win32 Content Prep Tool](https://go.microsoft.com/fwlink/?linkid=2065730).

### Prerequisites

To use Win32 app management, be sure you meet the following criteria:

- Use Windows 10 version 1607 or later (Enterprise, Pro, and Education versions).
- Devices must be registered or joined to Azure Active Directory (Azure AD) and auto-enrolled. The Intune management extension supports devices that are Azure AD registered, Azure AD joined, hybrid domain joined, and group policy enrolled. 

> NOTE: For the scenario of group policy enrollment, the user uses the local user account to Azure AD join their Windows 10 device. The user must log on to the device by using their Azure AD user account and enroll in Intune. Intune will install the Intune Management extension on the device if a PowerShell script or a Win32 app is targeted to the user or device.
{.is-info}


- Windows application size is capped at 30 GB per app.

### Convert the Win32 app content

Use the [Microsoft Win32 Content Prep Tool](https://go.microsoft.com/fwlink/?linkid=2065730) to pre-process Windows classic (Win32) apps. The tool converts application installation files into the *.intunewin* format. The tool also detects some of the attributes that Intune requires to determine the application installation state. After you use this tool on the app installer folder, you'll be able to create a Win32 app in the Intune console.

> IMPORTANT: The [Microsoft Win32 Content Prep Tool](https://go.microsoft.com/fwlink/?linkid=2065730) zips all files and subfolders when it creates the *.intunewin* file. Be sure to keep the Microsoft Win32 Content Prep Tool separate from the installer files and folders, so that you don't include the tool or other unnecessary files and folders in your *.intunewin* file.
{.is-info}



You can download the [Microsoft Win32 Content Prep Tool](https://go.microsoft.com/fwlink/?linkid=2065730) from GitHub as a .zip file. The zipped file contains a folder named *Microsoft-Win32-Content-Prep-Tool-master*. The folder contains the prep tool, the license, a readme, and the release notes. 

#### Process flow to create a .intunewin file

   ![win32-app-flow](https://docs.microsoft.com/en-us/mem/intune/apps/media/apps-win32-app-management/prepare-win32-app.png)

#### Running the Microsoft Win32 Content Prep Tool

If you run `IntuneWinAppUtil.exe` from the command window without parameters, the tool will guide you to enter the required parameters step by step. Or, you can add the parameters to the command based on the following available command-line parameters.

#### Available command-line parameters 

|    **Command-line   parameter**    |    **Description**    |
|--------------------------------|------------------------------------------------------------|
|    `-h`     |    Help    |
|    `-c <setup_folder>`     |    Folder for all setup files. All files in this folder will be compressed into an *.intunewin* file.    |
|    `-s <setup_file>`     |    Setup file (such as *setup.exe* or *setup.msi*).    |
|    `-o <output_folder>`     |    Output folder for the generated *.intunewin* file.    |
|    `-q`       |    Quiet mode.    |


#### Example commands

|    **Example command**    |    **Description**    |
|-------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    `IntuneWinAppUtil -h`    |    This command will show usage information for the tool.    |
|    `IntuneWinAppUtil -c c:\testapp\v1.0 -s c:\testapp\v1.0\setup.exe -o c:\testappoutput\v1.0 -q`    |    This command will generate the *.intunewin* file from the specified source folder and setup file. For the MSI setup file, this tool will retrieve required information for Intune. If `-q` is specified, the command will run in quiet mode. If the output file already exists, it will be overwritten. Also, if the output folder doesn't exist, it will be created automatically.    |

## Troubleshooting
There is a issue with the Microsoft Win32 Content Prep Tool in that it will occasionally crash when generating an intunewin package. The fix is to run CMD/Powershell, **maximise** the window, and call `IntuneWinAppUtil.exe` with your desired parameters.
