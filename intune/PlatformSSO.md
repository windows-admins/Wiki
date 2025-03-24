---
title: Platform SSO
description: SSO and Entra Join for MacOS
published: true
date: 2025-03-24T19:03:49.723Z
tags: sso, mdm
editor: markdown
dateCreated: 2024-12-05T15:41:33.868Z
---

# Platform SSO
In order to enable SSO on macOS devices and allow users to log in with their Entra credentials as well as loging in with the Phishing Resistant Multi-Factor Authentication Secure Enclave, Platform SSO must be implemented. Previously, there were three other tools to achieve a similar functionality, namely Domain Join, Microsoft Enterprise SSO plug-in, and Kerberos SSO. Platform SSO combines them in one tool to deploy, it is the Microsoft **prefered and recommended** option.

# Prerequisites
1. macOS 13.0 and newer
2. Microsoft Intune Company Portal app version 5.2404.0 and newer deployed to the devices
3. [Microsoft Single Sign On extension](https://chromewebstore.google.com/detail/microsoft-single-sign-on/ppnbnpeolgkicgegkbkbjmhlideopiji) if Chrome is being used
4. An Apple T2 Security Chip or Apple Chips for Secure Enclave 

> For instructions on how to deploy Cloud Kerberos Trust to access local file shares without a password, see [whfb-cloud-kerberos-trust](/active-directory/whfb-cloud-kerberos-trust).
{.is-info}

# Deploying Platform SSO

> Microsoft recommends using **Secure Enclave** as the authentication method when configuring Platform SSO
{.is-info}

## Intune

MS Documentation: https://learn.microsoft.com/mem/intune/configuration/platform-sso-macos

1. Create the Extensible Single Sign On (SSO) Intune Settings Catalogue Policy 
![settings-picker-authentication-extensible-sso.png](/intune/settings-picker-authentication-extensible-sso.png)

2. Enter these values and deploy the policy to the desired devices
![intune-psso-device-profile.png](/intune/intune-psso-device-profile.png)

3. The user will be prompted to login with their Entra credentials and MFA in order to enable Platfrom SSO and join the device to Entra
![platform-sso-macos-registration-required.png](/intune/platform-sso-macos-registration-required.png)

# Adding Non-Microsoft apps
1. In order to enable SSO on non-Microsoft apps select "Extension Data" in the settings picker
![settings-picker-authentication-extensible-sso-extension-data.png](/settings-picker-authentication-extensible-sso-extension-data.png)

2.  In Extension Data, add the following keys and values:
![extension-data-appprefixallowlist.png](/extension-data-appprefixallowlist.png)

- AppPrefixAllowList 
Type: String
Value: The prefixes of your apps (eg. com.fortinet., com.apple. ...) 

> You can get the full app name in the Intune portal, or run this command in a Terminal on your Mac.
`osasscript -e 'id of app "Your app name here"'`
{.is-info}


- browser_sso_interaction_enabled 
Type: Integer
Value: 1

- disable_explicit_app_prompt 
Type: Integer
Value: 1

# Enforcing Secure Enclave as MFA
1. To enforce Secure Enclave as MFA, create a Conditional Access Policy, and choose Windows Hello for Business as the authentication type, it includes Secure Enclave.

2. To login with Secure Enclave, do not enter the email at the Entra login page. Choose login options, and then choose "Face, fingerprint, PIN or security key" to login.

> For the user to go "passwordless", you would need to scramble the password from the user in Entra or AD to a strong unknown value, as a Passkey is an alternative login method, and a hacker could use the password to login instead.
{.is-info}

# Validating Functionality & Troubleshooting
To verify if Platfrom SSO has been deployed successfully on a device, enter `app-sso platform -s` into a Terminal on a Mac and you should see an SSO Token. 

Note: The Microsoft Intune Company Portal app functions as the authentication broker. If the app stops working, the authentication breaks.

# External Links
* [Best Practices for Deploying Platform SSO with Microsoft Entra ID–Michael Epping, Mark Morowczynski](https://www.youtube.com/watch?v=NEoKLSuO3gw)

* [macOS Platform single sign-on known issues and troubleshooting - Microsoft Entra ID | Microsoft Learn](https://learn.microsoft.com/en-us/entra/identity/devices/troubleshoot-macos-platform-single-sign-on-extension)