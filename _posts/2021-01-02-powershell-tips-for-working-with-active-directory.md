---
layout: post
title: PowerShell tips for working with Active Directory
date: 2021-01-02T05:37:31.193Z
permalink: powershell-active-directory
---
Some PowerShell commands for AD that make my life easier.

Run these commands from a machine joined to an AD domain. Or install RSAT on your system and make sure that Active Directory Module for Windows PowerShell is enabled in RSAT.

Find all groups an AD user is member of

`Get-ADPrincipalGroupMembership username | select name`

Get AD User info

`Get-ADUser username -Properties *`

To get specific user attributes

`Get-ADUser username -Properties * | select attr`

Get AD group info

`Get-ADGroup grp`

Get AD group members

`Get-ADGroupMembers group | select name`