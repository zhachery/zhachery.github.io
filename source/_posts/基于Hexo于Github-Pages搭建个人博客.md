---
title: 基于Hexo与Github Pages搭建个人博客
date: 2024-04-27 20:31:25
tags: Hexo
---

## 本地环境

以下自行安装，可以参照[Hexo官网](https://hexo.io/zh-cn/docs/)

- Git：用来推送代码，文章需要在本地编写，然后推送到git仓库
- Node.js：Hexo是基于Node.js的，所以需要安装
- Hexo：使用npm安装Hexo

## 开始搭建

1、首先初始化本地博客

```bash
mkdir <blog-folder>  # 为你的博客创建一个文件夹
cd <blod-folder>
hexo init
```
<!-- more -->
此时可以使用`hexo new post "hello"`命令来创建一个新的文章，这个文章会出现在`source/_posts`文件夹中，使用命令`hexo generate`可以生成静态资源文件，根目录下会出现`public/`文件夹。然后在命令行中输入如下命令本地测试：

```bash
hexo clean & hexo g & hexo s
```

2、将博客托管到Github Pages

hexo可以帮助我们将markdown文件转化为静态资源文件，而Github Pages则可以将静态资源文件托管。

创建一个github仓库，仓库名必须为`<用户名>.github.io`。进入仓库，在`Setting-Pages`设置`Build and deployment`项为`Github Actions`，让其执行流水线任务。

![alt text](基于Hexo于Github-Pages搭建个人博客/2024042701.png)

进入本地博客的根目录，在.github文件夹中创建workflows文件夹，创建文件`page.yml`设定流水线任务，文件内容如下：

```yml
name: Pages

on:
  push:
    branches:
      - main  # 这里的分支名需要确认一下，和远程仓库的分支名要一致

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          token: ${{ secrets.GITHUB_TOKEN }}
          # If your repository depends on submodule, please see: https://github.com/actions/checkout
          submodules: recursive
      - name: Use Node.js 20  # 这里的版本号最好和本地Node.js对应
        uses: actions/setup-node@v4
        with:
          # Examples: 20, 18.19, >=16.20.2, lts/Iron, lts/Hydrogen, *, latest, current, node
          # Ref: https://github.com/actions/setup-node#supported-version-syntax
          node-version: '20'
      - name: Cache NPM dependencies
        uses: actions/cache@v4
        with:
          path: node_modules
          key: ${{ runner.OS }}-npm-cache
          restore-keys: |
            ${{ runner.OS }}-npm-cache
      - name: Install Dependencies
        run: npm install
      - name: Build
        run: npm run build
      - name: Upload Pages artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./public
  deploy:
    needs: build
    permissions:
      pages: write
      id-token: write
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

然后打开git-bash，输入命令

```bash
git init
git remote add origin git@github.com:zhachery/zhachery.github.io.git  # 更换为你的仓库地址
git add .
git commit -m "hexo init"
git push -u origin master  # 注意分支要对应，如果不对应了可能会失败，根据命令行的提时来改正
```

成功推送之后，可以进入Action里看任务是否执行成功

![alt text](基于Hexo于Github-Pages搭建个人博客/2024042702.png)

如果执行成功了，应该就可以前往`https://<你的 GitHub 用户名>.github.io`查看网站了。

## 其他

### 插入图片不显示

很多网上的教程安装的是hexo-asset-image，但是这个插件太老了，而且有bug，尽管github仓库已经修改了[bug](https://github.com/xcodebuild/hexo-asset-image/issues/47)，但是我npm安装的仍然是旧版本，应该是很久没有维护了，所以不建议安装这个插件了。偶然间发现了另外一个插件：

```bash
npm install hexo-image-link --save
```

修改根目录下的_config.yml文件：

```yaml
post_asset_folder: true  # false改为true
```

这样当使用`hexo new post <title>`时会在_post目录下创建一个同名文件夹，将所要引用的图片保存在该文件夹下，在正文中直接引用时图片的路径为`<当前md文件名-无后缀>/<图片文件名-带后缀>`。

我使用VsCode作为markdown的编辑器，推荐一个VsCode的插件：Paste Image，然后点击设置。

![paste image](基于Hexo于Github-Pages搭建个人博客/2024-04-27-21-18-16.png)

找到如下设置项，并修改为`${currentFileDir}/${currentFileNameWithoutExt}`：

![修改配置](基于Hexo于Github-Pages搭建个人博客/2024-04-27-21-19-26.png)

使用快捷键`Ctrl+Alt+V`可以直接插入剪切板上的图片，并且图片会被自动保存到`<当前md文件名-无后缀>/<图片文件名-带后缀>`，非常方便。如此一来，本地markdown可以实时显示图片，博客静态网页也可以正确渲染图片。

### 主题

Hexo可以选择自己喜欢的主题，可以在官网下载主题，并更换。

- [NexT](https://github.com/next-theme/hexo-theme-next)
- [Geek](https://github.com/sanjinhub/hexo-theme-geek?tab=readme-ov-file)
