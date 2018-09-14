---
title: HEXO在不同的电脑上更新博客
date: 2016-08-31 17:22:59
tags:	#标签注释
	- hexo
	- 博客
	- 随笔
categories:
	- cc
---

### 描述

用hexo+github在家里的电脑上搭建完博客，资源文件都在这台电脑上，<!--more-->有没有一种方法可以随时在不同的电脑上更新内容呢，请往下看~~

### 原理
因为master主分支上存的是部署好的静态文件，我们更新内容到博客的时候其实只需要本地有一套源文件即可，解决方法就是创建一个hexo分支出来存放源文件，然后用不同的电脑操作源文件即可，下面是图：

![“原理图”](/images/he/hexobranch.png)

### 具体操作步骤

1、先配置hexo环境，安装Node.js，然后安装hexo及其模块
```
npm install -g hexo
npm install
npm install hexo-deployer-git --save
```

2、克隆gitHub上的XXX.github.io项目的文件到本地（需要先配置SSH key，方法自行网上查找）
```
git clone https://github.com/用户名/xxx.github.io.git
```
3、删除文件夹里除了.git文件夹的其他所有文件

4、把源文件hexo文件夹下的所有文件复制过来并把themes文件夹下自定义主题（比如yilia）下的.git文件夹删除，有可能这个.git是隐藏的，找出来删掉，因为自定义主题是从别的github上拉取下来的，一个git仓库不能包含另一个仓库，不然会提交不成功

5、hexo下有个叫.gitignore的文件，如果没有就输入 touch .gitignore，创建一个

6、 .gitignore文件里应该是这些内容 
```
.DS_Store 
Thumbs.db 
db.json 
*.log 
node_modules/ 
public/ 
.deploy*/ 
```

7、创建一个叫hexo的分支并切换到这个分支上 
```
git checkout -b hexo 
```
8、 提交复制过来的文件到暂存区 
```
git add --all 
```
9、 提交 
```
git commit -m "新建分支保存源文件" 
```
10、推送分支到github 
```
git push --set-upstream origin hexo 
```
11、以后如果在其他电脑上面用hexo写博客，就直接把创建的分支克隆下来，npm install安装依赖之后就可以用了，克隆分支的操作（-b后面跟的是分支名称，我这里分支名称是hexo，也可以通过设置hexo为默认分支的方法来去掉-b这个命令）
```
git clone -b hexo https://github.com/用户名/xxx.github.io.git 
```
11、到此已经完成，实际上hexo d -g部署文件和git add/commit/push 推送更新到分支是两个不同的过程，主分支保存静态文件，hexo分支则保存源文件，只要把源文件取到安装一下hexo环境就可以写博客了~~