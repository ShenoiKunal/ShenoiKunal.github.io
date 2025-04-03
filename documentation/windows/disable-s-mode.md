---
layout: post
title: Disable S-Mode
date: 2025-04-02
parent: Windows
tags:
  - windows
  - os
  - repair
  - s-mode
author: Kunal Shenoi
description: Disable S-Mode on Windows computers without signing into a Microsoft Account
---


# {{ page.title }}

*Published on {{ page.date | date: "%B %d, %Y" }}*

*Tags: {{ page.tags | join: ", " }}*

## Introduction

Disable S-Mode on Windows without signing into a Microsoft Account.

### Method 1 (Post Setup, Logged In)
1. Setup PC normally, including user account.
2. Navigate to `Settings  > Updates & Security > Recovery > Advanced Startup > Restart Now`. This will reboot PC into Advanced Startup options.
3. Navigate to `Troubleshoot > Advanced Options > Startup Settings > Restart`.
4. Once in Advanced Options, Select option `7` to **disable driver signature enforcement**.
5. Once rebooted, open registry editor.
6. Navigate to `HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\CI\Policy`
7. Change or create the DWORD named `SkuPolicyRequired` to a value of `0`.
8. Reboot the computer to BIOS/UEFI
9. Disable Secure Boot and TPM
10. Save the changes and exit BIOS/UEFI, and boot the computer to the operating system.
11. A TPM module warning will appear, with a prompt to enter a number sequence. Enter this number sequence as prompted.
12. When the login screen appears, the option to use a PIN to log into the user account will not be available. Enter the account password to log into the account.
13. Restart the PC, and boot back into the BIOS/UEFI.
14. Re-enable the TPM and Secure Boot.
15. Save the changes and exit BIOS/UEFI, then boot the computer to the operating system.
16. Reboot and S-mode will be disabled with TPM and Secure Boot on.

### Method 2 (Post Setup, RE)
1. Right-click on the Start menu, navigate to "Shut down or sign out", hold down the "Shift" key and select "Restart", keep the shift key held down until the PC reboots into the advanced startup menu, then release.
2. Navigate to "Troubleshoot", then "Advanced Options", then select "Command Prompt" to open a Command Prompt.
3. Type: `bcdedit.exe -set loadoptions DDISABLE_INTEGRITY_CHECKS` and press "Enter", this disables driver signature enforcement for the next boot. 
	1. NOTE: Two Ds at the beginning of `DDISABLE`
4. Type: `regedit` and press `ENTER` to open Registry Editor. 
5. Navigate to `HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\CI\Policy` and set the `SkuPolicyRequired` DWORD value to `0` if it is not already.
6. Close Registry Editor and Command Prompt using the X in the top right, this will take the PC back to the Advanced Startup Menu.
7. Navigate to "Troubleshoot", then "Advanced Options", then select "UEFI Firmware Settings" and click "Restart".
8. Disable Secure Boot and TPM
9. Reboot, login (if setup completed)
10. Reboot to BIOS/UEFI again, re-enable TPM and Secure Boot, save, and exit

### Method 3 (Before Setup, OOBE)

1. In OOBE, launch Task Manager using `CTRL`+`SHIFT`+`ESC`
2. Run a new task: `shutdown /r /o /f /t 0` to boot to Advanced Startup
3. Continue from step 2 of Method 2