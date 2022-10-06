---
title: Do You Need Seamless SSO?
description: 
published: true
date: 2022-10-06T20:18:03.687Z
tags: 
editor: markdown
dateCreated: 2022-10-06T20:18:03.687Z
---

Answer: Most likely, no. Seamless SSO (SSSO) is only used on downlevel OSs (e.g. Windows 7 and 8.1), and on Windows 10 devices that are not hybrid-joined. SSO on hybrid-joined Windows 10 devices functions via PRT.

https://learn.microsoft.com/azure/active-directory/hybrid/how-to-connect-sso#sso-via-primary-refresh-token-vs-seamless-sso

If you do require SSSO support, be sure to secure the `AZUREADSSOACC` computer object in a location in the directory that is only accessible by Domain Admins, and rotate the decryption key regularly (every 30 days is recommended).

https://learn.microsoft.com/azure/active-directory/hybrid/how-to-connect-sso-how-it-works#how-does-set-up-work
