---
title: "Hugo+Github搭建Blog"
date: 2019-06-23T11:12:40+08:00
categories:
- 工具
tags:
- 工具
---



### 安装Hugo

```
brew install Hugo
```

###  新建Hugo项目

```
hugo new site blog
cd blog
```

### 选择主题

```
git clone https://github.com/xianmin/hugo-theme-jane.git --depth=1 themes/jane
cp -r themes/jane/exampleSite/content ./
cp themes/jane/exampleSite/config.toml ./
```

### 添加新文章

```
hugo new posts/first-post.md
```

### 本地测试

```
hugo server
```

打开 [http://localhost:1313](http://localhost:1313/)查看效果

### 部署到github

### 新建两个repo

website-hugo 和 （axueya）.github.io，括号内改为自己github用户名

### 生成/public文件夹

```
hugo
```

### 将public文件夹与axueya.github.io链接到一起



```
cd public
git init
git remote add origin https://github.com/axueya/axueya.github.io.git
git add .
git commit -m "Init"
git push -u origin master
```

### 将整个目录与website-hugo链接

```
cd ../
git init
git remote add origin https://github.com/axueya/website-hugo.git
git add .
git commit -m "Init"
git push -u origin master
```

输入网址

 https://axueya.github.io/ 

