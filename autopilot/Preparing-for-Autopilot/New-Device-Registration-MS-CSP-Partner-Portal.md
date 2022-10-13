---
title: New-Device-Registration-MS-CSP-Partner-Portal
description: How to import new devices in preparation for autopilot provisioning
published: true
date: 2022-10-13T20:06:04.464Z
tags: 
editor: markdown
dateCreated: 2022-10-13T20:02:43.812Z
---

New-Device-Registration-MS-CSP-Partner-Portal

Note: you can not use this method to autopilot Virtual Machines (only real devices because MS checks the real device SN against a known DB of devices)

Summary of steps from the Microsoft CSP Partner side (plain Text no screenshots)
1-	login to Ingram or TD SYNNEX
2-	collect the Service Tag, Manufacturer, Model from the hardware PO 
3-	create the assets in Fresh Service (Service Tag, Manufacturer, Model)
4-	login to MS Partner Center
5-	search and find the target tenant
6-	click on Devices
7-	select Add Devices 
8-	select Auto Pilot Devices from the “Select or enter a name for this batch of devices”
9-	copy the CSV template to your machine (CTRL +C > CTRL +V)
10-	clean up the CSV template
11-	populate the CSV file with 3 pieces of information (Service Tag, Manufacturer, Model)
-	Manufacturer: Dell Inc.
-	Service Tag is also the Serial number
12-	upload the populated CSV file to partner Center
a.	**At this step MS will verify the Device against a real database**
13-	Go back to Devices
14-	assign a profile to the new uploaded devices
15- Wait and be patient
16- unbox one of the machines for testing
17- connect to Wi-Fi
18- it will automatically reboot
19- it will say welcome to "Tenant name"
 
IT/Partner can find the device in three portals:
1-	AFTER IT/Partner adds it in the MS partner center
a.	Auto pilot portal
2-	AFTER the user or the admin logs in to the device at the “Welcome to FGC Health” screen in OOBE
a.	Azure AD devices portal (join)
b.	Intune devices portal (enrollment)
User instructions:
1-	Unbox the computer
2-	Plug in the power cord
3-	((If provided plug in the ethernet adapter to your computer and plug in the ethernet cable between your adapter and internet modem))
4-	Turn on the computer by pushing on the power button
5-	Go through the initial Out-of-the-Box-Experience (OOBE) screens
a.	First screen: Select the language and click Yes on the bottom right corner
i.	“Continue in Selected Language”
1.	English
2.	Spanish
3.	French
b.	Second screen:
i.	Let’s start with a region. Is this right? Select a region and click Yes on the bottom right corner
c.	Third screen:
i.	Is this the right keyboard layout? If you also use another keyboard layout, you can use that next. Select the keyboard layout and click Yes on the bottom right corner
d.	Forth screen:
i.	Want to add a second keyboard layout? Hit Skip at the bottom right corner
e.	Fifth screen: (Very important) – Do not skip this step!!!
i.	Let’s connect you to a network. To finish setup, you’ll need to connect to the internet
1.	Select a Wi-Fi connection and enter the password then hit connect
2.	Click “No” for “do you want to allow your pc to be discoverable by other pcs and devices on this network?” 
f.	Then your computer will go into settings things up and then it will reboot automatically
g.	After a reboot the device will say “Welcome to your organization name branded with the organization logo” on the screen
h.	Now you are ready to login with the user ID and password that was provided to you by your IT team
i.	you will be asked to setup/prove your Multi-factor Authentication using the Microsoft Authenticator on your phone
j.	After that you might see “Setting up your device for work”, allow for some time for this to finish
k.	Then you will be automatically logged in and presented with setting up Windows Hello for Business Biometrics like fingerprint or facial recognition
l.	Then you will be asked to setup your PIN (6-digit PIN – numbers only)
i.	You may use your PIN instead of the password day-to-day to unlock your user profile



