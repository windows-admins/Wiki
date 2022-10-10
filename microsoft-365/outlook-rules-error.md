---
title: Outlook rules error
description: How to fix MS Outlook rules error
published: true
date: 2022-10-10T13:50:16.078Z
tags: m365apps outlook
editor: markdown
dateCreated: 2022-10-10T13:50:16.078Z
---

# Issue

When opening Outlook and/or opening the Manage Rules and Alerts screen, you receive the error:

`There was an error reading the rules from the server. The format of the server rules was not recognized.`

![applications-office-outlook--corrupt-rules-image1.png](/applications-office-outlook--corrupt-rules-image1.png)

# Resolution

Open the Manage Rules and Alerts

You'll notice there is a ` (client-only)` rule with no name. Click **Options** at top right.

![applications-office-outlook--corrupt-rules-image2.png](/applications-office-outlook--corrupt-rules-image2.png)

- Export the rules out to a local file.
- Delete all the rules.
- Restart outlook.
- Open Rules again, if you see (client-only) rules again delete them all.
- Create any simple test rule and save it.
- Restart Outlook.
- Open Rules, this time you will be asked to choose which rules you want to use "client" or "server". 
- Choose client.
- Import the saved rules from step 1.