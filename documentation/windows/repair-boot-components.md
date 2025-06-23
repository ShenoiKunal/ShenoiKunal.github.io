---
layout: post 
title: Repairing BIOS/UEFI Boot Components 
date: 2025-06-22 
parent: Windows 
tags:
  - windows
  - boot
  - repair
  - bios
  - uefi
  - bcd
  - bootmgr 
author: Kunal Shenoi 
description: Repair Windows boot components when system fails to boot or displays boot-related error messages
---

# {{ page.title }}

_Published on {{ page.date | date: "%B %d, %Y" }}_

_Tags: {{ page.tags | join: ", " }}_

## Introduction

This guide covers repairing Windows boot components when a computer is unable to boot, hangs at a blinking underscore during boot, or displays boot-related error messages.

## Issue Symptoms

The computer may experience one or more of the following symptoms:

- Unable to boot or hangs at blinking underscore
- Sits at spinning dots and never loads Windows
- Displays error messages such as:
    - Missing Operating System
    - BCD, BOOT.INI, NTLDR, BOOTMGR, Winload, and/or Hal.dll is missing, corrupt, or compressed
    - The Windows Boot Configuration Data file does not contain a valid OS entry
    - Error codes: 0xc000000e, 0xc000000f, 0xc0000001, 0xc0000098, 0xc0000225

## Cause

Windows Core Boot Components are missing, misconfigured, corrupted, or compromised.

## Directions:

First confirm whether the system is using BIOS or UEFI and MBR or GPT, then select the appropriate repair method below.

### Windows 7/8.1/10/11 (UEFI/GPT)

#### Repair the BCD

**Note:** The path in the command below assumes the OS is located at C:\Windows. Adjust the path as needed.

1. Boot into WinPE or into Windows RE.
2. Open Command Prompt.
3. Type `bcdboot c:\windows` and press "Enter".
4. Once BCDBoot completes, exit Command Prompt and reboot the system.

#### Force Repair the BCD

**Note:** Perform this if the above command fails to complete.

1. Boot into WinPE or into Windows RE.
2. Open Command Prompt.
3. Assign the System Partition to S:
4. Delete everything in S:\
    - Type `s:` and hit "Enter" to navigate to the mounted System partition.
    - Type `del *.*` and hit "Enter" to erase everything in S:.
5. Type `bcdboot c:\windows /s s: /f UEFI` and press "Enter". This will rebuild the Core Windows UEFI Boot Components.
6. Once BCDBoot completes, exit Command Prompt and reboot the system.

#### Manually Rebuilding the EFI System Partition

**Note:** Perform this if the System partition is completely missing.

1. Boot into WinPE or into Windows RE.
2. Open Command Prompt.
3. Type `diskpart` and press "Enter" to start a DiskPart session.
4. Execute the following DiskPart commands:

**Note:** The following commands assume the EFI System Partition is on Partition 2 of disk 0 and that it's already a GPT disk. It also assumes the OS is located at C:\Windows. Adjust these commands accordingly if needed

```
select disk 0
select partition 2
format quick fs=fat32 label=EFI
set id=C12A7328-F81F-11D2-BA4B-00A0C93EC93B
gpt attributes=0x8000000000000001
assign letter=S
exit
```

5. Type `bcdboot c:\windows /s s: /f UEFI` and press "Enter". This will rebuild the Core Windows UEFI Boot Components.
6. Exit Command Prompt.
7. Reboot the system.

### Windows 7/8.1/10 (BIOS/MBR)

#### BOOTMGR Compliant Boot Sector and Master Boot Record

**Note:** The drive letter "C:" in this command should be the drive letter assigned to the Active Partition. Change the drive letter as needed.

1. Boot into WinPE or into Windows RE.
2. Open Command Prompt.
3. Type `bootsect /nt60 C: /force /mbr` and press "Enter".
4. Once Bootsect.exe completes, exit Command Prompt and reboot the system.

#### Repair the BCD

1. Boot into WinPE or into Windows RE.
2. Open Command Prompt.
3. Type `bcdboot c:\windows` and press "Enter".
4. Once BCDBoot completes, exit Command Prompt and reboot the system.

#### Force Repair the BCD

**Note:** Perform this if the above command fails to complete.

1. Boot into WinPE or into Windows RE.
2. Open Command Prompt.
3. Type `bcdboot c:\windows /s d: /f BIOS` and press "Enter". This will rebuild the Core Windows Boot Components.

**Note:** The path in the command above assumes the OS is located at C:\Windows and that the Active Partition (e.g. System Reserved Partition) is assigned D:. Adjust the path for the system being serviced.

4. Once BCDBoot completes, exit Command Prompt and reboot the system.

### Windows Vista

#### BOOTMGR Compliant Boot Sector and Master Boot Record

**Note:** The drive letter "C:" in this command should be the drive letter assigned to the Active Partition. Change the drive letter as needed.

1. Boot into WinPE or into Windows RE.
2. Open Command Prompt.
3. Type `bootsect /nt60 C: /force /mbr` and press "Enter".
4. Once Bootsect.exe completes, exit Command Prompt and reboot the system.

#### Repair the BCD

1. Boot into WinPE or into Windows RE.
2. Open Command Prompt.
3. Type `bcdboot c:\windows` and press "Enter".
4. Once BCDBoot completes, exit Command Prompt and reboot the system.

### Windows XP

#### NTLDR Boot Sector and Master Boot Record

**Note:** The drive letter "C:" in this command should be the drive letter assigned to the Active Partition. Change the drive letter as needed.

1. Boot into WinPE or into Windows RE.
2. Open Command Prompt.
3. Type `bootsect /nt52 C: /force /mbr` and press "Enter".
4. Once Bootsect.exe completes, exit Command Prompt and reboot the system.

#### Rebuild a Boot.ini

1. Boot into XP Recovery Console.

**Note:** The Administrator password may be required to access the Recovery Console.

2. Type `bootcfg /rebuild` and press "Enter".
3. Follow any prompts provided by bootcfg.exe to rebuild the boot.ini.
4. Once Bootcfg.exe completes, exit Recovery Console and reboot the system.