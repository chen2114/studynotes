<h1 align="center">study-notes</h1>
<p align="center">
  <a href= "https://github.com/zhhlwd/vuepress-theme-indigo-material/blob/master/LICENSE">
   <img src="https://img.shields.io/npm/l/vuepress-theme-indigo-material.svg">
  </a>  
  <a href= "https://www.npmjs.com/package/vuepress-theme-indigo-material">
   <img src="https://img.shields.io/npm/v/vuepress-theme-indigo-material.svg">
  </a>
  <a href= "https://www.npmjs.com/package/vuepress-theme-indigo-material">
   <img src="https://img.shields.io/npm/dt/vuepress-theme-indigo-material.svg">
  </a> 
  <a href= "https://github.com/zhhlwd/vuepress-theme-indigo-material/commits/master">
   <img src="https://img.shields.io/github/last-commit/zhhlwd/vuepress-theme-indigo-material.svg">
  </a> 
</p>


## 介绍
study notes是我记录在前端开发中所遇到的问题、学习的笔记等。

具体感受，国内请点击码云的 [博客网站（国内 gitee）](https://chen_project.gitee.io/studynotes)， 国外请点击 [博客网站（国外 github）]( https://chen2114.github.io/studynotes)

### 使用开发分支


这是 develop 的目录说明

**一定要注意!** develop docs 目录下的文件夹除了 posts 和 about 可以动,其他都不能动, 原因是 vuepress 在编译时会注册这些文件夹和文件为路由, 如果您把它们删了,即使您后面用 addRouter 再注册成功也不行,编译时没有注册的当跳转到时会让 vue 从根实例重新渲染, 这会导致应用很多状态没了 ,例如折叠的侧边栏重新打开,文章列表每打开一次就重新渲染一次,

使用本主题在内容上,您只要把新文章放在 posts 目录下, 文件顶部写好相关信息, 和自由定义 about 目录下的 index.md 文件就行了

```sh
|-- develop
    |-- .babelrc                   # 主题的babel配置, 按需加载element ui所需
    |-- .gitignore                 # 让git忽略跟踪dist文件夹等等, 不要把docs文件夹加进去
    |-- deploy.sh                  # 部署到git 远程仓库的shell文件, 要部署时双击即可, 前提是配置的构建目录位置没变
    |-- init.sh                    # (只要执行一次)克隆develop分支到本地后, 双击它, 一步完成所有操作, 等他完成下载, 开启测试服务器, 打开http://localhost:8080/看到效果
    |-- package-lock.json
    |-- package.json
    |-- 目录说明.md
    |-- docs                       # 存放所有开发环境的目录
        |-- index.md               # 首页,不用改
        |-- .vuepress
        |   |-- config.js          # 主题的配置
        |   |-- public             # 存放静态文件的目录, 例如img, ico ...
        |       |-- avatar.jpg
        |       |-- brand.jpg
        |       |-- favicon.ico
        |-- about                  # 展示在自我介绍页面的内容
        |   |-- index.md           # 不能删除, 可以添加内容
        |-- tags                   # 不能删除, 不能动
        |   |-- index.md           # 不能删除, 不能动
        |-- all                    # 不能删除, 不能动
        |   |-- index.md           # 不能删除, 不能动
        |-- posts                  # 存放所有文章的目录
            |-- assets.md          # 资源汇总,存放好用的插件及学习网站 
            |-- configuration.md   # vue-cli常用项配置
            |-- css.md             # 常用的css样式
            |-- deploy.md          # Node+PM2项目部署
            |-- echart.md          # echart图表使用
            |-- es6.md             # es6语法
            |-- git.md             # git使用
            |-- html.md            # html笔记
            |-- jQuery.md          # jq常用方法
            |-- less.md            # less学习笔记
            |-- markdown.md        # md学习笔记
            |-- node.md            # node学习笔记
            |-- scss.md            # scss学习笔记
            |-- vue.md             # vue难点记录
            |-- vueRouter.md       # 路由学习笔记
            |-- vuex.md            # vuex学习笔记
    |-- vuepress                   # 网站部署

```

> 两个 sh 文件如果要自定义,很有可能会报错"No such file or directory", 如果能运行下去则不用理睬,运行中断,可能因为 window 和 Linux 的换行不一样,可以运行命令解决:

```
> 用 vim 打开该 sh 文件，输入：
> :set ff
> 回车，显示 fileformat=dos，重新设置下文件格式：
> :set ff=unix
> 保存退出:
> :wq
> 再执行，
```

最后 init.sh 文件 **只运行一次** 即可, 以后使用的是 package.json 中提供的两个命令,

```sh
npm run dev
npm run build
```

分别是运行本地环境开启服务器预览和运行编译打包, 它和平时的 vue 项目流程一模一样

## 配置

请去看[vuepress 的文档](https://vuepress.vuejs.org/zh/config/#%E5%9F%BA%E6%9C%AC%E9%85%8D%E7%BD%AE)
**填写 docs/.vuepress/config.js**的相关字段.具体作用我都有相关注释, 没写的更多配置在[vuepress 文档](https://vuepress.vuejs.org/zh/config/)中查询

## 静态资源

您可以将它们放在 .vuepress/public 中, 具体请看[vuepress 文档](https://vuepress.vuejs.org/zh/guide/assets.html#%E7%9B%B8%E5%AF%B9%E8%B7%AF%E5%BE%84), 值得一提的是

> 如果您的网站会被部署到一个非根路径，您将需要在 .vuepress/config.js 中设置 base，举例来说，如果您打算将您的网站部署到 https://foo.github.io/bar/，那么 base 的值就应该被设置为 "/bar/" (应当总是以斜杠开始，并以斜杠结束)。

> 有了基础路径（Base URL），如果您希望引用一张放在 .vuepress/public 中的图片，您需要使用这样路径：/bar/image.png

## 开始写作
由于没有自动生成 md 文件的命令,需要手动创建 Markdown 文件,而且要放在 **./docs/posts/** 下, 然后还需要文件顶部像原主题那样写上信息

```
---
title: 【读书笔记】《JavaScript权威指南》第7章数组
date: 2018-11-08 04:10:03
tags: [读书笔记, 《JavaScript权威指南》]
categories: [读书笔记]
---
```

**上下的---符号不能少**, **字段后面空一格**再写值 不然会报错.

### markdown 文件的元信息

**title**: 文章名只读取这里写的名字,所以**一定要写**, 我的建议是最好文章名和文件名保持一致

**date**: 主题用了 vuepress 的内置插件[@vuepress/plugin-last-updated](https://vuepress.vuejs.org/zh/plugin/official/plugin-last-updated.html#%E9%80%89%E9%A1%B9), 也可以不用写了,

然而此插件依赖的是在 git 仓库中的提交时间,这就是为什么 init.sh 中要进行 git init 的原因

在本主题中一篇文章的创建时间的确定顺序是

**md 文件顶部的信息中的 date 字段==>git 仓库的提交时间===>都没有那就是当前时间**

**注意**的是这只是一种容错机制, 这不意味着不用管时间了, 最后条件的当前时间**并没有写进对应文件**,
如果每次编译都匹配的是最后一条条件会导致这个文章**永远排序在最开头**, 因为每次编译都是取的当前时间,

所以对于以前的文章, 开头信息没有 date 的, **要么手动加上**, 格式一定要是 YYYY-MM-DD HH:mm:ss, 才能计算出正确的时间顺序,**要么把文章迁移到
./docs/posts/ 目录下后, 运行**

```
git add -A
git commit -m '初始化文章时间'
```

这样退求其次的以当前时间重置那些没写 date 的文章

而对于使用本主题之后的文章**最好不用写 date 字段**了,以免出错, 而是每次新建文章后进行一次 commit, 让主题取 git 仓库的时间, 并且这种工作流能让您的提交历史对应对应文章的时间

```
git add -A
git commit -m '新建文章xxx'
```

**tags**: 字段**必须是数组**,如果没有则要写上一个空数组 _[]_,这样此文章会被分类到 _'未分类'_,我的建议是最好写上内容, 它是文章的内容标签, 是一种分类

**categories**字段已经放弃, 因为它和**tags**字段的作用重复

**摘要\*** :vuepress 内置了文章内容摘抄功能,这也是本主题首先判断收录的,如果没有才去从文章内容中截取一段,所以您可以完全自定义文章在首页列表的摘要了,例如一段简单明了的介绍,会让人更加想点击进去,这功能开启方式是 \<\!\-\- more \-\-\> 注释，该注释之前的内容会被抓取为文章的摘要

> 如果一个 markdown 文件中有一个 \<\!\-\- more \-\-\> 注释，则该注释之前的内容会被抓取并暴露在 \$page.excerpt 属性中。如果您在开发一个博客主题，您可以用这个属性来渲染一个带摘抄的文章列表。

## 部署

写完文章, 在本地预览无误后，执行以下命令：

``` sh
npm run build           # 打包项目，生成的文件在dist下，复制dist文件夹下的文件到vuepress文件夹下
git add .               # 暂存更改
git commit -m 'update'  # 提交到本地存储库
git push origin develop # 推送到远程develop分支
```