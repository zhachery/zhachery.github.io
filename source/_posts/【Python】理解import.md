---
title: 【Python】理解import
date: 2024-04-29 17:48:57
tags: Python
---

在写Python项目时，经常会遇到类似于导包失败的情况，例如

```bash
ModuleNotFoundError: No module named "xxx"
```

每次都是潦草解决，现在有时间探究一下Python中的import导包。

## 概念
