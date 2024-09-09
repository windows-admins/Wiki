---
title: Windows Hello for Business - Cloud Kerberos Trust
description: 
published: true
date: 2024-09-09T19:18:24.529Z
tags: whfb
editor: markdown
dateCreated: 2023-03-31T14:54:12.491Z
---

# Windows Hello for Business - Cloud Kerberos Trust

In order to access on-premises resources (such as file shares), with a hybrid (i.e. Entra ID synced) identity, on a Hybrid AD or Entra ID joined endpoint utilizing Windows Hello for Business, Cloud Kerberos Trust needs to be deployed. Previously, there were two other trust types to facilitate this, namely Key Trust and Certificate Trust, both which involve deploying certificates to some degree.

Cloud Kerberos Trust simplifies this configuration greatly, utilizing the exisiting technology that enabled SSO via FIDO2, Entra Kerberos (formally known as "Azure AD Kerberos").

## Prerequisites

1. Windows 10 21H2 or later
2. Enough Windows Server 2016 or later Domain Controllers to handle the expected authentication load (why aren't they all 2022 already?)
3. User accounts expected to use WHfB synced to Entra ID
> WARNING: AD accounts that are a member of sensitive, highly privileged groups such as `Domain Admins`, or otherwise inherit membership into `Denied RODC Password Replication Group` cannot utilize Cloud Kerberos Trust, as Entra Kerberos functions as a "virtual" RODC.
>
> These accounts cannot authenticate against or have their password replicated to an RODC by default (and no, this should NOT be modified). Additionally, these accounts should not be synced to the cloud in the first place.
>
> Check the `adminCount` attribute of the account if you are unsure; if this attribute is set to `1`, this indicitates that it currently **is** or at one time **was** highly privileged.
{.is-danger}
4. Entra Kerberos in place in EVERY domain in EVERY forest containing user accounts that are synced to Entra ID and expected to utilize WHfB.
5. Cloud Kerberos Trust settings in place on endpoints (eith via GPO or Intune Settings Catalog configuration profile)

## Enabling Entra Kerberos

MS Documentation: https://learn.microsoft.com/azure/active-directory/authentication/howto-authentication-passwordless-security-key-on-premises#create-a-kerberos-server-object

> Note: Entra Kerberos must be enabled in EVERY domain across ALL forests that contain users accounts synced to Entra ID and expected to utilize WHfB.
{.is-info}

The easiest place to configure Azure AD Kerboros from is the server that runs Azure AD Connect, as this is considered a tier 0 server, and you will need to utilize Domain Admin and Global Admin/Hybrid Identity Administrator credentials. The required PowerShell module will also be present in the AADC install directory.

1. Import the module
```powershell
Import-Module -FullyQualifiedName 'C:\Program Files\Microsoft Azure Active Directory Connect\AzureADKerberos\AzureAdKerberos.psd1'
```
2. Cofigure Entra Kerberos (replace Domain with your AD domain name, and UserPrincipalName with the UPN of your GA account)
```powershell
Set-AzureADKerberosServer -Domain 'ad.domain.tld' -UserPrincipalName 'ga@domain.onmicrosoft.com'
```
3. Verify the configuration
```powershell
Get-AzureADKerberosServer -Domain 'ad.domain.tld' -UserPrincipalName 'ga@domain.onmicrosoft.com'
```
Sample output:
```powershell
Get-AzureADKerberosServer -Domain 'ad.domain.tld' -UserPrincipalName 'ga@domain.onmicrosoft.com'


Id                 : 31787
UserAccount        : CN=krbtgt_AzureAD,CN=Users,DC=ad,DC=domain,DC=tld
ComputerAccount    : CN=AzureADKerberos,OU=Domain Controllers,DC=ad,DC=domain,DC=tld
DisplayName        : krbtgt_31787
DomainDnsName      : ad.domain.tld
KeyVersion         : 46771
KeyUpdatedOn       : 6/23/2024 8:05:44 PM
KeyUpdatedFrom     : DC01.ad.domain.tld
CloudDisplayName   : krbtgt_31787
CloudDomainDnsName : ad.domain.tld
CloudId            : 31787
CloudKeyVersion    : 46771
CloudKeyUpdatedOn  : 6/23/2024 8:05:44 PM
CloudTrustDisplay  :
```

## Configuring Cloud Kerberos Trust Settings

MS Docs: https://learn.microsoft.com/windows/security/identity-protection/hello-for-business/hello-hybrid-cloud-kerberos-trust-provision?tabs=intune#configure-windows-hello-for-business-policy

Cloud Kerberos Trust can be enabled via GPO, or via a Configuration Profile in Intune. If using Intune, this setting is now in the Settings Catalog, so there is no longer a need to use a custom OMA-URI setting.

### Intune

1. Create a new Configuration Profile, selecting "Windows 10 and later", and "Settings catalog"
2. Give it a name, such as "Cloud Kerberos Trust", and click Next
3. Click "Add Settings", and search for "Cloud Trust"
4. Select the setting to add it to your profile and enable it:

![cloud-trust-01.png](/cloud-trust-01.png)

5. Continue clicking Next and configure Scope Tags and Assignments as desired, and click Create

### GPO

> Ensure your ADMX templates are up to date if you cannot find the specified setting
{.is-info}

1. Create a new GPO, or edit an existing one
2. Browse to Computer Configuration -> Administrative Templates -> Windows Components -> Windows Hello for Business
3. Configure "Use cloud trust for on-premises authentication"

![cloud-trust-02.png](/cloud-trust-02.png)

4. Save the GPO, and link it to the appropriate OU(s)

## Validating Functionality & Troubleshooting

Once configured, attempt to access an on-premises resource such as a file share while signed in using WHfB, and it should seamless open. One issue that we've seen pop up from time to time is you still get an auth prompt, and resetting the WHfB cert container and re-enrolling WHfB seems to be a quick and easy solution:

1. Open cmd as admin
2. Run `certutil -deleteHelloContainer`
3. Log off and back on with password, and re-enroll WHfB
4. Log off and back on with WHfB, and attempt to access on-premises resources again
5. Profit!

### Browsing to my internal website using IWA throws up an auth prompt?!

This is most likely due to you internal site not being added to the "intranet" site to zone mapping, which allows for automatic signin using logged on credentials. Add your internal site's URL(s) to the intranet zone via Intune configuration and this should start working.