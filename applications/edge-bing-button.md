---
title: Remove Bing button from Edge
description: 
published: true
date: 2023-04-06T08:26:46.005Z
tags: edge, bing
editor: markdown
dateCreated: 2023-04-06T08:26:46.005Z
---

# How to remove the Edge Bing button.

Policy name: HubsSidebarEnabled
Supported versions:

- On Windows and macOS since 99 or later

### Description

Sidebar is a launcher bar on the right side of Microsoft Edge's screen.

If you enable or don't configure this policy, the Sidebar will be shown. If you disable this policy, the Sidebar will never be shown.
Supported features:

- Can be mandatory: Yes
- Can be recommended: Yes
- Dynamic Policy Refresh: Yes

### Data Type:

- Boolean

### Windows information and settings
#### Intune Settings Catalog
- Microsoft Edge > Show Hubs Sidebar

#### Group Policy (ADMX) info

- GP unique name: HubsSidebarEnabled
- GP name: Show Hubs Sidebar
- GP path (Mandatory): Administrative Templates/Microsoft Edge/
- GP path (Recommended): Administrative Templates/Microsoft Edge - Default Settings (users can override)/
- GP ADMX file name: MSEdge.admx

#### Windows Registry Settings

- Path (Mandatory): SOFTWARE\Policies\Microsoft\Edge
- Path (Recommended): SOFTWARE\Policies\Microsoft\Edge\Recommended
- Value Name: HubsSidebarEnabled
- Value Type: REG_DWORD

Example value:
```
0x00000001
```

### Mac information and settings

- Preference Key Name: HubsSidebarEnabled
- Example value:

```XML
<true/>
```
