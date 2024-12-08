---
title: Troubleshooting Reboots
description: Troubleshooting reboots during device phase of autopilot.
published: true
date: 2024-12-08T14:47:34.247Z
tags: autopilot, esp, reboots
editor: markdown
dateCreated: 2024-12-08T14:47:34.247Z
---

# Troubleshooting	
Some policies applied to the device can force it to reboot during the device phase of Autopilot, leading to a less than desirable user experience.

Microsoft has some of these policies that conflict with autopilot noted here:
[What are some of the known policies that conflict with Windows Autopilot?](https://learn.microsoft.com/en-us/autopilot/troubleshooting-faq#what-are-some-of-the-known-policies-that-conflict-with-windows-autopilot-)

# How to fix this
To resolve these issues, identify the policy causing the reboot in Autopilot. Once located, reassign the policy from a device assignment to a user assignment.

[Policy Assignments - Support Matrix](https://learn.microsoft.com/en-us/mem/intune/configuration/device-profile-assign#support-matrix)

# Identify unexpected reboots
Reboots are supported on the ESP during the device setup phase (not supported during the account setup phase). 
Reboots during the device ESP process must be managed by Intune. For example, in the created package, you should specify the return codes to perform a reboot by Intune. There are some policies that conflict with the ESP and Microsoft is aware of them. For unexpected reboots, you can use the reboot-URI CSP to detect what triggers a reboot. In Event Viewer, an event is logged as follows:

```
channel="MDM_DIAGNOSTICS_ADMIN_CHANNEL"
level="win:Informational"
message="$(string.EnterpriseDiagnostics.RebootRequiredURI)"
symbol="RebootRequiredURI"
template="OneString"
value="2800"
```

The following sample event indicates which URI triggers a coalesced reboot:

`[ETW [2022-08-02T13:28:10.3350735Z] [Microsoft-Windows-DeviceManagement-Enterprise-Diagnostics-Provider]`
`[Informational] - The following URI has triggered a reboot:`
`(./Device/Vendor/MSFT/Policy/Config/Update/ManagePreviewBuilds)"`

For more information about how to identify unexpected reboots during the OOBE flow, see [Troubleshooting unexpected reboots](https://techcommunity.microsoft.com/t5/intune-customer-success/support-tip-troubleshooting-unexpected-reboots-during-new-pc/ba-p/3896960).

[Original Source](https://learn.microsoft.com/en-us/troubleshoot/mem/intune/device-enrollment/understand-troubleshoot-esp#identify-unexpected-reboots)