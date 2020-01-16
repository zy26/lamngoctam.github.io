---
title: "How to setup Linux subsystem in Window 10"
date: 2019-01-20
categories:
  - softwares
tags:
  - linux
---

## There are some steps which can help you working on Linux system
- Changing some features in Window as the below images
![image](/assets/linux/LinuxSubsystem2.png)

-
![image](/assets/linux/LinuxSubsystem3.png)

- 
![image](/assets/linux/LinuxSubsystem4.png)

- Next step is downloading the Ubuntu app from Microsoft Store in Window. 

![image](/assets/linux/LinuxSubsystem5.png)

## Working on Ubuntu bash
- If we do `ls` command, there is nothing here.
- `cd /` then `ls` we can see the root of subsystem. __mnt__ is the C folder of Window
- `cd mnt` - `ls` - `cd C` now we are working on C folder of window
- From `/mnt/c/users/ngoct/Projects` we do `$ sudo apt update` to avoid data corruption if we work on the folder in linux subsystem inside window folder.
- The `$ sudo apt upgrade`
