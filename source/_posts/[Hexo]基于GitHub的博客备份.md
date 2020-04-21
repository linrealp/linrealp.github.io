---
title: [Hexo]基于GitHub的博客备份
date: 2020-01-07
categories:
- 环境搭建
- 博客搭建
tags:
- GitHub
- Hexo
- Git
- 博客
---

Hexo会自动推送生成的静态文件到远程库，但是我们查看可以看到，远程库的内容与本地的内容不一样。

如果我们更换了电脑，那么本地的文件就不方便拿到，通过拷贝的方式，很不方便。在网上找到的一个很好的方法，就是通过Git的分支来管理。

<!--more-->



# 前置条件

- **Git**

- **Hexo**

- **GitHub**

  

# 具体实现

1. 克隆GitHub上的远程库`<你的名字>.github.io`项目的文件到本地	

   ```bash
   git clone https://github.com/<你的名字>/<你的名字>.github.io.git
   ```
![文件目录](https://blog-1258865037.cos.ap-chengdu.myqcloud.com/Hexo——基于GitHub的博客备份/20200106215812.png)

3. 删除文件夹内除了`.git`的其他所有文件（`.git`是一个隐藏文件）

4. 将本地Hexo目录下的文件全部复制过来

   ![新文件目录](https://blog-1258865037.cos.ap-chengdu.myqcloud.com/Hexo——基于GitHub的博客备份/20200106220408.png)

5. 删除主题文件中的`.git`文件，`.git`中嵌套`.git`会出现问题

6. 创建一个hexo分支，并切换到该分支上

   ```bash
   git checkout -b hexo
   ```

7. 提交复制过来的文件到暂存区

   ```bash
   git add --all
   ```

8. 提交

   ```bash
   git commit -m "新建hexo分支"
   ```

9. 推送

   ```bash
   git push --set-upstream origin hexo
   ```

   之后更新本地博客到远端，只需要在`hexo分支`上`git push`就行，hexo操作不变

10. 更换电脑，只需要将远程仓库`hexo分支`克隆到本地就行了，npm依赖项按照提示安装（再执行`hexo s`之后会有提示）

   ```bash
   git clone -b hexo https://github.com/<你的名字>/<你的名字>.github.io.git
   ```

   

