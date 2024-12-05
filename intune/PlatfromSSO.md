---
title: Platfrom SSO
description: 
published: false
date: 2024-12-05T16:21:01.782Z
tags: sso, mdm
editor: markdown
dateCreated: 2024-12-05T15:41:33.868Z
---

# Platform SSO
In order to enable SSO on Mac Devices, and enabling users to log in with their Entra Credentials, Platform SSO need to be deployed. Previously, there were three other tools to achieve similar things, namely Domain Join, Microsoft Enterprise SSO plug-in, and Kerberos SSO, Platform SSO combines them in one tool to deploy.

# Prerequisites
1. macOS 13.0 and newer
2. Microsoft Intune Company Portal app version 5.2404.0 and newer deployed 
3. Microsoft Single Sign On extension if Chrome is getting used

> On how to deploy Cloud Kerberos Trust to access local file shares without an password see [whfb-cloud-kerberos-trust](/active-directory/whfb-cloud-kerberos-trust).
{.is-info}

# Deploying Platform SSO

> Microsoft recommends using **Secure Enclave** as the authentication method when configuring Platform SSO
{.is-info}

**Intune**

MS Documentation: https://learn.microsoft.com/en-us/mem/intune/configuration/platform-sso-macos

1. Create the Extensible Single Sign On (SSO) Intune Settings Catalogue Policy 
![settings-picker-authentication-extensible-sso.png](/intune/settings-picker-authentication-extensible-sso.png)

2. Enter these values and deploy the policy
![intune-psso-device-profile.png](/intune/intune-psso-device-profile.png)

3. Open the notification on your macs, and login with MFA in order to enable and join the device to entra.
![platform-sso-macos-registration-required.png](/intune/platform-sso-macos-registration-required.png)

# Adding Non-Microsoft apps
1. In order to enable SSO on non-Microsoft apps select Extension Data in the settings picker
![settings-picker-authentication-extensible-sso-extension-data.png](/settings-picker-authentication-extensible-sso-extension-data.png)

2.  In Extension Data, Add the following keys and values:
![extension-data-appprefixallowlist.png](/extension-data-appprefixallowlist.png)

- AppPrefixAllowList 
Type: String
Value: The prefixes of your apps (eg. com.fortinet., com.apple. ...) 

> You can get the full app name if you enter xxx into the terminal of the mac, or you can get it in the intune portal
{.is-info}


- browser_sso_interaction_enabled 
Type: Integer
Value: 1

- disable_explicit_app_prompt 
Type: Integer
Value: 1

# Validating Functionality & Troubleshooting

# External Links
Best Practices for Deploying Platform SSO with Microsoft Entra ID–Michael Epping, Mark Morowczynski
macOS Platform single sign-on known issues and troubleshooting - Microsoft Entra ID | Microsoft Learn