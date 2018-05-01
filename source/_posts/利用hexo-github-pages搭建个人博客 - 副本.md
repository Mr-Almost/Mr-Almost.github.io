---
title: 利用hexo+github pages搭建个人博客
date: 2018-04-17 7:02:22
tags:
---

## 目标： 用一个仓库做博客展示页，一个仓库存程序代码
### 搭建博客展示页仓库

1. 首先在桌面创建一个文件夹做展示页的仓库 （一个仓库就够了，不需要再创建多一个文件夹做存程序代码的仓库，因为用来做存代码的仓库可以包含在里面，接下来会说原因..）

<img src="https://raw.githubusercontent.com/Mr-Almost/blog-generator/master/%E5%88%A9%E7%94%A8hexo%2Bgithub%20pages%E5%BB%BA%E7%AB%8B%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/blog1.1.png" width="50%" height="50%" />


2. 把Mr-Almost.github.io的文件夹右键用gitbash打开

3. 登陆github，创建空仓库，命名与文件夹相同，即“github的用户名.github.io”


<!--more-->


![](https://raw.githubusercontent.com/Mr-Almost/blog-generator/master/%E5%88%A9%E7%94%A8hexo%2Bgithub%20pages%E5%BB%BA%E7%AB%8B%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/2.png)




4. 在gitbash上依次输入下面的代码。（由于我已经创好了仓库所以下面的仓库名是另外打的）注意：要确保gitbash上进入的位置是Mr-Almost.github.io的文件夹
![](https://raw.githubusercontent.com/Mr-Almost/blog-generator/master/%E5%88%A9%E7%94%A8hexo%2Bgithub%20pages%E5%BB%BA%E7%AB%8B%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/3.png)


5. 然后输入 
```
npm install -g hexo-cli (安装 Hexo)
hexo init
npm i
hexo init myBlog
cd myBlog
hexo new 开博大吉
```
> 我本来是想直接hexo init的..但是它弹出显示说文件不是空的不能init，好吧…那就再创一个空文件好了
6. 这时候文件夹里多出一个myBlog的文件夹，进入打开名为_config.yml的文件
修改最下面的deploy代码：
![](https://raw.githubusercontent.com/Mr-Almost/blog-generator/master/%E5%88%A9%E7%94%A8hexo%2Bgithub%20pages%E5%BB%BA%E7%AB%8B%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/4.png)
7. 在gitbash里输入：
```
npm install hexo-deployer-git –save  （安装 git 部署插件）
hexo deploy
```
8. 这时刷新github即可看到自己的博客

### 搭建博客程序代码仓库

打开github展示页的仓库研究了一下，发现展示页的仓库只是myBlog里的public文件夹，所以我把public的父目录myBlog改为存程序的仓库发现也是可以的~

1. 在github上再次创建空仓库，名字为blog-generator,然后右键下面myBlog文件夹用gitbash打开，依次输入github上的6行代码
2. 在gitbash依次输入下面代码即可完成。
```
git add .
git commit -m "xx"
git push
```
### 总结：
每次创建新博客可以在gitbash的myblog路径下输入： hexo new 新博客名字
然后输入 
```
hexo generate
hexo deploy
```
这时候展示页仓库就会更新
那存代码的仓库如何更新? 
输入:
```
git add .
git commit -m "xx"
git push
```

Git的指令和hexo的指令要熟悉
谷歌了一下发现存程序代码的方法还可以用分支的方法做，有空学习一下
