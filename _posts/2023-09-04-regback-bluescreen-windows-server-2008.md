---
layout: single
title: Windows Server 2008 Crash BSOD
date: 2018-11-18 12:00:00
classes: wide
header:
  teaser: /assets/images/slae32.png
categories:
  - WINDOWS
tags:
  - windows
  - server2008
  - regback
---


Windows login screen the Windows Server 2008 R2 gives STOP Code 0X000000F4
![image](https://user-images.githubusercontent.com/78656150/230791432-144486c8-a2c0-43fe-bb4e-553ecbda77b2.png)

 
 ![image](https://user-images.githubusercontent.com/78656150/230791065-5b12f1eb-81da-477f-b6df-f66fead3103a.png)

The Server only successfully boots on Safe Mode with Networking,

Noticed that Crowdstrike installed an update recently,
 
![image](https://user-images.githubusercontent.com/78656150/230791151-58b1d996-acf9-45c4-9bb0-d56ab4faabcb.png)

When trying  uninstall got an error code message,
 
This update modified the registry and it does not allow the server to boot.

Question
Sign in to vote
0
Sign in to vote

If we cannot boot up and want to replace hive files, we need to boot from WinRE, and open CMD. 
```bat
cd C:\windows\system32\config
```
Backup the current system/software hive:
```bat
      Ren system system_backup
      Ren Software Software_backup 
```

3. Replace the system / software hive by:
```bat
     Copy C:\windows\system32\config\regback\software C:\windows\system32\config\software
     Copy C:\windows\system32\config\regback\system C:\windows\system32\config\system
```
Now reboot

By making a RegBack procedure (swapping system & software hives with the ones within RegBack folder), and by changing the default control set value to number 2 (Last Known Good).

 Any faulty configuration made by any software, is reverted and now the server boots as expected,
