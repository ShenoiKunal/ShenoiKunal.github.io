---
layout: post
title: Command Repo
date: 2025-04-02
parent: Cheatsheets
tags:
  - powershell
  - bash
  - cmd
  - command prompt
  - commands
author: Kunal Shenoi
description: Collection of commands I frequently use
---


# {{ page.title }}

*Published on {{ page.date | date: "%B %d, %Y" }}*

*Tags: {{ page.tags | join: ", " }}*

## **Run Commands**

---
### 1. Restart to Advanced Startup

- `shutdown /r /o /f /t 0`

### 2. Restart to UEFI BIOS

- `shutdown /r /fw /t 0`

---

## **CMD Commands**

---

### 1. Check IP Configuration

- `ipconfig`

### 2. Flush DNS Resolver Cache

- `ipconfig /flushdns`

### 3. Check Network Connections

- `netstat`

### 4. System File Checker

- `sfc /scannow`
    - **OffBoot/Off-Windows**
        - `sfc /scannow /offbootdir=C:\ /offwindir=C:\Windows`

### 5. Deployment Image Servicing and Management (DISM)

- `DISM /Online /Cleanup-Image /RestoreHealth`
    - **Off-Boot/Off-Windows**
        - `DISM /Image:C:\ /Cleanup-Image /RestoreHealth`

### 6. Trace Route

- `tracert example.com`

### 7. Display TCP/IP Configuration

- `netsh interface ip show config`

### 8. Display Active Network Connections

- `netstat -an`

---

## **PowerShell Commands**

---

### 1. List All Installed Programs

- `Get-ItemProperty HKLM:\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\* | Select-Object DisplayName, DisplayVersion, Publisher, InstallDate | Format-Table –AutoSize; Get-ItemProperty HKLM:\Software\Microsoft\Windows\CurrentVersion\Uninstall\* | Select-Object DisplayName, DisplayVersion, Publisher, InstallDate | Format-Table –AutoSize; Get-ItemProperty HKCU:\SoftwareMicrosoft\Windows\CurrentVersion\Uninstall\* | Select-Object DisplayName, DisplayVersion, Publisher, InstallDate | Format-Table –AutoSize`
- Better Version:
	- `Get-ItemProperty HKLM:\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\* | Select-Object DisplayName, DisplayVersion, Publisher, InstallDate | Format-Table –AutoSize; Get-ItemProperty HKLM:\Software\Microsoft\Windows\CurrentVersion\Uninstall\* | Select-Object DisplayName, DisplayVersion, Publisher, InstallDate | Format-Table –AutoSize; Get-ItemProperty HKCU:\Software\Microsoft\Windows\CurrentVersion\Uninstall\* | Select-Object DisplayName, DisplayVersion, Publisher, InstallDate | Format-Table –AutoSize`
#### List all Installed Programs on a non-active Windows install
- `reg load HKLM\InactiveWindows "D:\Windows\System32\config\software"
	- Replace `"D:\Windows\System32\config\software"` with the desired Windows install
- `Get-ItemProperty HKLM:\InactiveWindows\Microsoft\Windows\CurrentVersion\Uninstall\* | Select-Object DisplayName, DisplayVersion, Publisher, InstallDate | Format-Table –AutoSize`
- `reg unload HKLM\InactiveWindows`
`

### 2. Get System Information

- `Get-ComputerInfo`

### 3. Restart a Service (Example: Windows Update)

- `Restart-Service -Name wuauserv`

### 4. Check Disk Space

- `Get-PSDrive C | Select-Object Used, Free`

### 5. Check Windows Version

- `Get-ComputerInfo | Select-Object WindowsVersion, WindowsProductName`

### 5.5 Check Bios and Windows Version
- `Get-ComputerInfo | Select-Object WindowsVersion, WindowsProductName, BiosBIOSVersion, CsSystemFamily, CsSystemSKUNumber`

### 6. List Windows Services

- `Get-Service`

### 7. List Running Processes

- `Get-Process`

### 8. Check Disk Space

- `Get-PSDrive C | Select-Object Used, Free`

### 9. List Windows Features

- `Get-WindowsFeature`

### 10. Check Firewall Status

- `Get-NetFirewallProfile | Select-Object Name, Enabled`

### 11. Set SKU Policy to 0 (S-Mode Disable)

- `Set-ItemProperty -Path "HKLM:\SYSTEM\ControlSet001\Control\CI\Policy" -Name "SkuPolicyRequired" -Value 0`
	- Remember to #reboottobios and disable TPM and Secure Boot
		- Re-enable after restart

