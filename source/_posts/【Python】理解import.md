---
title: 【Python】理解import
date: 2024-04-29 17:48:57
tags: Python
---

在写Python项目时，经常会遇到类似于导包失败的情况，例如

```bash
ModuleNotFoundError: No module named "xxx"
```

这是Python的模块导入不正确导致的，这篇文章浅析一下Python的模块导入。

## 前置概念

- 模块（module）：Python代码的一种组织单位，可以理解为一个.py文件，每一个模块都有独立的命名空间，模块中可以包含若干个Python对象，可以被导入到Python中，例如`import`。<!-- more -->
- 命名空间（namespace）：命名空间是存放变量的场所。命名空间有局部、全局和内置的，还有对象中的嵌套命名空间（在方法之内）。命名空间是一种避免命名冲突的方案，例如，函数`builtins.open`与`os.open()`可通过各自的命名空间来区分。
- 包（package）：也是一种模块，不同的是他可以包含子模块或递归地包含子包。从技术层面上讲，包是包含了`__path__`属性的模块。
  - 平常接触的是常规包（regular package），可以理解为包含有一个__init__.py文件的目录。常规包第一次被导入时，会被编译为pyc文件，存于包所在目录下。
  - 后来[PEP 420](https://peps.python.org/pep-0420/)又引入了命名空间包，和常规包不同，它没有__init__.py，命名空间包通常只出现在大型项目中，它的目的是可以把目录分散的模块作为一个包，方便协同开发。例如有一个parent文件夹，在导入parent包或者其子包时，导入搜索可能找到多个parent目录，每一个目录中都没有__init__.py文件，Python将为顶级的parent包创建一个命名空间包。

## 导入系统

在Python中有多种模块导入的方法，例如使用`import`语句、内置的`__import__()`函数或者`importlib.import_module()`。

平时最常用的就是`import`语句，此时内置的`__import__()`函数会被调用，可以替换`__import__()`，但是不建议这么做，因为没理由换掉，它很好用，而且很多代码假定了所用的是默认实现。

下面就来讲讲，当我们使用在Python中导入模块时，都发生了什么。

### 导入缓存

导入搜索前首先会先检查`sys.modules`，其本质是一个字典，存储了所有曾经导入过的模块。如果在这个字典中有相应的key和value，则直接返回，如果value是None，那么会报错`ModuleNotFoundError`。`sys.modules`是可写的，如果手动设置了某一个value为None，那么导入相关模块的时候必定报错。如果出现类似的情况，可以使用`importlib.reload(module)`来重新导入指定的模块。

如果在`sys.modules`中找不到对应的key，则将发起调用 Python 的导入协议以查找和加载该模块。此协议由两个概念性模块构成，即**查找器**和**加载器**。查找器的任务是确定是否能使用其所知的策略找到该名称的模块。同时实现这两种接口的对象称为**导入器**。

查找器的搜索过程见下一节。

### 查找路径

在项目中如果要引用上一级目录下的模块，需要在脚本中添加：

```python
import sys
sys.path.append('..')
```

其中`sys.path`是模块的搜索路径，其初始化值为环境变量`PYTHONPATH`以及安装有关的默认路径。在命令行中通过`python scripy.py`来执行脚本文件时，会添加脚本目录。也可以在命令行中通过`python -m module`来添加模块对应的工作目录。这也解释了为什么可以导入当前模块目录的下一级目录的模块，但是却无法导入上一级目录下的模块。

## 参考

1. [官方文档](https://docs.python.org/zh-cn/3/reference/import.html)。
