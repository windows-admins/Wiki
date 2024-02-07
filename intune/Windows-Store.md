---
title: Windows Store
description: Provides brief important information about the Windows Store
published: true
date: 2024-02-07T19:33:55.160Z
tags: intune, windows, csp, mdm, windows store
editor: markdown
dateCreated: 2023-07-25T19:41:32.958Z
---

# Windows Store
This entry will try to point out typical misconceptions about all settings related to the Windows Store using Intune. Please note that it is specifically aimed at Intune using CSPs, which in this case requires a Windows Enterprise/Education license for most settings shown here (verify by visiting the CSP documentation linked here). The behavior using different methods of configuration should be similar, but is not guaranteed.

## Block user access to the store while allowing updates
Currently, both methods provide the same outcome, with the exception of winget availabiltiy. Microsoft usually points at the second method, hinting so 
### Using "Require private store only"
There is only one setting required to block access to the store and it is called  ["Require private store only"](https://learn.microsoft.com/en-us/windows/client-management/mdm/policy-csp-applicationmanagement#requireprivatestoreonly). Keep in mind that if the store is disabled for a long time, app updates will happen at this point. Consider configuring delivery optimization to avoid network congestion. 

This setting can be applied to the user or to a device:
- It is possible to scope this setting to a user so that an admin can access the store while a normal user cannot.
- If scoped to a device, **all users** on that device cannot access the store.
- Winget will continue to work, you can configure it by configuring the [App Installer](/intune/App-Installer)

### Using "Turn off the Store application"
Two settings are required to block the store while still allowing built-in applications to be updated. 
- [RemoveWindowsStore _1(User) or _2(Device)](https://learn.microsoft.com/en-us/windows/client-management/mdm/policy-csp-admx-windowsstore#removewindowsstore_2). This setting is ADMX-based, which means that other configurations (GPO or security tools) may interfere with this setting. Set this to "**Enabled**". Both the user and device settings have a note that reads **"If you enable this setting, you will be denied access to the Store application. Access to the Store is required to install application updates."** so we also need the following setting. 
- [AllowAppStoreAutoUpdate](https://learn.microsoft.com/en-us/windows/client-management/mdm/policy-csp-applicationmanagement#allowappstoreautoupdate): Does what it says: Control the automatic update of Store applications. Set this to "Allowed.". If you don't set this preference and users have access to the store, [they can disable it themselves](https://support.microsoft.com/en-us/windows/turn-on-automatic-app-updates-70634d32-4657-dc76-632b-66048978e51b)

> The ability to use Winget to install applications is also removed by using *RemoveWindowsStore*.
{.is-warning}

### User Experience
In both cases, the user experience when launching the store looks like this:
![microsoftstoreisblocked.png](/microsoftstoreisblocked.png)
## Automatic Windows Store app updates
Unless you [have very specific requirements](https://learn.microsoft.com/en-us/microsoft-store/distribute-offline-apps#why-offline-licensed-apps), you should ***not configure*** the following settings because they will prevent automatic Store updates. As of May 2023, the only automatic offline update feature available through Microsoft using Configuration Manager is **disabled**, after being [deprecated since November 2021](https://learn.microsoft.com/en-us/mem/configmgr/apps/deploy-use/manage-apps-from-the-windows-store-for-business). In other words, updates to built-in applications will only be done online (automatically) or using Winget (via scripts).

> While there are other settings like "Turn off Automatic Download of updates on Win8 machines", only settings that apply to supported Windows 10 an higher will be discussed here. You shouldn't configure settings that don't apply to your operating system, as this could have unintended consequences.
{.is-info}

> Both methods described earlier will update apps on their own schedule. As of January 2024 there is no reliable way to trigger store updates when the store access is disabled. 
{.is-info}

### Consequences of not updating built-in apps
- Security issues are not fixed
- Upgrading to newer versions of the operating system may cause applications to stop working (calculator, photos, sticky notes...)
- Documented features may be missing from applications, resulting in a poorer user experience

### "Disable all apps from Microsoft Store"
[DisableStoreOriginatedApps](https://learn.microsoft.com/en-us/windows/client-management/mdm/policy-csp-applicationmanagement#disablestoreoriginatedapps)
This not only turns off apps, it also prevents those apps from receiving store updates. As a reminder, depending on your scenario, you may still need to update these apps for security reasons.

## False friends
### "Turn off access to the Store"
This setting, [as Rudy points out](https://call4cloud.nl/2020/06/managing-apps-in-the-microsoft-store/#part1), does not disable access to the store, but rather the option to point to the store for unknown file types. It does not prevent "open with", you have to configure [ShellNoUseStoreOpenWith_1](https://learn.microsoft.com/en-us/windows/client-management/mdm/policy-csp-admx-icm#shellnousestoreopenwith_1) for that additionally.
### "Turn off access to all Windows Update features"
[RemoveWindowsUpdate_ICM](https://learn.microsoft.com/en-us/windows/client-management/mdm/policy-csp-admx-icm#removewindowsupdate_icm): While this setting does not prevent Store applications from updating, it does prevent the user from adding optional features (including language packs) and from searching for updates online.

## Persistence of apps through wipe
When 'System' is selected as the installation context for UWP applications using the Microsoft Store (new) method, they are added to the operating system's provisioned packages. This means that applications installed this way will survive a wipe (with no options selected), a fresh start, and an Autopilot reset. However, after these actions, the application will appear as if uninstalled in the company portal, even though it is installed and available to the user.
![uwpaddedtoprovisionedpackage.png](/uwpaddedtoprovisionedpackage.png)

## Network requirements
Working with a proxy or firewall the following URLs need to be reachable in both the system **and** user context.
> [General domains/URLs](https://learn.microsoft.com/en-us/mem/intune/fundamentals/intune-endpoints#microsoft-store) 
> [Specific domains/URLs](https://learn.microsoft.com/en-us/windows/privacy/manage-windows-11-endpoints)
{.is-info}

## Related Content
- [App Installer](/intune/App-Installer)
- [Deploying store apps during ESP](/autopilot/deploying-store-apps-during-esp)
