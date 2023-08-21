---
title: App Installer
description: The app installer (aka winget) is a package manager for Windows Operating Systems
published: true
date: 2023-08-21T21:14:37.782Z
tags: intune, windows, csp, mdm, app installer, winget
editor: markdown
dateCreated: 2023-08-21T18:25:37.613Z
---

# App Installer
The app installer is a package manager built into Windows OS. It provides similar functions to APT in Linux based OS. The package managers executable is called `winget` and behaves similar to `apt-get`. It cannot be controlled by ConfigMgr. [Deploying Store apps through Intune](https://learn.microsoft.com/en-gb/mem/intune/apps/store-apps-microsoft) is now the only supported way to install these types of applications.

## Configure
As of `August 2023` there are no settings catalog configurations available. Configuration has to be done through custom OMA URIs, GPO or [imported ADMX files](https://learn.microsoft.com/en-us/mem/intune/configuration/administrative-templates-import-custom).
> **Note**: To get the latest ADMX files use a patched Windows computer and grab  "C:\Windows\PolicyDefinitions\DesktopAppInstaller.admx" plus the en-us adml "C:\Windows\PolicyDefinitions\en-US\DesktopAppInstaller.adml"
**Note2**: Importing the app installer ADMX file into Intune the Windows.admx is also required. 
**Note3**: The ADMX does not contain all available settings from the CSP. 
{.is-info}

### Completely Block user access 
To block users access to winget use the ['Enable App Installer'](https://learn.microsoft.com/en-us/windows/client-management/mdm/policy-csp-desktopappinstaller#enableappinstaller) setting and set it to 'disabled'. Please keep in mind, that this is a _device_ setting and will therefore be applied to all users on a device, regardless what object type it is assigned to.

### Limit access to allowed sources
The default sources for the app installer are `winget` and `msstore`. These are enabled by default. Depending on your situation, you might want to limit access to winget to one of the defaults or to a custom source. 
* To limit access to source 'winget': **Disable** the setting [EnableMicrosoftStoreSource](https://learn.microsoft.com/en-us/windows/client-management/mdm/policy-csp-desktopappinstaller#enabledefaultsource)
* To limit access to source 'msstore': **Disable** the setting [EnableDefaultSourceDisable](https://learn.microsoft.com/en-us/windows/client-management/mdm/policy-csp-desktopappinstaller#enabledefaultsource)
* Its also possible to limit the sources to a custom source by [allowing](https://learn.microsoft.com/en-us/windows/client-management/mdm/policy-csp-desktopappinstaller#enableallowedsources) or [enabling](https://learn.microsoft.com/en-us/windows/client-management/mdm/policy-csp-desktopappinstaller#enableadditionalsources) them. Set the last two bullet points accordingly if this should be the only source.
### Disable app installer protocol (CSP only)
By default, `ms-appinstaller` links can be used in several places like this `ms-appinstaller:?source=https://aka.ms/getwinget` (put this in a "run" window). This policy is not in the ADMX and must be set using a custom OMA URI. [Here are some examples](https://learn.microsoft.com/en-us/troubleshoot/mem/intune/device-configuration/deploy-oma-uris-to-target-csp-via-intune#oma-uris-from-the-microsoft-intune-admin-center) of how that looks. 
## Troubleshooting
[Refer to the official documentation](https://learn.microsoft.com/en-us/windows/package-manager/winget/troubleshooting). Logs are located in `%LOCALAPPDATA%\Packages\Microsoft.DesktopAppInstaller_8wekyb3d8bbwe\LocalState\DiagOutputDir`