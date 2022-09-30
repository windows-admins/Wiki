---
title: Site Upgrades
description: 
published: true
date: 2022-09-30T14:07:33.946Z
tags: 
editor: markdown
dateCreated: 2022-05-14T19:10:15.796Z
---

Looking for a good starter guide/checklist for performing site upgrades? Look no further!

## Optional, but recommended pre-upgrade steps:

1. Verfiy all ConfigMgr site system servers are fully patched and recently rebooted.
2. Verfiy SQL is update to date with the latest SP/CU.
3. Upgrade your ADK installation if desired.

## Upgrade steps

1. Verify your most recent site backup completed successfully.
2. Verify site/component status, and resolve any errors/warnings.
3. Run the pre-req check for the update you intend to install.
4. Resolve any blocking errors listed in the pre-req check results, and determine if any warnings are a concern to you. Warnings can be bypassed via a checkbox in the install wizard; errors cannot.
5. Install the update, and monitor logs (`cmupdate.log`, `hman.log`,and `sitecomp.log` are all good logs to monitor).
6. Upgrade all consoles.
7. Update the client in boot images.
8. Update the client installed via software update (if used).
9. Verfiy site functionality.
10. Crack open a beverage of choice, you're done!
