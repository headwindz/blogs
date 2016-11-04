---
title: 手把手用hexo建一个美腻动人的Github博客
date: 2016-11-04
tags: hexo
---

## hexo

hexo是一款基于Node静态博客框架，我会告诉你我是冲着node才选择hexo的么~

### 一言不合先安装
``` bash
npm install -g hexo
```

### 初始化
``` bash
# 初始化目录
hexo init ${your_hexo_dir_name}

# 生成静态页面(public/子目录)。该目录下生成的文件就是之后发布到github上的静态文件
hexo generate

# 启动本地服务器。 可通过localhost:4000。 每次发布前可运行此命令查看博客效果
hexo server
```

### hexo帮助
``` bash
# 查看hexo相关命令
hexo help
```
<!-- more -->

## 给你的博客换换肤

网上有很多自定义的hexo主题。可自行选择你喜欢的。[请搓我](https://www.zhihu.com/question/24422335)

我用的是[hexo-theme-next](https://github.com/iissnan/hexo-theme-next)

```bash
# 进入生成的hexo文件夹下的theme目录
cd ${your_hexo_dir_name}/theme
# 拉下你的主题包
git clone https://github.com/iissnan/hexo-theme-next.git
```

[next主题配置搓这里](http://theme-next.iissnan.com/theme-settings.html)

## 修改默认配置

在你的hexo文件夹下有个_config.yml配置文件。更换皮肤，博客名，博客作者需配置此文件。下面是我的配置。请自行对应修改。

```bash
title: 一叶知秋
description: 前端路漫漫且行且珍惜
author: 不吃猫(🐱)的鱼(🐟)
language: default

# 别忘了更换皮肤！
theme: hexo-theme-next
```

## 开始写新文章

```bash
hexo new ${your_post_name}
# 以上命令会在hexo站点下的source/_posts目录生成一个$your_post_name.md的文件。
# 该md文件就是你的新文章内容.
# 编辑完毕后可用hexo server预览
```

## 部署到Github

博客弄好了之后下一步就是发布到网上供别人膜(吐)拜(槽)了。以下是操作步骤:

### [建立Github.io仓库](https://pages.github.com/)

### 配置_config.yml文件里的deploy相关内容。例子如下
文件末尾，配置如下内容。其中，reposity对应的url为上一步在Github上建立的reposity地址。
```bash
deploy:
  type: git
  repo: ${这里填上一步在Github.ip的仓库地址}
  message: website deploy
```
配置完后，执行如下命令：
``` bash
hexo deploy
```

通过访问 https://${your_github_username}.github.io (例如我的Github账户为mzjh, 所以我的博客地址 https://mzjh.github.io) 即可访问你的博客了

## 我踩过的那些坑

### 执行hexo deploy时出现: Deployer not found: git错误

```bash
npm install hexo-deployer-git --save
```
即可！

### hexo语言变成非中文
修改hexo主站的_config.yml文件对language进行修改
```bash
language: zh-Hans
```
即可~
