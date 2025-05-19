---
title: Windows Subscription Activation
description: This page explains what to look out for with subscription activation.
published: true
date: 2025-05-19T20:17:27.337Z
tags: windows, licensing, activation, subscription activation
editor: markdown
dateCreated: 2025-05-15T20:21:25.692Z
---

# Windows Subscription Activation
This page will shortly explain how subscription activation works and what to look out for.

## How do I know if my computer is subscription activated?
Press `Windows key + R` and enter `ms-settings:activation`. A successful subscription activation looks like this:
![subscriptionactivation-success.png](/subscriptionactivation-success.png){.align-center}
## My computer looks different
You may look at a "Windows Pro" activation instead of "Enterprise". The key for this can be a MAK, Retail or OEM (UEFI/BIOS embedded, meaning it's embedded with the devices firmware) source. Even a KMS key will look like this.
![retail-or-oem-activation.png](/retail-or-oem-activation.png){.align-center}
            
If the Microsoft personal account that you're signed in with has a key, it will look like this:
![microsoftaccount.png](/microsoftaccount.png){.align-center}

## Prerequistes
Check these prerequistes first
* Network: [Check the endpoints outlined in the table under 'Licensing'](https://learn.microsoft.com/en-us/windows/privacy/manage-windows-11-endpoints). Additionally you might want to look at the Intune [requirements for the Microsoft Store](https://learn.microsoft.com/en-us/intune/intune-service/fundamentals/intune-endpoints?#microsoft-store)
* Make sure the "ClipSVC" service is enabled and set to *enabled* with startup-type manual
* Device has to be Entra or Hybrid-Joined - no exceptions.
* Education licensing has [their own requirements](https://learn.microsoft.com/en-us/windows/deployment/windows-subscription-activation?pivots=windows-11#windows-education-requirements).

## Points worth noting
Depending on your environment the following provide some guidance around troubleshooting known issues. 
* A subscription activation will [replace another same-level activation](https://learn.microsoft.com/en-us/windows/deployment/windows-subscription-activation?pivots=windows-11#existing-enterprise-deployments) such as KMS or ADBA. Meaning, it will not fall back to these once it has activated using a users license. The built-in Windows Pro key will be used as fallback if this method fails.
* Devices that have never activated their embedded key might have to use a [PowerShell once to activate](https://learn.microsoft.com/en-us/windows/deployment/windows-subscription-activation?pivots=windows-11#existing-enterprise-deployments)
* Conditional Access Policies that cover "All users" and "All resources" and enforce MFA might want to exclude the Microsoft Store app unless users are signing into Windows using a password instead of WHfB to satisfy the Multi-Factor requirement. [Otherwise might get prompted to authenticate](https://learn.microsoft.com/en-us/windows/deployment/windows-subscription-activation?pivots=windows-11#adding-conditional-access-policy).
* If you're in the EEA zone users [need to confirm they want to use SSO](https://techcommunity.microsoft.com/blog/windows-itpro-blog/upcoming-changes-to-windows-single-sign-on/4008151). This means that no token is created unless the user confirms this dialog at least once. Without a token, the user cannot activate their subscription successfully because that's how the license is verified.
* As long as a user with a Windows subscription signs [in every 30 days](https://learn.microsoft.com/en-us/windows/deployment/windows-subscription-activation?pivots=windows-11#licenses), shared devices will use subscription activation.
* VMs created on a device using subscription activation are [also automatically activated](https://learn.microsoft.com/en-us/windows/deployment/windows-subscription-activation?pivots=windows-11#adding-conditional-access-policy)
