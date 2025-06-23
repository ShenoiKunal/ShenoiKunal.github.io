---
layout: post 
title: Windows Installer Cannot Find Disk on Intel 10th Gen and Up 
date: 2025-06-22 
parent: Windows 
tags:
  - windows
  - intel
  - drivers
  - vmd
  - install
  - disk
  - nvme
  - RST
  - Volume Management Device
  - NVMe
  - SSD
author: Kunal Shenoi 
description: Fix Windows installation media not detecting local disks on Intel 10th generation and newer systems
---

# {{ page.title }}

_Published on {{ page.date | date: "%B %d, %Y" }}_

_Tags: {{ page.tags | join: ", " }}_

## Introduction

When performing a clean Windows installation on systems with Intel 10th generation processors and newer, the Windows installation media may fail to detect the local hard disk during the installation process.

## Issue

On PCs running 10th generation Intel processors and newer, booting to Windows installation media to perform a clean install may result in the installation process not being able to see the local hard disk.

## Cause

Intel introduced a new disk management technology called Volume Management Device (VMD) in 10th generation and newer systems. VMD is used to manage NVMe SSDs and/or Optane memory caches, and requires specific drivers for the disk to be recognized during Windows installation.

## Directions:

### 10th/11th Generation Intel Systems

1. Download the Intel Rapid Storage Technology driver package:
    - Navigate to Intel's download site for 10th and 11th gen platforms
    - Download the `SetupRST.exe` package
    - Save the file in the Downloads folder
    - Ensure the file is named exactly `SetupRST.exe`
2. Open Command Prompt and navigate to Downloads:
    
    ```
    cd %USERPROFILE%\Downloads
    ```
    
3. Extract the drivers using this exact command:
    
    ```
    .\SetupRST.exe -extractdrivers SetupRST_extracted
    ```
        
4. Navigate to the extracted drivers:
    - Open File Explorer and go to the `SetupRST_extracted` folder
    - Navigate to `Production > Windows10-x64 > 15063 > Drivers`
5. Copy the following folders to a USB flash drive:
    - `AHCI`
    - `RAID`
    - `VMD`
6. During Windows installation when the local disk cannot be found:
    - Click "Load Driver"
    - Insert the USB drive with the drivers
    - Navigate to one of the three folders copied in step 5
    - Select the appropriate INF file:
        - `iaStorVD` (VMD folder)
        - `iaStorAC` (RAID folder)
        - `iaAHCIC` (AHCI folder)
    - Click "OK"
7. If the local disk appears after loading the first driver, continue with Windows installation. If not, repeat step 6 with the other driver folders.
    
8. The local disk should now be visible. If using an 11th generation CPU and these drivers don't work, follow the steps for 11th-13th generation systems below.
    

### 11th/12th/13th Generation Intel Systems

1. Download the newer Intel Rapid Storage Technology driver package:
    
    - Navigate to Intel's download site for 11th-13th gen platforms
    - Download the `SetupRST.exe` package
    - Save the file in the Downloads folder
    - Ensure the file is named exactly `SetupRST.exe`
2. Open Command Prompt and navigate to Downloads:
    
    ```
    cd %USERPROFILE%\Downloads
    ```
    
3. Extract the drivers using this exact command:
    
    ```
    .\SetupRST.exe -extractdrivers SetupRST_extracted
    ```
    
    **Note:** This command must be typed exactly as shown or it will fail.
    
4. Navigate to the extracted drivers:
    
    - Open File Explorer and go to the `SetupRST_extracted` folder
    - Navigate to `Production > Windows10-x64 > 15063 > Drivers`
5. Copy only the `VMD` folder to a USB flash drive.
    
6. During Windows installation when the local disk cannot be found:
    
    - Click "Load Driver"
    - Insert the USB drive with the drivers
    - Navigate to the `VMD` folder
    - Select the `iaStorVD.inf` file
    - Click "OK"
7. The local disk should now be visible and the Windows installation can proceed.
