---
title: Encoding and Decoding Strings
description: Useful snippets and functions for encoding and decoding strings.
published: true
date: 2022-09-30T11:32:11.223Z
tags: powershell, strings, encoding, decoding, base64, urlencode
editor: markdown
dateCreated: 2022-09-29T20:43:30.153Z
---

There are many useful string encoding / decoding functions in PowerShell obscured in various .NET classes which are often hard to remember. So here are some helpful PowerShell function wrappers for your PowerShell Profile file.

## Base 64 Encode / Decode

These two functions handle converting a string to/from Base64.

```powershell
function ConvertFrom-Base64String ([String]$InputString, [System.Text.Encoding]$Encoding) {
    $DecodedString = $Encoding.GetString([System.Convert]::FromBase64String($InputString))
    $DecodedString
}

function ConvertTo-Base64String ([String]$InputString, [System.Text.Encoding]$Encoding) {
    $StringBytes = $Encoding.GetBytes($InputString)
    $EncodedString = [System.Convert]::ToBase64String($StringBytes)
    $EncodedString
}
```

You can add these to your PowerShell profile `code $PROFILE` and then invoke them as follows:

```powershell
# Convert to Base64
ConvertTo-Base64String -InputString 'WinAdmins.io' -Encoding ([System.Text.Encoding]::UTF8) # V2luQWRtaW5zLmlv
# Convert from Base64
ConvertFrom-Base64String -InputString 'V2luQWRtaW5zLmlv' -Encoding ([System.Text.Encoding]::UTF8) # WinAdmins.io
```

## URL Encode / Decode

These two functions handle URL encoding/decoding a string.

```powershell
function ConvertFrom-URLEncodedString ([String]$InputString, [System.Text.Encoding]$Encoding) {
    $DecodedString = [System.Web.HttpUtility]::UrlDecode($InputString)
    $DecodedString
}

function ConvertTo-URLEncodedString ([String]$InputString) {
    $EncodedString = [System.Web.HttpUtility]::UrlEncode($InputString)
    $EncodedString
}
```

You can add these to your PowerShell profile `code $PROFILE` and then invoke them as follows:

```powershell
# URL encode
ConvertTo-URLEncodedString -InputString '#WinAdmins.io%&' # %23WinAdmins.io%25%26
# URL decode
ConvertFrom-URLEncodedString -InputString '%23WinAdmins.io%25%26' # #WinAdmins.io%&
```