---
title: Windows Hello for Business - Cloud Kerberos Trust
description: 
published: true
date: 2023-05-28T20:02:08.942Z
tags: whfb
editor: markdown
dateCreated: 2023-03-31T14:54:12.491Z
---

# Windows Hello for Business - Cloud Kerberos Trust

In order to access on-premises resources with a hybrid (i.e. synced) identity on a Hybrid or Azure AD joined endpoint utilizing Windows Hello for Business, Cloud Kerberos Trust needs to be deployed. Previously, there were two other options to facilitate this, namely Key Trust and Certificate Trust, both which involve deploying certificates to some degree.

Cloud Kerberos Trust simplifies this configuration greatly, utilizing the exisiting technology that enabled SSO via FIDO2, Azure AD Kerberos.

## Prerequisites

1. Windows 10 21H2 or later
2. Enough Windows Server 2016 or later Domain Controllers to handle the expected authentication load (why aren't they all 2019+ already?)
3. User accounts expected to use WHfB synced to Azure AD
> WARNING: AD accounts that are a member of sensitive, highly privlidged groups such as Domain Admins, or otherwise inherit membership into `Denied RODC Password Replication Group` cannot utilize Cloud Kerberos Trust, as Azure AD Kerberos functions as a "virtual" RODC, and these accounts cannot auth against or have their password replicated to an RODC by default (and no, this should NOT be modified). Additionally, these accounts should not be synced to the cloud in the first place.
{.is-danger}
4. Azure AD Kerberos in place
5. Cloud Kerberos Trust settings in place (eith via GPO or Intune Settings Catalog configuration profile)

## Enabling Azure AD Kerberos

MS Documentation: https://learn.microsoft.com/azure/active-directory/authentication/howto-authentication-passwordless-security-key-on-premises#create-a-kerberos-server-object

> Note: Azure AD Kerberos must be enabled in EVERY domain across ALL forests that contain users accounts synced to AAD and expected to utilize WHfB.
{.is-info}

The easiest place to configure Azure AD Kerboros from is the server that runs Azure AD Connect, as this is considered a tier 0 server, and you will need to utilize Domain Admin and GLobal Admin credentials. The required PowerShell module will also be present in the AADC install directory.

1. Import the module
```powershell
Import-Module -FullyQualifiedName 'C:\Program Files\Microsoft Azure Active Directory Connect\AzureADKerberos\AzureAdKerberos.psd1'
```
2. Cofigure Azure AD Kerberos (replace Domain with your AD domain name, and UserPrincipalName with the UPN of your GA account)
```powershell
Set-AzureADKerberosServer -Domain 'ad.domain.tld' -UserPrincipalName 'ga@domain.onmicrosoft.com'
```
3. Verify the configuration
```powershell
Get-AzureADKerberosServer -Domain 'ad.domain.tld' -UserPrincipalName 'ga@domain.onmicrosoft.com'
```
Sample output:
```powershell
Get-AzureADKerberosServer -Domain 'corp.ajf.one' -UserPrincipalName 'ajf-ga@ajf.one'


Id                 : 11637
UserAccount        : CN=krbtgt_AzureAD,OU=Users,OU=T0,DC=corp,DC=ajf,DC=one
ComputerAccount    : CN=AzureADKerberos,OU=Domain Controllers,DC=corp,DC=ajf,DC=one
DisplayName        : krbtgt_11637
DomainDnsName      : corp.ajf.one
KeyVersion         : 864665
KeyUpdatedOn       : 3/15/2023 6:44:39 AM
KeyUpdatedFrom     : CORPDC02.corp.ajf.one
CloudDisplayName   : krbtgt_11637
CloudDomainDnsName : corp.ajf.one
CloudId            : 11637
CloudKeyVersion    : 864665
CloudKeyUpdatedOn  : 3/15/2023 6:44:39 AM
CloudTrustDisplay  : Microsoft.AzureAD.Kdc.Service.TrustDisplay
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

## Validating Functionality

soon...