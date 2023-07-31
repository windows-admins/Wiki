---
title: Windows Store
description: Provides brief important information about the Windows Store
published: true
date: 2023-07-31T12:58:36.273Z
tags: intune, windows, csp, mdm, windows store
editor: markdown
dateCreated: 2023-07-25T19:41:32.958Z
---

# Windows Store
This entry will try to point out typical misconceptions about all settings related to the Windows Store using Intune. Please note that it is specifically aimed at Intune using CSPs, which in this case requires a Windows Enterprise/Education license for most settings shown here (verify by visiting the CSP documentation linked here). The behavior using different methods of configuration should be similar, but is not guaranteed.  

## Block user access to the store
There is only one setting required to block access to the store and it is called "Require private store only". https://learn.microsoft.com/en-us/windows/client-management/mdm/policy-csp-applicationmanagement#requireprivatestoreonly. Keep in mind that if the store is disabled for a long time, app updates will happen at this point. Consider configuring delivery optimization to avoid network congestion. 

This setting can be applied to the user or to a device:
- It is possible to scope this setting to a user so that an admin can access the store while a normal user cannot.
- If scoped to a device, **all users** on that device cannot access the store.

![microsoftstoreisblocked.png](/microsoftstoreisblocked.png)

## Automatic Windows Store app updates
Unless you have very specific requirements (see https://learn.microsoft.com/en-us/microsoft-store/distribute-offline-apps#why-offline-licensed-apps), you should not configure the following settings because they will prevent automatic Store updates. As of May 2023, the only automatic offline update feature available through Microsoft using Configuration Manager is **disabled**, after being deprecated since November 2021 (see https://learn.microsoft.com/en-us/mem/configmgr/apps/deploy-use/manage-apps-from-the-windows-store-for-business). In other words, updates to built-in applications will only be done online (automatically) or using Winget (via scripts).

> While there are other settings like "Turn off Automatic Download of updates on Win8 machines", only settings that apply to supported Windows 10 an higher will be discussed here. You shouldn't configure settings that don't apply to your operating system, as this could have unintended consequences.
{.is-info}

### Consequences of not updating built-in apps
- Security issues are not resolved
- Upgrades to newer versions of the operating system may cause applications to stop working (calculator, photos, sticky notes...)
- Documented features may be missing from applications, resulting in a poorer user experience

### "Turn off the Store application"
https://learn.microsoft.com/en-us/windows/client-management/mdm/policy-csp-admx-windowsstore#removewindowsstore_1 Both, the user and the device setting have a note that reads: 
**"If you enable this setting, access to the Store application is denied. Access to the Store is required for installing app updates."**
### "Allow apps from the Microsoft app store to auto update"
https://learn.microsoft.com/en-us/windows/client-management/mdm/policy-csp-applicationmanagement#allowappstoreautoupdate 
Does what it says: Control the automatic update of Store applications.
> If you don't set this preference and users have access to the store, they can disable it. See https://support.microsoft.com/en-us/windows/turn-on-automatic-app-updates-70634d32-4657-dc76-632b-66048978e51b
{.is-warning}

### "Disable all apps from Microsoft Store"
https://learn.microsoft.com/en-us/windows/client-management/mdm/policy-csp-applicationmanagement#disablestoreoriginatedapps 
This not only turns off apps, it also prevents those apps from receiving store updates. As a reminder, depending on your scenario, you may still need to update these apps for security reasons.
## False friends
### "Turn off access to the Store"
This setting, as Rudy points out here https://call4cloud.nl/2020/06/managing-apps-in-the-microsoft-store/#part1, does not disable access to the store, but rather the option to point to the store for unknown file types. It does not prevent "open with", you have to configure https://learn.microsoft.com/en-us/windows/client-management/mdm/policy-csp-admx-icm#shellnousestoreopenwith_1 for that additionally.
### "Turn off access to all Windows Update features"
https://learn.microsoft.com/en-us/windows/client-management/mdm/policy-csp-admx-icm#removewindowsupdate_icm
While this setting does not prevent Store applications from updating, it does prevent the user from adding optional features (including language packs) and from searching for updates online.