---
title: Time Synchronization
description: 
published: true
date: 2022-10-01T14:13:16.271Z
tags: 
editor: markdown
dateCreated: 2022-10-01T14:13:16.271Z
---

Use GPO with a WMI Filter to force the PDC emulator to be the authoritative time source.

```sql
SELECT DomainRole FROM Win32_ComputerSystem WHERE DomainRole = 5
```

|Domain Role|Number|
|-|-|
|Standalone Workstation|0|
|Member Workstation|1|
|Standalone Server|2|
|Member Server|3|
|Backup Domain Controller|4|
|Primary Domain Controller|5|

References:

* https://learn.microsoft.com/archive/blogs/nepapfe/its-simple-time-configuration-in-active-directory
* https://learn.microsoft.com/windows/win32/cimwin32prov/win32-computersystem
