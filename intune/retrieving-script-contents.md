---
title: Retrieving Script Contents
description: 
published: true
date: 2022-10-21T15:44:43.031Z
tags: base64, intune, win32, scripts
editor: markdown
dateCreated: 2022-10-21T14:46:26.209Z
---

# Retrieving sscript contents from intune

## win32 requirements script

1: go to the app in the intune portal and copy the GUID of the app from the URL

2: go to https://developer.microsoft.com/en-us/graph/graph-explorer and sign in.

3: in the query input box, paste the following: https://graph.microsoft.com/beta/deviceAppManagement/mobileApps/ and add your app GUID at the end.
you should have a string that looks similar to this: https://graph.microsoft.com/beta/deviceAppManagement/mobileApps/123456798abcd-1235-1234-1234-123456789abcd

4: run the query and look for "scriptContent" in the response preview window below

5: copy the value from "scriptContent" and paste it into https://www.base64decode.org/ to decode the script