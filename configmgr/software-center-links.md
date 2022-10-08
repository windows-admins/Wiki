---
title: Software Center Links
description: Link directly to Software Center
published: true
date: 2022-09-30T11:26:06.105Z
tags: configmgr, software center
editor: markdown
dateCreated: 2022-09-30T11:21:03.650Z
---

# Linking to specific Software Center page
There is a URI handler for Software Center which is generally used for sharing Application links by end users.

The format is: 
`softwarecenter:SoftwareID=ScopeId_143AB326-A02F-40FF-8B8E-AD7E2A423159/Application_5ae88719-501f-4f22-84de-7abccca3ecd1`

However, what if we don't want to link to an Application? Well we can get half way there and link to the specific tab that's required.
Here are the links to each tab for Software Center.

Applications – `softwarecenter:Page=AvailableSoftware`
Updates – `softwarecenter:Page=Updates`
Operating Systems – `softwarecenter:Page=OSD`
Installation Status – `softwarecenter:Page=InstallationStatus`
Device Compliance –  `softwarecenter:Page=Compliance`
