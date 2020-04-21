---
title: [Hexo]基于GitHub搭建个人博客
date: 2020-01-06
categories:
- 环境搭建
- 博客搭建
tags:
- Hexo
- Git
- GitHub
- 博客
---

[Hexo](https://hexo.io/zh-cn/)是一个快速、简洁且高效的博客框架，支持MarkDown。本篇博客将会教会搭建博客并将其部署在GitHub上。

<!--more-->

# 前置条件

- **GitHub账号**
- **Git**
- **Node.js**



# 本地搭建

## 安装Hexo

打开**Git Bash**执行以下命令安装Hexo：

```bash
npm install -g hexo-cli
```

## 初始化Hexo

创建一个放置博客的目录（示例是在E盘中创建了一个Blog的文件夹  `/e/Blog`）。

![Blog目录](https://blog-1258865037.cos.ap-chengdu.myqcloud.com/Hexo——基于GitHub搭建个人博客/20200106145232.png)

使用**Git Bash**进入该目录

```bash
cd /e/Blog
```

使用**Git Bash**初始化

```bash
hexo init
```

![Blog目录](https://blog-1258865037.cos.ap-chengdu.myqcloud.com/Hexo——基于GitHub搭建个人博客/20200106150650.png)

## 启动Hexo

在启动之前，我们需要查看我们写的博客的位置。`Blog/source/_posts`中存放的便是我们写的博客文件。里面已经存在了一个内置的`hello-world.md`文件。

![存放博客文件目录](https://blog-1258865037.cos.ap-chengdu.myqcloud.com/Hexo——基于GitHub搭建个人博客/20200106151205.png)

在**Git Bash**中启动博客

```bash
hexo s
```

**注：`hexo s`是`hexo server`缩写。**

浏览器访问地址： http://localhost:4000/

![博客示例](https://blog-1258865037.cos.ap-chengdu.myqcloud.com/Hexo——基于GitHub搭建个人博客/20200106151651.png)

## 修改内容

这里的修改内容指的是修改博客标题、描述、作者等。

Hexo根目录称为站点目录，打开站点目录下的`_config.yml`

```yml
# Site
title: Hexo #标题
subtitle: '' #小标题
description: '' #描述
keywords:  #没有使用过 不清楚
author: John Doe #作者
language: en   #语言 
timezone: '' #时区
```

**注：`\themes\landscape\languages` 找到支持的语言。**

# 远端部署

目前的博客是搭载本地的，我们需要将博客部署在**GitHub**上。

## 创建仓库

仓库名必须以固定格式`<你的名字>.github.io`

![创建仓库](https://blog-1258865037.cos.ap-chengdu.myqcloud.com/Hexo——基于GitHub搭建个人博客/20200106152703.png)

## 安装部署插件

将本地的博客推送到GitHub上，需要安装一个博客部署插件。
在**Git Bash**中输入以下命令：

```bash
npm install hexo-deployer-git --save
```

## 部署设置

打开站点目录下的`_config.yml`

在最后添加：

```yml
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git 
  # 这里是刚才创建的远程仓库
  repo: https://github.com/linrealp/linrealp.github.io.git
  branch: master
```

**注意：冒号后面有一个空格**

## 推送

生成静态文件

```bash
hexo g
```

**注： `hexo g`是`hexo generate`的缩写**

部署到远程站点：

```bash
hexo d
```

**注： `hexo d`是`hexo deployer`的缩写**

等待1分钟左右，访问地址：`<你的名字>.github.io`

例如：`linrealp.github.io`



**至此，博客搭建以及部署已经完成。但是并不太美观，故我们需要下载自己喜欢的Hexo主题，来美化我们的博客。不同主题设置会有一些不同，我个人使用的是[melody](https://molunerfinn.com/hexo-theme-melody-doc/zh-Hans/)，文档有中文且详细。**

