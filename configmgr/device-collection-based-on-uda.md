---
title: Device Collection based on User Device Affinity
description: How to create a collection based on user device affinity (primary user) and also their AD department attribute
published: true
date: 2022-09-30T08:14:16.032Z
tags: 
editor: markdown
dateCreated: 2022-09-30T07:57:20.132Z
---

# Preface
This post assumes you have tweaked and perfected User and Device Affinity in client settings for your org.

You must configure **Active Directory User Discovery** to include the "department" attribute.

This only really works in practice if your AD Users have a standard format for the department. e.g. All personnel in IT department have "IT & Digital" in the department field.

# Create collection
Create a Device Collection with the following query:

```sql
SELECT
    SMS_R_SYSTEM.ResourceID,
    SMS_R_SYSTEM.ResourceType,
    SMS_R_SYSTEM.Name,
    SMS_R_SYSTEM.SMSUniqueIdentifier,
    SMS_R_SYSTEM.ResourceDomainORWorkgroup,
    SMS_R_SYSTEM.Client
FROM
    SMS_R_System
    JOIN SMS_UserMachineRelationship ON SMS_R_System.Name = SMS_UserMachineRelationship.ResourceName
    JOIN SMS_R_User ON SMS_UserMachineRelationship.UniqueUserName = SMS_R_User.UniqueUserName
WHERE
    SMS_UserMachineRelationship.Types = 1
    AND SMS_R_User.department LIKE "IT%Digital"
```

Replace "IT%Digital" with department of choice. Uses wildcard to catch minor typos.