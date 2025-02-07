---
title: Strong Certificate Mapping Enforcement
description: 
published: true
date: 2025-02-07T16:29:08.279Z
tags: 
editor: markdown
dateCreated: 2025-01-29T14:28:09.017Z
---

## KB5014754: Certificate-based authentication changes on Windows domain controllers

Domain controllers will be moved to full enforcement for strong certificate mapping in the 2025-02 cumulative update. If you are utilizing certificate authentication for things like 802.1x/NAC/WiFi/etc, you *may* see issues, depending on how your infrastructure works and how you are delivering certificates. See the links below for more information.


## Additional Useful Links

* [KB5014754: Certificate-based authentication changes on Windows domain controllers]([https://support.microsoft.com/topic/kb5014754-certificate-based-authentication-changes-on-windows-domain-controllers-ad2c23b0-15d8-4340-a468-4d4f3b188f16)
* [MS Tech Community - Implementing strong mapping in Microsoft Intune certificates](https://techcommunity.microsoft.com/blog/intunecustomersuccess/support-tip-implementing-strong-mapping-in-microsoft-intune-certificates/4053376)
* [MS Docs - Changes required for SCEP certificate profiles](https://learn.microsoft.com/en-us/mem/intune/protect/certificates-profile-scep#update-certificate-connector-strong-mapping-requirements-for-kb5014754)
* [MS Docs - Changes required for PKCS certificate profiles](https://learn.microsoft.com/en-us/mem/intune/protect/certificates-pfx-configure#update-certificate-connector-strong-mapping-requirements-for-kb5014754)
* [Strong Certificate Mapping Enforcement February 2025 - Richard Hicks](https://directaccess.richardhicks.com/2025/01/27/strong-certificate-mapping-enforcement-february-2025/)
* [How to configure certificate-based WiFi with Intune - Oliver Kieselbach](https://oliverkieselbach.com/2024/02/21/how-to-configure-certificate-based-wifi-with-intune/)
* [Cisco ISE Field Notice - Software Upgrade Recommended](https://www.cisco.com/c/en/us/support/docs/field-notices/742/fn74227.html)
* [Jamf Pro - Supporting Microsoft Active Directory Strong Certificate Mapping Requirements](https://learn.jamf.com/en-US/bundle/technical-articles/page/Supporting_Microsoft_Active_Directory_Strong_Certificate_Mapping_Requirements.html)
