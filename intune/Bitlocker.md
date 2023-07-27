---
title: Bitlocker
description: 
published: true
date: 2023-07-27T15:23:45.468Z
tags: 
editor: markdown
dateCreated: 2023-07-26T14:39:27.350Z
---

# Intro
This guide was made with simplicity in mind, just enough to get you going with a working bitlocker configuration.

While bitlocker can be configured in various ways, for simplicities sake we will stick with the endpoint security node in the intune admin portal

> This guide was tested on a Entra only environment, no legacy devices were considered.
{.is-info}


endpoint security -> disk encryption -> create policy

the policy will present you with two sections, bitlocker and administrative templates

# Configuring the policy

## Bitlocker

Require Device Encryption: **Enabled**
Allow Warning For Other Disk Encryption: **Disabled**
- Allow Standard User Encryption: **Enabled**

Configure Recovery Password Rotation: **Refresh on for Azure AD-Joined devices**

## Administrative templates
### Windows Components > BitLocker Drive Encryption
Choose drive encryption method and cipher strenght (Windows 10 [Version 1511] and later): **Enabled**
- Select the encryption method for removable data drivers: **AES-CBC 128-bit (default)**
- Select the encryption method for fixed data drives: **AES-CBC 128-bit (default)**
- Select the encryption method for operating system drives: **AES-CBC 128-bit (default)**

### Windows Components > BitLocker Drive Encryption > Operating System Drives

Enforce drive encryption type on operating system drives: **Enabled**
- Select the encryption type: (Device): **Used Space Only encryption**
Require additional authentication at startup: **Enabled**
- Configure TPM startup key and PIN: **Do not allow startup key and PIN with TPM**
- Configure TPM startup PIN: **Do not allow startup PIN with TPM**
- Configure TPM startup: **Require TPM**
- Allow BitLocker without a compatible TPM (requires a password or a startup key on a USB flash drive): **False**
- Configure TPM startup key: **Do not allow startup key with TPM**

Choose how BitLocker-protected operating system drives can be recovered: **Enabled**

- Omit recovery options from the BitLocker setup wizard: **True**
- Allow data recovery agent: **False**
- Allow 256-bit recovery key:
- Configure storage of BitLocker recovery information to AD DS: **Store recovery passwords and key packages**
- Do not enable BitLocker until recovery information is stored to AD DS for operating system drives: **True**
- Save BitLocker recovery information to AD DS for operating system drives: **True**
- Configure user storage of BitLocker recovery information: **Allow 48-digit recovery password**
- 
- 



















