---
title: 【Python】理解import
date: 2024-04-29 17:48:57
tags: Python
---

在写Python项目时，经常会遇到类似于导包失败的情况，例如

```bash
ModuleNotFoundError: No module named "xxx"
```

每次都是潦草解决，现在有时间探究一下Python中的import导包。[官方文档](https://docs.python.org/zh-cn/3/reference/import.html)。

## 概念

- 模块（module）：Python代码的一种组织单位，可以理解为一个.py文件，每一个模块都有独立的命名空间，模块中可以包含若干个Python对象，可以被导入到Python中，例如`import`。
- 命名空间（namespace）：命名空间是存放变量的场所。命名空间有局部、全局和内置的，还有对象中的嵌套命名空间（在方法之内）。命名空间是一种避免命名冲突的方案，例如，函数`builtins.open`与`os.open()`可通过各自的命名空间来区分。
- 包（package）：也是一种模块，不同的是他可以包含子模块或递归地包含子包。从技术层面上讲，包包含了`__path__`属性的模块。
  - 平常接触的是常规包（regular package），可以理解为包含有一个__init__.py文件的目录。常规包第一次被导入时，会被编译为pyc文件，存于包所在目录下。
  - 后来[PEP 420](https://peps.python.org/pep-0420/)又引入了命名空间包，和常规包不同，它没有__init__.py，命名空间包通常只出现在大型项目中，它的目的是可以把目录分散的模块作为一个包，方便协同开发。
