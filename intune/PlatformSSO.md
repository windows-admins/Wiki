---
title: Platform SSO
description: SSO and Entra Join for MacOS
published: true
date: 2025-03-08T09:56:01.850Z
tags: sso, mdm
editor: markdown
dateCreated: 2024-12-05T15:41:33.868Z
---

# Platform SSO
In order to enable SSO on macOS devices and allow users to log in with their Entra credentials as well as loging in with the Phishing Resistend Multi-Factor-Authentication Secure Enclave, Platform SSO must be implemented. Previously, there were three other tools to achieve a similar functionality, namely Domain Join, Microsoft Enterprise SSO plug-in, and Kerberos SSO, Platform SSO combines them in one tool to deploy, it is the Microsoft **prefered and recommended** option.

# Prerequisites
1. macOS 13.0 and newer
2. Microsoft Intune Company Portal app version 5.2404.0 and newer deployed to the devices
3. [Microsoft Single Sign On extension](https://chromewebstore.google.com/detail/microsoft-single-sign-on/ppnbnpeolgkicgegkbkbjmhlideopiji) if Chrome is getting used
4. An Apple T2 Security Chip or Apple Chips for Secure Enclave 

> On how to deploy Cloud Kerberos Trust to access local file shares without an password see [whfb-cloud-kerberos-trust](/active-directory/whfb-cloud-kerberos-trust).
{.is-info}

# Deploying Platform SSO

> Microsoft recommends using **Secure Enclave** as the authentication method when configuring Platform SSO
{.is-info}

## Intune

MS Documentation: https://learn.microsoft.com/mem/intune/configuration/platform-sso-macos

1. Create the Extensible Single Sign On (SSO) Intune Settings Catalogue Policy 
![settings-picker-authentication-extensible-sso.png](/intune/settings-picker-authentication-extensible-sso.png)

2. Enter these values and deploy the policy
![intune-psso-device-profile.png](/intune/intune-psso-device-profile.png)

3. The user is getting prompted, to login with MFA in order to enable Platfrom SSO and to join the device to entra
![platform-sso-macos-registration-required.png](/intune/platform-sso-macos-registration-required.png)

# Adding Non-Microsoft apps
1. In order to enable SSO on non-Microsoft apps select Extension Data in the settings picker
![settings-picker-authentication-extensible-sso-extension-data.png](/settings-picker-authentication-extensible-sso-extension-data.png)

2.  In Extension Data, Add the following keys and values:
![extension-data-appprefixallowlist.png](/extension-data-appprefixallowlist.png)

- AppPrefixAllowList 
Type: String
Value: The prefixes of your apps (eg. com.fortinet., com.apple. ...) 

> You can get the full app name in the intune portal, or if you enter osasscript -e 'id of app "You app name"' in the terminal of an mac.
{.is-info}


- browser_sso_interaction_enabled 
Type: Integer
Value: 1

- disable_explicit_app_prompt 
Type: Integer
Value: 1

# Enforcing Secure Enclave as MFA
1. To enforce Secure Enclave as MFA, create an Conditional Access Policy, and choose Windows Hello for Business as the authentication type, it includes Secure Enclave.

2. To login with Secure Enclave, do not enter the e-Mail at the Entra login page, choose login options, and then choose Face recognition, fingerprint, PIN or security key to login. 

> You have to scramble the password from the user in Entra or AD, as a Passkey is an alternative login method, and a hacker could use the password to login instead.
{.is-info}

# Validating Functionality & Troubleshooting
To verify, if Platfrom SSO has been deployed sucesfully, enter app-sso platform -s into the terminal, on an mac and you should see an SSO Token. 

Note: The Microsoft Intune Company Portal functions as the authentication broker, if the app stops working, the authentication breaks.

# External Links
* [Best Practices for Deploying Platform SSO with Microsoft Entra ID–Michael Epping, Mark Morowczynski](https://www.youtube.com/watch?v=NEoKLSuO3gw)

* [macOS Platform single sign-on known issues and troubleshooting - Microsoft Entra ID | Microsoft Learn](https://learn.microsoft.com/en-us/entra/identity/devices/troubleshoot-macos-platform-single-sign-on-extension)