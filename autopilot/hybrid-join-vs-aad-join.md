---
title: Hybrid Join vs AAD Join
description: The struggle of staying on-prem because it's "safe" vs. breaking your tech-debt shackles and moving to modern management.
published: true
date: 2025-07-12T07:53:25.813Z
tags: autopilot, intune, windows, best practice, haadj
editor: markdown
dateCreated: 2022-09-30T11:20:45.613Z
---

# Hybrid Join vs Cloud Native Join

> The article uses Cloud Native Join, Entra Join and AAD Join synonymously.
{.is-info}


This question is asked more than any other in the community, and this page aims to cover some common myths/misconceptions around the main reasons many people believe their devices need to continue to be Hybrid Joined.

## Clearing Up Common Confusion
The word "Hybrid" is often used interchangably within discussions around Active Directory/Entra ID/Intune, but there are two key differences:

### **Hybrid Identity**

Hybrid Identity is the technology that underpins almost all of the infrastructure to support an On-Premise + Microsoft Cloud environment. It is 100% required if running in this scenario and is not in question.

With the use and configuration of Entra ID Connect, an on-premise account and its authentication method is synced to Entra ID. This can extend on-premise accounts into the cloud and enable single-sign on capability across cloud services.

* [What is hybrid identity with Azure Active Directory? - Microsoft Entra | Microsoft Learn](https://learn.microsoft.com/azure/active-directory/hybrid/whatis-hybrid-identity)

### **Microsoft Entra Hybrid Join (MEHJ)**

Entra Hybrid Join for devices is the actual technology being discussed below.

## Common Misconceptions

**1:**
> We need to be Hybrid to access file-shares on-prem!

> Assuming Hybrid Identity is configured appropriately through Entra ID Connect, a user account accessing a file-share via a fully cloud-native device will be able to access the share in exactly the same way as a domain-joined device would. 
You would still need line-of-sight to the server either physically or via a VPN.
{.is-danger}

**2:**
> We have ConfigMgr and need to be Hybrid to do keep using it!

> Using [co-management](https://learn.microsoft.com/en-gb/mem/configmgr/comanage/how-to-prepare-win10) via [cloud attach](https://learn.microsoft.com/en-us/mem/configmgr/cloud-attach/overview) and a [Cloud Management Gateway (CMG)](https://learn.microsoft.com/en-gb/mem/configmgr/core/clients/manage/cmg/overview), you can continue to use your on-premise infrastructure to deploy policies, applications and updates to endpoints.
{.is-danger}

**3:**
> We've got legacy apps that need us to be Hybrid!

> Unless the application uses device-based authentication (which is not common or a recommended practice), a cloud-native device would be able to access a web or thick-client app in the same way a domain-joined device would.
You would still need line-of-sight to the server either physically or via a VPN.
Consider publishing internal web apps through [Entra ID Application Proxy](https://learn.microsoft.com/en-us/entra/identity/app-proxy/application-proxy) and SaaS versions of your software which can integrate with Entra ID.
{.is-warning}

**4:**
> We have to be Hybrid as we use RADIUS/802.1x!

> This _*is*_ a valid reason to require MEHJ as it requires device authentication, however to take a cloud-first mindset, you should investigate ways to transition away from using this, such as utilising user or certificate-based auth, third-party integrations, or removing the requirement entirely.
{.is-success}

**5:**
> Devices are only secure if they're domain joined!

> This is incredibly outdated thinking and your infrastructure is likely weak in other areas, which may lead to a serious breach. 
Begin to review guidance on [Zero Trust identity and device access](https://learn.microsoft.com/en-us/microsoft-365/security/office-365-security/microsoft-365-policies-configurations) concepts and pivot thinking around security and risk. 
{.is-danger}

## When should you use Hybrid Joined Devices?

* Hybrid Join is **recommended** for your existing, already domain-joined devices! In fact there's no reason **not** to enable hybrid join for all existing domain-joined devices.
* You have to continue to use Group Policy to manage devices. (This refers to compliance reasons, not to technical reasons)
* Anything you continue to image for "reasons" in a traditional process should end up hybrid-joined.
* You need device-based Kerberos auth and you can't replace it with certificates.
* You have 802.1x infrastructure relying on computer objects in AD.

## Autopilot
![cloudnativerecommendation.png](/cloudnativerecommendation.png){.align-center}
> Source: [Enrollment for Microsoft Entra Hybrid Joined devices - Windows Autopilot | Microsoft Learn](https://learn.microsoft.com/en-us/autopilot/windows-autopilot-hybrid)

### Why *shouldn't* you Hybrid Join Autopilot

* For brand new devices, it is not recommended by Microsoft. (Or would you buy a bike that the salesperson advises you not to buy?)
* It can be hideously difficult to implement and even require deployment of new infrastructure depending on your environment. Implementation can be extremely difficult and, depending on the environment, may even require a new infrastructure to be set up. (e.g. a VPN solution)
* It has significantly more moving parts involved, and a failure in any of them will result in failed Autopilot builds.
* It keeps your system more complex (more points of failure, security risks, troubleshooting, etc.); hybrid joining even of existing devices serves to facilitate the transition to Entra Join.
* It keeps on-premises downsides (like needing line of sight to the DC to login, domain trust, etc.)
* If hybrid isn't needed, it builds up technical debt.

### Why *should* you use Cloud Native Join Autopilot

* Microsoft Entra ID Join is the **recommended, preferred and safe choice going forward**.
* It provides a significantly better end-user onboarding experience.
* With the right pre-requisites in place, most on-prem resource access will continue to work.
* New features such as [Autopilot device preparation](https://learn.microsoft.com/en-us/autopilot/device-preparation/overview) _require_ cloud native devices.

## How do I migrate from Hybrid to Cloud Native?

There is **no _supported_ migration path** from Hybrid Joined Devices to Cloud Native Devices that doesn't potentially compromise the device in a way that could cause significant issues in the future and be almost impossible to diagnose. The device should be wiped/reset and brought into being Cloud Native via Autopilot either via attrition (user leaver/joiner process) or defined projects to bring devices into Modern Management.

When you migrate the policies to Intune, ask yourself, do you REALLY need all the GPO's?

There is **no pressure** to do this within a defined timescale. Your existing on-premise infrastructure will continue to manage those devices.

## External Links

* [Cloud native Windows endpoints | Microsoft Learn](https://aka.ms/cloudnativeendpoints)
* [Road to the cloud - Implement a cloud-first approach when moving identity and access management from Active Directory to Azure AD | Microsoft Learn](https://learn.microsoft.com/en-us/azure/active-directory/architecture/road-to-the-cloud-implement)
* [To AAD Join or Not … That is the Question](https://techcommunity.microsoft.com/t5/core-infrastructure-and-security/to-aad-join-or-not-that-is-the-question/ba-p/3435768)
* [HAADJ: Stop it, you're making it worse for yourself (mostly)](https://skiptotheendpoint.co.uk/haadj-stop-it-youre-making-it-worse-for-yourself-mostly/)
* [Entra join only is a journey – are you on it yet?](https://manima.de/2024/10/entra-join-only-is-a-journey-are-you-on-it-yet/)
