---
layout: post
title: Bypass Microsoft Account Sign In on Windows 11 Setup
date: 2025-04-02
parent: Windows
tags:
  - windows
  - os
  - oobe
  - ms
  - microsoft
author: Kunal Shenoi
description: Rollback registry hives to resolve OS corruption
---


# {{ page.title }}

*Published on {{ page.date | date: "%B %d, %Y" }}*

*Tags: {{ page.tags | join: ", " }}*

## Introduction

The following directions are intended to allow you to create a local account in the setup screen for Windows 11.

## Directions

### Option 1 (bypassnro)

*Note: If the device is in S-Mode, you will need to disable driver signature enforcement in order to launch cmd*

1. When in OOBE, Press `SHIFT` + `F10` to open command prompt
2. Type `oobe\bypassnro` and hit `ENTER`
3. The computer will reboot and allow the option for `I don't have internet`

### Option 2 (ms-cxh)

1. Launch cmd like above
2. Type `start ms-cxh:localonly` and hit `ENTER`