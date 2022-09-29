---
title: HAADJ vs AAD Join
description: 
published: true
date: 2022-09-29T21:13:58.494Z
tags: 
editor: markdown
dateCreated: 2022-04-15T22:42:17.174Z
---

# Which one should you use?


## When should you use HAADJ?
- haadj is fine for your existing already domain joined devices.

- there's no reason not to enable hybrid join for all existing domain joined devices

- anything you continue to image for "reasons" in a traditional process should end up hybrid joined

## when you shouldn't use HAADJ



- Hybrid Autopilot is actually pretty easy to set up and get working (at least on-perm without VPN), but do you really need to do it? no, you most likely don't. continue to provision old devices using existing processes, and only build new processes for things that will stay around. why waste the time implementing hybrid AP if it will eventually go away, once everything is AADJ?

- 


## When should you use Azure AD Join?

- Azure AD join is the preferred choice going forward

- Any user assigned device deployed via Autopilot should be user-driven Azure AD-only 99% of the time.