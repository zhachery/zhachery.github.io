---
title: 【Linux】Samba文件共享
date: 2024-05-08 15:51:14
tags: Linux
categories: 技术随笔
---

本文系统：Ubuntu18.04

## 安装

首先安装samba

```bash
sudo apt install samba
```

安装好之后默认的配置文件位于/etc/samba/smb.conf中。系统单元（Unit）位于/etc/systemd/system中，可执行文件是/usr/sbin/smbd。

## 配置

打开/etc/samba/smb.conf文件，在末尾写入用户配置

```conf
[zhuka]
  comment = zhuka's home
  path = /home/zhuka
  read only = no
  browsable = yes
  writable = yes
  available = yes
  valid users = zhuka
  # config recycle
  vfs object = recycle
  recycle:repository = /home/zhuka/recycle
  recycle:versinos = yes
  recycle:keeptree = yes
  recycle:maxsize = 0

```
