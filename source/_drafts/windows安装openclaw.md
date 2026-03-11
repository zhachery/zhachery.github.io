---
title: windows安装openclaw
categories: 学习记录
tags:
---

## 环境准备

在windows上启用WSL2，由于我是win11家庭版，所以没办法开虚拟机，参考方法：https://zhuanlan.zhihu.com/p/574675329

```cmd
pushd "%~dp0"
dir /b %SystemRoot%\servicing\Packages\*Hyper-V*.mum >hyper-v.txt
for /f %%i in ('findstr /i . hyper-v.txt 2^>nul') do dism /online /norestart /add-package:"%SystemRoot%\servicing\Packages\%%i"
del hyper-v.txt
Dism /online /enable-feature /featurename:Microsoft-Hyper-V-All /LimitAccess /ALL
```

下载并安装Ubuntu：https://www.daimagu.com/article/2502181533404040.html

## 安装openclaw

参考官网安装教程：
https://docs.openclaw.ai/zh-CN/start/getting-started
