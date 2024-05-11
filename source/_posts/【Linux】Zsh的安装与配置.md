---
title: 【Linux】Zsh的安装与配置
date: 2024-05-08 15:12:41
tags: Linux
categories: Linux
---

本文使用的系统是Ubuntu，默认的shell是bash，但是非常建议大家试一下zsh，用了就回不去了。

## 安装

首先查看一下系统中可用的shell：

```bash
cat /etc/shells
```

如果已经有了`/bin/zsh`，那么只需要修改默认的shell即可：

```bash
# 方法一
chsh -s /bin/zsh # default
# 方法二
sudo usermod —shell /bin/zsh <username>
# 方法三
修改/etc/passwd文件
```

如果没有也没有关系，zsh的安装非常简单：

```bash
sudo apt-get install zsh
```

安装好之后修改默认shell即可。修改完成之后重启终端，会看到如下信息：

```bash
This is the Z Shell configuration function for new users,
zsh-newuser-install.
You are seeing this message because you have no zsh startup files
(the files .zshenv, .zprofile, .zshrc, .zlogin in the directory
~).  This function can help you with a few settings that should
make your use of the shell easier.

You can:

(q)  Quit and do nothing.  The function will be run again next time.

(0)  Exit, creating the file ~/.zshrc containing just a comment.
     That will prevent this function being run again.

(1)  Continue to the main menu.

(2)  Populate your ~/.zshrc with the configuration recommended
     by the system administrator and exit (you will need to edit
     the file by hand, if so desired).

--- Type one of the keys in parentheses --- 
```

选择q，回车退出。

## 配置

原生的zsh还算一般，需要配置一些主题和插件来打造最适合自己的shell。本文选择[ohmyzsh](https://ohmyz.sh/)来管理和配置zsh：

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

然后可以挑选一些zsh的主题，本文使用powerlevel10k主题：

```bash
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```

修改用户的配置文件，更换了zsh之后，用户配置文件变成了`.zshrc`：

```bash
cd ~
vim .zshrc

# 找到ZSH_THEME=并修改
ZSH_THEME="powerlevel10k/powerlevel10k"
```

退出vim编辑器，输入以下命令使配置生效

```bash
source .zshrc
```

然后会进入到配置页面，根据页面提时选择自己的喜好即可。另外推荐两个插件：

1、 自动补全

这个插件会记住你以前运行过的命令，当你第二次输入相同命令时，只需要输入头几个字母，就会有完整命令的提示，按方向键的$\rightarrow$就可以采纳提示。

![自动补全](【Linux】Zsh的安装与配置/2024-05-08-15-43-06.png)

```bash
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```

2、语法校验

这个插件可以检验你输入的命令是否可以正常执行，可以执行的显示绿色，不可以执行的显示红色。

```bash
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```

下载好插件之后，需要修改配置文件来启用：

```bash
cd ~
vim .zshrc

# 修改如下内容：
plugins=(
    # other plugins
    zsh-autosuggestions
    zsh-syntax-highlighting
)
```

退出vim编辑器，输入以下命令使配置生效

```bash
source .zshrc
```
