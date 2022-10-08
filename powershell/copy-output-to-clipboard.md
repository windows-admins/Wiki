---
title: Copy Output to Clipboard
description: When you're working in PowerShell, it's sometimes useful to have the ability to send data right to your clipboard
published: true
date: 2022-10-05T22:18:38.205Z
tags: powershell, tips-and-tricks
editor: markdown
dateCreated: 2022-04-20T17:51:22.763Z
---

# Piping Output to your Clipboard
There are a few different ways to send your PowerShell prompts results to your computers clipboard. You might wonder, "why do I care" well I care because I'm lazy. Here are a few different methods you can use to make life just a little bit faster and more precise. 

## The Clip Method

```PowerShell
$ENV:COMPUTERNAME | Clip
```
Clip is a tiny binary on your computer. You can use it in both powershell AND command prompt. It's pretty neat. However it does occasionally have the drawback of including an extra return field.

## Set-Clipboard Method "more powershelly"

```PowerShell
$ENV:COMPUTERNAME | Set-ClipBoard
```

That's a lot more letters for the same functionality... well ish. Set clipboard handles copying objects a LOT better when it comes to objects and things with a return character. 

## A fun example of why you might use this: 

```PowerShell
(Get-FileHash .\data.csv -Algorithm SHA256).hash | clip
```

Say for example you wanted the hash of a file so you could look it up on Virus Total or something..

https://twitter.com/JordanTheITguy/status/1503416496445345796?s=20&t=EgAV8_qJC0eNQDJsmrrC1w