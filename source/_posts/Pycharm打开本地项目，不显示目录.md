---
title: Pycharm打开本地项目，不显示目录
date: 2024-05-08 20:39:43
tags: Debug
categories: Debug
---

今天git远程拉了一个项目到本地，使用PyCharm打开时发现目录树是这样的：

![目录树](Pycharm打开本地项目，不显示目录/2024-05-08-20-41-08.png)

原因是Project Structure中没有设置root目录，不知道为什么PyCharm不识别当前目录为Root目录。解决方案是打开File-Settings-Project Structure，并选择当前目录，应用即可。

![setting](Pycharm打开本地项目，不显示目录/2024-05-08-20-41-45.png)
