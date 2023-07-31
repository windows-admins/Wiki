---
title: Windows Store
description: Provides brief important information about the Windows Store
published: false
date: 2023-07-31T12:02:56.172Z
tags: intune, windows, csp, mdm, windows store
editor: markdown
dateCreated: 2023-07-25T19:41:32.958Z
---

# Windows Store
This entry will try to point out typical misconceptions about all settings related to the Windows Store using Intune. Please note that it is specifically aimed at Intune using CSPs, which in this case requires a Windows Enterprise/Education license for most settings shown here (verify by visiting the CSP documentation linked here). The behavior using different methods of configuration should be similar, but is not guaranteed.  

## Block user access to the store
There is only one setting that is required to block the access to the store and its called "Require private store only". https://learn.microsoft.com/en-us/windows/client-management/mdm/policy-csp-applicationmanagement#requireprivatestoreonly. Keep in mind, that if the store was disabled for a long time app updates will happen at this point. Consider configuring Delivery Optimization to prevent network congestion. 

This setting can be deployed to the user or to a device:
- It is possible to scope this setting to a user setting, so that an admin would have access to the store whereas a normal user would not.
- If scoped to device **all users** on that device cannot access the store.

![microsoftstoreisblocked.png](/microsoftstoreisblocked.png)

## Automatic Windows Store app updates
Unless you have very specific requirements (see https://learn.microsoft.com/en-us/microsoft-store/distribute-offline-apps#why-offline-licensed-apps), you should not configure the following settings because they will prevent automatic Store updates. As of May 2023, the only automatic offline update feature available through Microsoft using Configuration Manager is **disabled**, after being deprecated since November 2021 (see https://learn.microsoft.com/en-us/mem/configmgr/apps/deploy-use/manage-apps-from-the-windows-store-for-business). In other words, updates to built-in applications will only be done online (automatically) or using Winget (via scripts).

> While there are other settings like "Turn off Automatic Download of updates on Win8 machines", only Updates applying to supported Windows 10 an higher will be discussed here. You should not have settings configured, that don't apply to your operating system as that might have unforseen consequences.
{.is-info}

It is possible to update apps using scripts around winget. However, this means controlling each app 

### Consequences of not updating built-in apps
- Security issues will not be resolved
- Upgrades to more recent versions of the operating system might cause apps to stop functioning (Calculator, Photos, Sticky Notes ...)
- Documented features might be missing from apps, which will cause a worse user experience

### "Turn off the Store application"
https://learn.microsoft.com/en-us/windows/client-management/mdm/policy-csp-admx-windowsstore#removewindowsstore_1 Both, the user and the device setting have a note that reads: 
**"If you enable this setting, access to the Store application is denied. Access to the Store is required for installing app updates."**
### "Allow apps from the Microsoft app store to auto update"
https://learn.microsoft.com/en-us/windows/client-management/mdm/policy-csp-applicationmanagement#allowappstoreautoupdate 
Does what it says: Control the automatic update of Store applications.
> If you don't set this setting and users have access to the store, they can disable this. See https://support.microsoft.com/en-us/windows/turn-on-automatic-app-updates-70634d32-4657-dc76-632b-66048978e51b
{.is-warning}

### "Disable all apps from Microsoft Store"
https://learn.microsoft.com/en-us/windows/client-management/mdm/policy-csp-applicationmanagement#disablestoreoriginatedapps 
This doesn't just turn off apps, it also prevents store updates of those apps. Reminder, depending on your scenario you still might those apps updated for security reasons.
## False friends
### "Turn off access to the Store"
This setting, as Rudy points out here https://call4cloud.nl/2020/06/managing-apps-in-the-microsoft-store/#part1, does not disable the store access, but rather the option to point to the store for unknown filetypes. Does not prevent "Open with" though, you have to addtionally configure https://learn.microsoft.com/en-us/windows/client-management/mdm/policy-csp-admx-icm#shellnousestoreopenwith_1 for that.
### "Turn off access to all Windows Update features"
While this setting does not prevent store apps from updating, it will prevent the user from adding optional features (including language packs) and searching for updates online.