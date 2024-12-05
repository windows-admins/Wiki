---
title: Platfrom SSO
description: 
published: false
date: 2024-12-05T15:55:33.864Z
tags: sso, mdm
editor: markdown
dateCreated: 2024-12-05T15:41:33.868Z
---

# Platform SSO
In order to deploy SSO on Mac Devices, and enable users to log in with their Entra Credentials, Platfrom SSO need to be deployed. Previously, there were three other tools to achieve similar things, namely Domain Join, Microsoft Enterprise SSO plug-in, and Kerberos SSO, Platform SSO combines them in one tool to deploy.
# Prerequisites
1. MacOS 13.0 and newer
2. Microsoft Intune Company Portal app version 5.2404.0 and newer deployed 
3. Microsoft Single Sign On extension if Chrome is getting used

# Deploying Platform SSO

MS Documentation: https://learn.microsoft.com/en-us/mem/intune/configuration/platform-sso-macos

> Microsoft recommends using **Secure Enclave** as the authentication method when configuring Platform SSO.
{.is-info}

1. Create the Extensible Single Sign On (SSO) Intune Settings Catalogue Policy 
![settings-picker-authentication-extensible-sso.png](/intune/settings-picker-authentication-extensible-sso.png)

2. Enter these Values and deploy the policy
![intune-psso-device-profile.png](/intune/intune-psso-device-profile.png)

3. Register the device with MFA
![platform-sso-macos-registration-required.png](/intune/platform-sso-macos-registration-required.png)

# Adding Non-Microsoft apps
1. In order to enable SSO on non-Microsoft apps

# External Links
Best Practices for Deploying Platform SSO with Microsoft Entra ID–Michael Epping, Mark Morowczynski
macOS Platform single sign-on known issues and troubleshooting - Microsoft Entra ID | Microsoft Learn