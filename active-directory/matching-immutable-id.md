---
title: Hard Match AD and Entra ID user - Immutable ID
description: Connect On-Prem and Entra ID user in case of migration or similar
published: true
date: 2026-01-07T11:33:08.291Z
tags: immutable id, merge identities
editor: markdown
dateCreated: 2024-09-17T12:03:04.332Z
---

# Matching Immutable IDs Between Entra and On-Premises AD Accounts
## Introduction and Purpose
When managing users across both on-premises Active Directory (AD) and Entra, it's essential to maintain consistency between the two environments. The Immutable ID is a unique identifier that ensures a user’s cloud identity (in Entra) matches their on-premises identity (in AD). This synchronization is crucial when:

* A user moves between domains, and their on-premises AD account needs to be moved without losing any associated cloud data (e.g., emails, OneDrive files).
* An existing cloud-only Entra user needs to be synchronized back to an on-premises AD account.

By correctly matching the Immutable ID between Azure AD and on-premises AD, you can maintain data integrity and seamless access for the user across both environments.

# Prerequisites
Ensure the following PowerShell modules are installed:

Active Directory Module
Microsoft.Graph.Users Module

# Step-by-Step Instructions
## Fetch the Entra Immutable ID
Use the following PowerShell command to get the Immutable ID of the Azure AD user:

```
connect-mggraph
get-mguser -property OnPremisesImmutableId -userid "user@example.com" | select OnPremisesImmutableId 
```

This command will output the Immutable ID for the user.


## Retrieve the Immutable ID from On-Premises AD

To retrieve the corresponding Immutable ID from the on-prem AD user, use the following commands:

``` 
$guid = (Get-ADUser -Identity "username").ObjectGUID
$immutableID = [System.Convert]::ToBase64String($guid.ToByteArray())
$immutableID 
```
This converts the on-prem AD user's **ObjectGUID** into a base64-encoded string, which is the format used for the Immutable ID in Azure AD.

## Set the Immutable ID in Entra

To update the Entra user with the on-prem AD Immutable ID, run the following command:

``` 
Set-MgUser -UserId "user@example.com" -OnPremisesImmutableId "base64ImmutableID"
```

Replace "user@example.com" with the user's Entra Object ID and "base64ImmutableID" with the Base64 string obtained from the previous step.

## Sync Entra with On-Premises AD

Wait for the next scheduled synchronization, or force a synchronization using the following command on your Entra Connect server:

``` 
Start-ADSyncSyncCycle -PolicyType Delta 
```

# Notes

* Ensure that you have the necessary permissions to execute these commands.
* Verify that the Entra Connect server is correctly configured and running.

By following these steps, you can ensure that the on-premises AD user is correctly matched with the Entra user, preserving all cloud data and settings.