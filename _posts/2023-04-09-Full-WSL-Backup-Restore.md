---
layout: single
title: WSL Backup & Restore
date: 2023-04-09
classes: wide
header:
  teaser: /assets/images/slae32.png
categories:
  - WINDOWS
tags:
  - wsl
  - windows
  - ubuntu
---
A clean way to full backup & restore WSL.

Backup:

In order to perform a full backup of the image and the Disk VHDX file together, WSL V2 has the --export  command.

First list the images that you have installed and look for the name of the image that you want to backup.

```bat
wsl -l -v
```

Then we shutdown WSL to avoid any errors while accessing the files,
```bat
wsl  --shutdown
```

To make a full backup of the WSL image use the following syntax:
```bat
wsl --export <Image Name> <Export location file name.tar>
```
 
We can give a a look at the VHDX file size by navigating to where these are stored. 
Those can be found here:

%userprofile%AppDataLocalPackages

For Ubuntu 20.04 WSL2 image,  is listed in folder under CanonicalGroupLimited.

Under the Local State folder, you will find the VHDX file.

WSL Ubuntu 20.04 VHDX file and file size

![image](https://user-images.githubusercontent.com/78656150/230788972-b64514e8-8c8c-48bb-ae29-635ee69ac5c9.png)

As you can see above, the raw VHDX file is 2.3 GB or so, while the backup .tar file was around 1.7 GB. 

Therefore, it also compresses all the required files into a tarball.

RESTORE

Now that we have created the .tar file with all necessary we need to perform a clean installation to restore the backup.

WSL needs three main features to work, these are:

**Hyper-V (Default Switch, WSL adapter), Virtual Machine Platform (Manage App Packages), WSL (Kernel Virtualization)**
 
All those can be done automatically with the following commands

Open Powershell as administrator,

Enable the feature
```powershell
enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All
enable-WindowsOptionalFeature -Online -NoRestart -All -Verbose -FeatureName VirtualMachinePlatform
enable-WindowsOptionalFeature -Online -All -Verbose -FeatureName Microsoft-Windows-Subsystem-Linux
```
 
Reboot your machine

After the enablement of the subsystems, this will download and install WSL 2 kernel and install WSL 2 with Ubuntu 20.04, please replace this value with the required distribution

```powershell
(New-Object System.Net.WebClient).DownloadFile(“https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi”,"$env:APPDATA\wsl_update_x64.msi")
Start-Process "$env:APPDATA\wsl_update_x64.msi" '/quiet'
```

```cmd
CMD
wsl --set-default-version 2
wsl --install --distribution "Ubuntu-20.04"
```
 
With those steps performed now we are ready to perform a full import of the .tar file;

```cmd
wsl --import <Image Name> <Directory where you want to store the imported image> <Directory where the exported .tar file exists>
```

This will import all the necessary files to fully restore WSL V2,

You can run wsl  -l -v to confirm it
