---
title: Hybrid Join vs AAD Join
description: The struggle of staying on-prem because it's "safe" vs. breaking your tech-debt shackles and moving to modern management.
published: true
date: 2023-07-10T17:40:35.043Z
tags: autopilot, intune, windows, best practice, haadj
editor: markdown
dateCreated: 2022-09-30T11:20:45.613Z
---

# Hybrid Join vs AAD Join

This question is asked more than any other in the community, and this page aims to cover some common myths/misconceptions around the main reasons many people believe they need to continue to be Hybrid Joined.

## Clearing Up Common Confusion
The word "Hybrid" is often used interchangably within discussions around AD/AAD/Intune, but there are two key differences:

### **Hybrid Identity**

Hybrid Identity is the technology that underpins almost all of the infrastructure to support an On-Premise + Microsoft Cloud environment. It is 100% required if running in this scenario and is not in question.

With the use and configuration of AAD Connect, an on-premise account and its authentication method is synced to Azure AD. This can extend on-premise accounts into the cloud and enable single-sign on capability across cloud services.

More Info:

* [What is hybrid identity with Azure Active Directory? - Microsoft Entra | Microsoft Learn](https://learn.microsoft.com/azure/active-directory/hybrid/whatis-hybrid-identity)
* https://aka.ms/goaadj

### **Hybrid Azure AD Join (HAADJ)**

Hybrid AD Join is the actual technology being discussed below:

## Common HAADJ Misconceptions

**1:**
> We need HAADJ to access file-shares on-prem!

> Assuming Hybrid Identity is configured appropriately through AAD Connect, a user account accessing a file-share via a fully cloud-native device will be able to access the share in exactly the same way as a domain-joined device would. 
You would still need line-of-sight to the server either physically or via a VPN.
{.is-danger}

**2:**
> We have ConfigMgr and need HAADJ to do keep using it!

> Using [co-management](https://learn.microsoft.com/en-gb/mem/configmgr/comanage/how-to-prepare-win10) via [cloud attach](https://learn.microsoft.com/en-us/mem/configmgr/cloud-attach/overview) and a [Cloud Management Gateway (CMG)](https://learn.microsoft.com/en-gb/mem/configmgr/core/clients/manage/cmg/overview), you can continue to use your on-premise infrastructure to deploy policies, applications and updates to endpoints.
{.is-danger}

**3:**
> We've got legacy apps that need us to be HAADJ!

> Unless the application uses device-based authentication (which is not common or a recommended practice), a cloud-native device would be able to access a web or thick-client app in the same way a domain-joined device would.
You would still need line-of-sight to the server either physically or via a VPN.
Consider publishing internal web apps through [Azure AD Application Proxy](https://learn.microsoft.com/en-us/azure/active-directory/app-proxy/application-proxy) and SaaS versions of your software which can integrate with Azure AD.
{.is-warning}

**4:**
> We have to be HAADJ as we use RADIUS/802.1x!

> This is a valid reason to require HAADJ as it requires device authentication, however to take a cloud-first mindset, you should investigate ways to transition away from using this, such as utilising user or certificate-based auth, or removing the requirement entirely.
{.is-success}

**5:**
> Devices are only secure if they're domain joined!

> This is incredibly outdated thinking and your infrastructure is likely weak in other areas, which may lead to a serious breach. 
Begin to review guidance on [Zero Trust identity and device access](https://learn.microsoft.com/en-us/microsoft-365/security/office-365-security/microsoft-365-policies-configurations) concepts and pivot thinking around security and risk. 
{.is-danger}

## When should you use HAADJ?

* HAADJ is fine for your existing, already domain-joined devices.
* There's no reason not to enable hybrid join for all existing domain-joined devices.
* You want to continue to use Group Policy to manage devices.
* Anything you continue to image for "reasons" in a traditional process should end up hybrid-joined.
* You need device-based Kerberos auth and you can't replace it with certificates.
* You have 802.1x infrastructure relying on computer objects in AD.

## Why shouldn't you use HAADJ

* Hybrid Autopilot is actually pretty easy to set up and get working (at least on-premises without VPN), but do you really need to do it? No, you most likely don't.
* Continue to provision old devices using existing processes, and only build new processes for things that will stay around.
* Why waste the time implementing Hybrid AAD if it will eventually go away, once everything is AADJ?

## When should you use Azure AD Join?

* Azure AD join is the preferred choice going forward
* Any user assigned device deployed via Autopilot should be user-driven Azure AD-only 99% of the time.

## How do I migrate from HAADJ to AADJ?

There is **no migration path** from HAADJ Devices to AADJ Devices that doesn't potentially compromise the device in a way that could cause significant issues in the future and be almost impossible to diagnose. The device should be wiped/reset and brought into AADJ via Autopilot either via attrition (user leaver/joiner process) or defined projects to bring devices into Modern Management.

There is **no pressure** to do this within a defined timescale. Your existing on-premise infrastructure will continue to manage those devices.

## External Links

* [Cloud native Windows endpoints | Microsoft Learn](https://learn.microsoft.com/en-gb/mem/cloud-native-endpoints-overview)
* [To AAD Join or Not … That is the Question](https://techcommunity.microsoft.com/t5/core-infrastructure-and-security/to-aad-join-or-not-that-is-the-question/ba-p/3435768)
* [HAADJ: Stop it, you're making it worse for yourself (mostly)](https://skiptotheendpoint.co.uk/haadj-stop-it-youre-making-it-worse-for-yourself-mostly/)
