## 概述

这是一个用来放`个人学习`、`个人记录`等内容的仓库

## 目录结构

项目结构 & 主要的内容，主要是 `content`、`themes/DoIt/assets/images` 目录：
```
$ tree -d -L 1
.
|-- archetypes
|-- content        // 主要存放文章的地方
|-- data
|-- layouts
|-- resources
|-- static
|-- template
`-- themes        // 博客主题，图片的保存位置

```

- **content**

主要内容，保存了博客的所有 md 文件

目录结构：
```
$ tree content/ -d
content/
`-- posts
    |-- ai
    |-- algorithm
    |-- codestyle
    |-- container
    |   |-- docker
    |   `-- kubernetes
    |-- go
    |   |-- gin
    |   `-- gorm
    |-- linux
    |-- network
    |-- os
    `-- tools
```

这是目前有的一些分类：ai、算法、编码风格、容器、go 等等，通过目录将文章分类管理，方便在 obsidian（目前正在使用的文章编辑工具）或者其他软件上查看，

- **themes**

这个目录存放了博客的主题文件
```txt
$ tree themes/ -d -L 3
themes/
`-- DoIt
    |-- assets
    |   |-- images // 主要是这个目录
	...
```

主要是 `themes/DoIt/assets/images` 这个目录，保管了文章的静态图片资源

## 技术栈、工具栈

博客搭建时间：2023.05.09
- 静态页面生成：[Hugo](https://gohugo.io/)（v0.91.2）
- 主题：[DoIt](https://hugodoit.pages.dev/zh-cn/)
- 文档编写：obsidian

## 写作流

以`编写一篇文档为例`：
1. 使用 obsidian 编写文档
2. 执行命令行本地生成 hugo 页面，看看有没有报错、文档编排之类的问题

```sh
hugo server -e production --disableFastRender
```

3. 如果没有问题，需要把相应内容推送到两个仓库：保存md文档的[仓库A](https://github.com/904566722/study-blog)（本仓库）、保存hugo生成的静态页面的[仓库B](https://github.com/904566722/904566722.github.io)

	可以先推送到仓库A，保存文章
	然后执行 hugo 生成静态网页，切到 public 目录，推送内容到仓库B

```sh
git add .
git commit -m "m/w. desc"
git push

hugo
cd public
git add .
git commit -m "m/w. desc"
git push
```

