---
title: Platfrom SSO
description: 
published: false
date: 2024-12-05T15:43:53.650Z
tags: sso, mdm
editor: markdown
dateCreated: 2024-12-05T15:41:33.868Z
---

# Header
Your content here

In order to deploy SSO on Mac Devices, and enable users to log in with their Entra Credentials, Platfrom SSO need to be deployed. Previously, there were three other tools to achieve similar things, namely Domain Join, Microsoft Enterprise SSO plug-in, and Kerberos SSO, Platform SSO combines them in one tool to deploy.
# Prerequisites
1. MacOS 13.0 and newer
2. Microsoft Intune Company Portal app version 5.2404.0 and newer deployed 
3. Microsoft Single Sign On extension if Chrome is getting used# Deploying Platform SSO

MS Documentation: Configure Platform SSO for macOS devices | Microsoft Learn Microsoft recommends using Secure Enclave as the authentication method when configuring Platform SSO.1. Create the Extensible Single Sign On (SSO) Intune Settings Catalogue Policy 


Screenshot that shows the Settings Catalog settings picker, and selecting authentication and extensible SSO category in Microsoft Intune.

2. Enter these Values and deploy the policy

Screenshot that shows the recommended Platform SSO settings in an Intune MDM profile.

3. Register the device with MFA

Screenshot that shows the registration required prompt on end user devices when you configure Platform SSO in Microsoft Intune.

# Adding Non-Microsoft apps1. In order to enable SSO on non-Microsoft apps# External LinksBest Practices for Deploying Platform SSO with Microsoft Entra ID–Michael Epping, Mark MorowczynskimacOS Platform single sign-on known issues and troubleshooting - Microsoft Entra ID | Microsoft Learn