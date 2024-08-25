---
title: Gitpage Setup AIO ✔️
layout: home
---

🔵  介绍

	官方手册:  https://pages.github.com/ 


🔵 jeklly 主题: just the docs

	gitpage 是用jekyll 的构建的.
	我主要是写文档. 目录是必要功能. 
	用jekyll 现成的主题就可以实现   https://just-the-docs.com/
	但是这个主题 最简单是在创建gitpage 前设置.
	直接用这个主题模板 来创建gitpage 项目. 


🔵 用just-the-docs模板新建项目

	https://just-the-docs.github.io/just-the-docs/#getting-started
	点击 use the template 
	登录你的github. 创建项目

	项目名格式必须是: username.github.io
	homeless-xu.github.io
	( 不能是github.io 前面必须再加用户名)

	确保可以 https://homeless-xu.github.io/


	🔶 配置github page 
	静态网站是要编译的. github 现在支持自动帮你编译. 开启下面功能就行.
		go to Settings > Pages > Build and deployment > Source, and select GitHub Actions

	现在网站是正常显示了. 但是本地下载下来的... 还是得配置本地服务器. 不然不方便看效果.


🔵 自定义域名(可选)

	如果有域名 可以用你自己的.
	比如用 0214.icu 自动跳转到 https://homeless-xu.github.io/

	1. 域名验证:  
		登录域名网站 加个txt类型的域名记录. 成功后即可删除

	2: 域名解析: cname 

		任意cname 解析到 https://xxxxx.github.io/
		https://homeless-xu.github.io/
		xxxxx 改成你自己的github用户名


🔵 jekyll 本地安装
	( 只要是配置本地环境. 本地要可以预览网站. 方便写看网页效果. github自动的毕竟有延迟 10分钟内)
	git 先克隆git项目到本地.

	安装依赖(看报错决定)
		gem install rexml -v 3.3.5
		bundle install

	运行本地服务器
	bundle exec jekyll serve

	127.0.0.1:4000 就可以本地访问了.



