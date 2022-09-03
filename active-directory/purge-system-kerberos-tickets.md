---
title: Purge SYSTEM Kerberos Tickets
description: 
published: true
date: 2022-07-15T21:44:08.485Z
tags: 
editor: markdown
dateCreated: 2022-07-15T21:43:37.792Z
---

# Purge SYSTEM Kerberos Tickets

When you add a principal to an Active Directory group, you typically need to have the account log out and log back in, in order to "learn" about the new group membership. For users, this is obvious, but for computers, the only way to log out and back in is by restarting.

However, you can easily purge all Kerberos tickets for the SYSTEM account, as log as you have local admin rights:

```
klist.exe -li 0x3e7 purge
```