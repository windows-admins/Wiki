---
title: Filters
description: Filters allow easy, more granular control over assignment scope of policies and apps.
published: true
date: 2022-07-12T13:56:51.839Z
tags: intune, policy, apps, filters
editor: markdown
dateCreated: 2022-07-12T13:56:48.699Z
---

# Filters

> MEM Portal Links (all pages do the same thing):-
>
> - [Devices - Microsoft Endpoint Manager admin center](https://endpoint.microsoft.com/#view/Microsoft_Intune_DeviceSettings/TenantAdminMenu/~/assignmentFilters)
> - [Apps - Microsoft Endpoint Manager admin center](https://endpoint.microsoft.com/#view/Microsoft_Intune_DeviceSettings/TenantAdminMenu/~/assignmentFilters)
> - [Tenant admin - Microsoft Endpoint Manager admin center](https://endpoint.microsoft.com/#view/Microsoft_Intune_DeviceSettings/TenantAdminMenu/~/assignmentFilters)
>   {.is-info}

## Purpose

When you create an app, config or compliance policy, you can use filters to assign it based on rules you create. A filter allows you to narrow the assignment scope of a policy. For example, use filters to target devices with a specific OS version, device manufacturer, target only personal devices or only organization-owned devices, and more.

Be aware that while filters share a similar syntax to AAD Dynamic Groups, they have a limited set of available properties. They are however far more performant at scale than utilising complex Dynamic Device Group rules, so try to use them instead of creating a new group (where possible).

> Filters are not supported for all workloads!
> For current filter support, refer to: https://learn.microsoft.com/en-us/mem/intune/fundamentals/filters-supported-workloads
> {.is-warning}

<br>

![How Filters Work](https://docs.microsoft.com/en-us/mem/intune/fundamentals/media/filters/admin-creates-filter.png)
How Filters Work[^1]
[^1]: How Filters Work image from https://learn.microsoft.com/en-us/mem/intune/fundamentals/filters

---

## References & Links

> - [Intune grouping, targeting, and filtering: recommendations for best performance - Microsoft Tech Community](https://techcommunity.microsoft.com/t5/intune-customer-success/intune-grouping-targeting-and-filtering-recommendations-for-best/ba-p/2983058)
> - https://docs.microsoft.com/en-us/mem/intune/fundamentals/filters
> - https://docs.microsoft.com/en-us/mem/intune/fundamentals/filters-device-properties
>   {.is-info}
