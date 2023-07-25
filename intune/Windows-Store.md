---
title: Windows Store
description: Provides brief important information about the Windows Store
published: false
date: 2023-07-25T19:41:32.958Z
tags: intune, windows, csp, mdm, windows store
editor: markdown
dateCreated: 2023-07-25T19:41:32.958Z
---

# Windows Store
This entry will try to point out typical misconceptions about all settings related to the Windows Store using Intune. Please not that it is specifically aimed at Intune using CSPs. 

## Block user access to the store
There is only one setting that is required to block the access to the store and its called "Require private store only". https://learn.microsoft.com/en-us/windows/client-management/mdm/policy-csp-applicationmanagement#requireprivatestoreonly 

1. It is possible to scope this setting as a user setting, so that an admin would have access to the store whereas a normal user would not. 