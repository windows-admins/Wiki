---
title: User Offboarding Admin Takeover
description: Instructions for administrative takeover of an account, generally as part of an offboarding process.
published: true
date: 2022-09-29T22:16:00.512Z
tags: offboarding
editor: markdown
dateCreated: 2022-08-22T19:37:37.794Z
---

# Administrative takeover of Microsoft 365 resources
As part of your offboarding process, you may wish to maintain control or transfer ownership of various resources created within a Microsoft 365 user account. 

Within this article you will find instructions for completing this process with various Microsoft 365 services.


## OneDrive

OneDrive access can be provided to a user by making them a site collection administrator for the offboarded user's personal site. This can be accomplished via the web or via PowerShell.

### Web-based 

1. Browse to your SharePoint admin center.
1. Select the `More features` menu item from the menu.
1. Select `Open` under the User Profiles section.
1. Select the `Manage user profiles` option under the People section.
1. Enter the offboarded user's name in the Find Profiles section, then select `Find`.
1. Select the offboarded user, then select `Manage site collection owners`
1. Add the user who should have access as a site collection administrator, then press OK.
	1. If the offboarded user has already had their licensing removed, you will need to remove them from both sections or your changes will not save.
  
### Powershell

The SharePoint Online Powershell module can be used to add the required permissions as in the example below.

```
$AdminURL = "https://winadmins-admin.sharepoint.com"
$OffboardedUserURL = "https://winadmins-my.sharepoint.com/personal/justin_winadmins_io"
$AddingUser = "someoneelse@winadmins.io"
 
Connect-SPOService -Url $AdminURL
 
Set-SPOUser -Site $OffboardedUserURL -LoginName $AddingUser -IsSiteCollectionAdmin $True
```

### Access
Once completed, the offboarded user's OneDrive will be accessible to the specified user via the web. 

It can be accessed via a link similar to: `https://winadmins-my.sharepoint.com/personal/justin_winadmins_io`

- The SharePoint domain will need to be customized to match your Microsoft 365 tenant.
- at (@) and period (.) symbols in the offboarded user's UPN must be replaced with underscores (_).

## Forms

Migrating Microsoft Forms is done via a special delegation page.

1. Browse to `https://forms.office.com/Pages/delegatepage.aspx?originalowner=[offboarded user's email address]`
	- Such as: `https://forms.office.com/Pages/delegatepage.aspx?originalowner=justin@winadmins.io`
1. For each form listed, select the menu button, then select Move.
1. Specify where you'd like the form to be moved to.

Source: [Microsoft Docs](https://docs.microsoft.com/en-us/microsoft-forms/admin-information#form-ownership-transfer)

## Power Automate: Flows

Ownership of Power Automate flows can be added via PowerShell using the Microsoft.PowerApps.Administration.PowerShell package.

First, get the GUID of the offboarded user:

```
$OffboardedGUID = (Get-AzureADUser -ObjectId justin@winadmins.io).ObjectId
```

Then, get the GUID of the user to be added:

```
$AddingGUID = (Get-AzureADUser -ObjectId notjustin@winadmins.io).ObjectId
```

Then, get all of the offboarded user's flows and add the new owner:

```
Get-AdminFlow -CreatedBy $OffboardedGUID | Set-AdminFlowOwnerRole -PrincipalType User -PrincipalObjectId $AddingGUID
```

> As a flow can have multiple owners, this is effectively adding an owner rather than replacing them.
{.is-info}


