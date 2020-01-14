---
layout: post
title:  "A df command tutorial"
date:   2020-01-14 16:44:00 -0600
permalink: /df-tutorial/
---
df is a very useful command in Linux that is used to see mounted file systems. It shows what partition are mounted on what paths including NFS shares, their sizes and what amount has been used. 

## Usage 

```
df
```
Shows mounted file systems on local server. The result includes the following parts.

- The filesystem (partition, LVM volumes, NFS shares, mounted media etc).
- The size in kilobytes.
- How much of the filesystem has been used in kB.
- How much is free in kB.
- The percentage of the FS that has been used.
- The path the filesystem has been mounted in.

```
df -h
```
Shows partition sizes in human readable form. 

```
df -hT
```

Shows partition sizes in human readable forms along with their types. 