### 12. Battery Report

- `powercfg /batteryreport`
	- `/output` -- add destination path
### 13. [[Disable Windows Online Search]]
- `New-Item -Path HKCU:\Software\Policies\Microsoft\Windows -Name Explorer | Out-Null`
- `New-ItemProperty -Path "HKCU:\Software\Policies\Microsoft\Windows\Explorer" -Name "DisableSearchBoxSuggestions" -Value 1 -PropertyType DWord -Force`
### 14. Get Scheduled Tasks
- `Get-ScheduledTask | Select-Object TaskPath, TaskName, Description, State, Triggers | Export-Csv -path J:\myScheduledTasks.csv`
	- Remove `| Export-Csv -path J:\myScheduledTasks.csv` if you don't want an output file or swap the path for the correct directory
	  ```powershell
		$ScheduledTasks = Get-ScheduledTask
		
		foreach ($item in $ScheduledTasks) {
		
		    [string]$Name       = ($item.TaskName)
		    [string]$Action     = ($item.Actions | select -ExpandProperty Execute)
		    [datetime]$Start    = ($item.Triggers | select -ExpandProperty StartBoundary)
		    [string]$Repetition = ($item.Triggers.Repetition | select -ExpandProperty interval)
		    [string]$Duration   = ($item.triggers.Repetition | select -ExpandProperty duration)
		
		    $splat = @{
		
		    'Name'       = $Name
		    'Action'     = $Action
		    'Start'      = $start
		    'Repetition' = $Repetition
		    'Duration'   = $Duration
		
		    }
		
		    $obj = New-Object -TypeName PSObject -property $splat
		
		    $obj | Write-Output
		}
```

---

## **Bash Commands**

---

### Reboot to UEFI Setup
`systemctl reboot --firmware-setup`

### Remove all empty directories with "behind" in their name
- To remove all empty directories with "behind" in their name, you can use the `find` command again but with different options suited for directories.
- `find . -type d -empty -name '*behind*' -exec rmdir {} +`
	- `find .`: This searches the current directory and its subdirectories.    
	- `-type d`: This tells `find` to look only for directories.    
	- `-empty`: This option restricts the action to empty directories.    
	- `-name '*behind*'`: This searches for directories with "behind" in the name. The asterisks are wildcards that represent any character sequence, including an empty one.    
	- `-exec rmdir {} +`: This executes the `rmdir` command on each directory found. `rmdir` is a safer option than `rm -r`, as it only removes empty directories. The `{}` is a placeholder that `find` replaces with the actual directory name, and the `+` sign at the end causes `find` to process multiple items in a single batch, which can be more efficient.
### Move files with "Behind" in their names into "Behind the Scenes/"
- `find . -type f -name '*Behind*' -print0 | xargs -0 mv -t "Behind the Scenes/"`
	- `find . -type f -name '*S09*' -print0 | xargs -0 mv -t "Season 9/"`
### Search for file
#### Search for a Specific File Name
`find /path/to/search -type f -name "filename"`
#### Search for All Files in a Directory
`find /path/to/search -type f`
#### Search for Files with a Certain Extension
`find /path/to/search -type f -name "*.txt"`
#### Case-Insensitive Search for Files
`find /path/to/search -type f -iname "*.txt"`
#### Search for Directories Instead of Files
`find /path/to/search -type d -name "dirname"`
#### Execute a Command on Each File Found
`find /path/to/search -type f -exec ls -l {} \;`
#### Search for Files with Certain Permissions
`find /path/to/search -type f -perm 644`
#### Search for Files Modified in the Last n Days
`find /path/to/search -type f -mtime -7`
#### Search for Files Accessed in the Last n Minutes
`find /path/to/search -type f -amin -30`
#### Search and Output Detailed Information
`find /path/to/search -type f -exec ls -lh {} \;`

#### Make Directory Structure
`mkdir -p /path/to/create `
- Can also use {} to specify multiple subdirectories in one directory 

#### Find and replace text in multiple files
`find . -type f | xargs grep -l 'registerUser' | xargs sed -i '' -e 's/registerUser/registerAdmin/g'`
- The first part of the command is find, which finds all files and excludes directories. 
- That result is then piped to grep, which lists all files that contain `registerUser`
- The results are then sent to sed, which replaces all occurrences of `registerUser` with `registerAdmin`.
