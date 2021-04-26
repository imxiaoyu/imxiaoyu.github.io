---
title: hexo+github博客多电脑协同
date: 2021-04-17 14:15:30
tags:
  - 博客
---
# 一、简介

hexo是当前最火的静态博客框架，支持Markdown格式文章编辑并自动生成对应的静态网页，简单高效令人爱不释手。
  
<!-- more -->
使用hexo写博客的流程通常是在电脑本地文件夹里cmd窗口：

	命令	                  作用

	hexo new text	在source/_post目录下生成一个待写的text.md文件

	hexo g	        编译生成对应的HTML文件

	hexo s	        本地预览

	hexo d	        发布到远程仓库的master分支
    

# 二、困扰
如果你换了一台电脑或者说在自己的电脑和工作时的电脑上都想写博客，那应该怎么解决同步问题，我看了网上的一些方法，大部分都是通过github上git分支来巧妙地实现用同一个仓库保存静态网页和博客源码。

# 三、解决办法
## 1.新建git分支
Github Page要求使用master分支作为发布网站的源代码，我们只能用master分支来保存hexo生成的静态网页，博客源码，可以新建一个hexo分支来存储。在github上打开Pages对应的仓库，也就是以"username.github.io"命名的仓库，然后建立一个hexo分支。

因为我已经创建过source分支，故下方会显示目前该仓库上有master和source两个分支。其中source分支显示打钩，表示当前仓库的默认分支已经是source而不是master了，下面会讲。

## 2.更改仓库的默认分支
github上的仓库初始都会有个master分支，也就是默认分支。对于一个仓库project_name，当我们通过
	git clone https://github.com/imxiaoyu/imxiaoyu.github.io.git
下载代码时，实际拉取的是默认分支master对应的代码。而我们用hexo写博客时，通常是与md源文件打交道，对于deploy生成的master分支代码并不需要我们关注，因此可将仓库的默认分支改为保存源码的hexo分支，这样通过git clone拉取的就是hexo分支代码了。

在仓库的主页面，通过Settings -> Branchs，可以看到Default branch的Tab，显示的默认分支是master，可以勾选hexo，然后update即可将默认分支设置为hexo。


将本地hexo目录与远程仓库关联
进入到本地hexo工程目录，也就是我们通常执行hexo new post等命令的目录，执行如下操作：
	
	git remote add origin https://github.com/imxiaoyu/imxiaoyu.github.io.git

## 3.推送博客源码
将本地的md源文件、站点配置文件等推送到source分支。
因为我们只需要保留博客源码，其他无关的文件并不希望推送，需要确保配好了.gitignore文件，通常如下：

	.DS_Store
	Thumbs.db
	db.json
	*.log
	node_modules/
	public/
	.deploy*/

然后依次执行如下命令：

	git add .
	git commit -m '博客资源文件'
	git push origin hexo

## 4.删除public等文件（可选）
因为source分支是从master分支新建的，初始代码实际就是master的拷贝，因而master中已有的public等deploy生成的文件也会一起带过来，这些都不算是博客源文件，如果你也觉着source分支还存着这些有些别扭，就可以先在本地把它删掉，然后执行：

	git add .
	git commit -m 'DEL: public things which only for deploy'
	git push origin source

后续即便你再发布博客时，deploy生成public文件，在提交博客源码时，也不会将其带上去，因为有.gitignore将其忽略了。

新电脑
假设我们换电脑了，要在新环境继续在原有仓库基础上撸文章，此时通过git clone将博客源码拉到本地，然后安装、初始化hexo就能搞定：

	git clone https://github.com/imxiaoyu/imxiaoyu.github.io.git
	cd imxiaoyu/imxiaoyu.github.io
	npm install hexo
	npm install hexo-deployer-git -save

 hexo环境配置好后，继续像之前一样

	命令	                  作用

	hexo new text	在source/_post目录下生成一个待写的text.md文件

	hexo g	        编译生成对应的HTML文件

	hexo s	        本地预览

	hexo d	        发布到远程仓库的master分支

# 四、注意事项
确保hexo deploy推送的是master分支，hexo目录下的_config.yml文件通常会配置deploy推送的目标地址，这个一般在最初使用hexo时，就会配置为master，不用改动：

	# Deployment
	## Docs: https://hexo.io/docs/deployment.html
	deploy:
	  type: git
	  repo: https://github.com/imxiaoyu/imxiaoyu.github.io.git
	  branch: master
