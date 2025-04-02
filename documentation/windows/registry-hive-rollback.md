---
layout: post
title: Registry Hive Rollback
date: 2025-04-02
parent: Windows
tags:
  - windows
  - registry
  - rollback
  - os repair
author: Kunal Shenoi
description: Rollback registry hives to resolve OS corruption
---


# {{ page.title }}

*Published on {{ page.date | date: "%B %d, %Y" }}*

*Tags: {{ page.tags | join: ", " }}*

## Introduction

Many issues in Windows can be caused by errors in the registry. A registry hive rollback allows you to revert to a previous version of the registry in an attempt to resolve these issues.

## Directions

1. Boot to a Windows RE or Windows PE
2. Identify the windows installation to repair (e.g. `C:\`)
3. Navigate to System32 
	1. `cd C:\Windows\System32`
4. List shadow volumes 
	1. `vssadmin list shadows`
5. Identify the most recent **Shadow Copy Volume**
	1. Ex. `\\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy1`
6. Create a symlink to the shadow
	1. `mklink /D C:\Shadow \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy1\`
7. Navigate to `windows\system32\config` in the Shadow symlink
	1. `cd C:\Shadow\windows\system32\config`
8. Verify the existence of `DEFAULT`, `SAM`, `SECURITY`, `SOFTWARE`, and `SYSTEM`
9. Rename current registry hives to `.old` or `.bak`
	1. `cd C:\Windows\System32\config`
	2. `ren {FILE} {FILE}.old` (`ren DEFAULT DEFAULT.old)
10. Copy the shadow registry hives to the current registry hive folder
	1. `copy C:\Shadow\windows\system32\config\default C:\Windows\System32\config`
	2. Repeat for `SAM`, `SECURITY`, `SOFTWARE`, and `SYSTEM`
11. Remove symlink `rmdir C:\Shadow`
12. Reboot