---
title: Windows Hello for Business - Cloud Kerberos Trust
description: 
published: true
date: 2025-07-21T00:43:53.069Z
tags: 
editor: markdown
dateCreated: 2023-03-31T14:54:12.491Z
---

# Windows Hello for Business - Cloud Kerberos Trust

In order to access on-premises resources (such as file shares), with a hybrid (i.e. Entra ID synced) identity, on a Hybrid AD or Entra ID joined endpoint utilizing Windows Hello for Business, Cloud Kerberos Trust needs to be deployed. Previously, there were two other trust types to facilitate this, namely Key Trust and Certificate Trust, both which involve deploying certificates to some degree.

Cloud Kerberos Trust simplifies this configuration greatly, utilizing the exisiting technology that enabled SSO via FIDO2, Entra Kerberos (formally known as "Azure AD Kerberos").

## Prerequisites

1. Windows 10 21H2 or later
2. Enough Windows Server 2016 or later Domain Controllers to handle the expected authentication load (why aren't they all 2025 already?)
3. User accounts expected to use WHfB synced to Entra ID
> WARNING!
>
> AD accounts that are a member of sensitive, highly privileged groups such as `Domain Admins`, or otherwise inherit membership into `Denied RODC Password Replication Group` cannot utilize Cloud Kerberos Trust, as Entra Kerberos functions as a "virtual" RODC.
>
> These accounts cannot authenticate against or have their password replicated to an RODC by default (and no, this should NOT be modified). Additionally, these accounts should not be synced to the cloud in the first place.
>
> Check the `adminCount` attribute of the account if you are unsure; if this attribute is set to `1`, this indicitates that it currently **is** or at one time **was** highly privileged.
>
> Additionally, if the user is not a member of `Domain Users` for some reason, this will prevent things from working "by default", as this is how users are able to use Entra Kerberos.
{.is-danger}
* You can test if an account can authenticate using Entra Kerberos by checking if password replication to the AzureADKerberos RODC object is allowed by doing the following:
  1. Open the properties window for the AzureADKerberos object and go to the Password Replication Policy tab
  2. Click Advanced, and select the Resultant Policy tab
  3. Click Add, and search for and select the user account(s) you want to check
  4. View the results. In the below example, `ajf` is a normal user account, and is allowed. `ajf-da` is a member of `Domain Admins` and thus is denied.

  ![aadk-rodc-props.png](/aadk-rodc-props.png)

4. Entra Kerberos in place in EVERY domain in EVERY forest containing user accounts that are synced to Entra ID and expected to utilize WHfB.
5. Cloud Kerberos Trust settings in place on endpoints (eith via GPO or Intune Settings Catalog configuration profile)

## Enabling Entra Kerberos

MS Documentation: https://learn.microsoft.com/azure/active-directory/authentication/howto-authentication-passwordless-security-key-on-premises#create-a-kerberos-server-object

> Note: Entra Kerberos must be enabled in EVERY domain across ALL forests that contain users accounts synced to Entra ID and expected to utilize WHfB.
{.is-info}

> Note: The `AzureADHybridAuthenticationManagement` PowerShell module does not seem to work 100% in PowerShell 7. Use this module with PowerShell 5.1 for the best results.
{.is-danger}

The easiest place to configure Entra Kerberos from is the server that runs Entra Connect, as this is considered a tier 0 server, and you will need to utilize Domain Admin and Global Admin/Hybrid Identity Administrator credentials.

```powershell
# Install and import the required PowerShell module
Install-Module -Name AzureADHybridAuthenticationManagement
Import-Module -Name AzureADHybridAuthenticationManagement

# Cofigure Entra Kerberos (replace Domain with your AD domain name, and UserPrincipalName with the UPN of your GA account)
Set-AzureADKerberosServer -Domain 'ad.domain.tld' -UserPrincipalName 'ga@domain.onmicrosoft.com'

# Verify the configuration
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

## Rotating the Entra Kerberos Keys

Entra Kerberos functions as a "virtual" RODC, and has a krbtgt account assoicated with it. The keys for this account should be periodically rotated, similar to rotating the domain krbtgt keys (see https://aka.ms/krbtgt for a script for doing that). The Entra Kerberos keys need to be rotated with a specific command in order to ensure the new keys are synced to Entra.

MS Docs: https://learn.microsoft.com/en-us/entra/identity/authentication/howto-authentication-passwordless-security-key-on-premises#rotate-the-microsoft-entra-kerberos-server-key

```powershell
Import-Module -Name AzureADHybridAuthenticationManagement

# Rotate keys
Set-AzureADKerberosServer -Domain ad.domain.com -UserPrincipalName 'ga@domain.onmicrosoft.com' -RotateServerKey

# Verify key rotation
Get-AzureADKerberosServer -Domain ad.domain.com -UserPrincipalName 'ga@domain.onmicrosoft.com'
```

See the sample about in the previous section and confirm that the `KeyUpdatedOn` and `CloudKeyUpdatedOn` attributes match and that they are current.

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

### Be sure Certificate Trust is NOT also enabled

As mentioned eariler, there are two other trust types, certificate and key. While there are no settings to "enable" key trust, as it is always attempted if nothing else is enabled, certificate trust, if enabled, will take priority over cloud trust. If cloud trust is going to be used, be sure to disable any existing settings for certificate trust,

### Browsing to my internal website using IWA throws up an auth prompt?!

This is most likely due to you internal site not being added to the "intranet" site to zone mapping, which allows for automatic signin using logged on credentials. Add your internal site's URL(s) to the intranet zone via Intune configuration and this should start working.

### Always On VPN Issues

You may face an issue where on-premises resource access works with direct LoS, but not when using AOVPN. Richard Hicks has a few articles related to this:

* https://directaccess.richardhicks.com/2019/05/20/always-on-vpn-clients-prompted-for-authentication-when-accessing-internal-resources/
* https://directaccess.richardhicks.com/2021/09/20/always-on-vpn-short-name-access-failure/

This may be caused by AOVPN authenticating via certificate, and then that being attempted to access further resources, instead of Kerberos. A potential solution to try is the following:

"To resolve this issue, edit the rasphone.pbk file and change the value of UseRasCredentials to 0. Rasphone.pbk can be found in the $env:AppData\Microsoft\Network\Connections\Pbk folder."

Richard has a script in his GitHub that can be used to modify this file: https://github.com/richardhicks/aovpn/blob/master/Update-Rasphone.ps1

