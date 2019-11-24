---
layout: post
title:  "Jekyll + Github Pages 建站笔记"
date:   2017-11-18
---
早就听说了 Jekyll + Github Pages 建站的种种好处，今天周末，花时间鼓捣了一下。

现在把简单的步骤记下来，以免以后忘掉。

## 5 步建好一个最简单的 Github Pages 站点

[Github Pages 首页](https://pages.github.com) 列出了这 5 个步骤，非常简单，两三分钟就可以完成：

1. 新建一个 Github 仓库，名称为 username.github.io(username 为你的 Github 用户名)；
2. 将此仓库 clone 到本地：
	
	```
	git clone https://github.com/username/username.github.io
	```
	
3. 在本地仓库根目录新建一个 index.html 文件，并简单编辑一下：
	
	```
	cd username.github.io
	echo "Hello World" > index.html
	```
4. 推送至远程仓库：

	```
	git add --all
	git commit -m "Initial commit"
	git push -u origin master
	```
5. 访问 `https://username.github.io`，即可看到建好的 Github Pages 页面。

至此，一个最简单的 Github Pages 站点就剪好了（如果想用自己的域名代替，可以在仓库的 setting 页面设置，稍后会详细介绍）。

## Github Pages 主题选择

在对应仓库的 setting 页面即可选择自己想要的主题，这个很简单，不详细记录了。

## 在本地对网站 theme 及目录结构做个性化设置

首先需要确保安装了各种依赖包，具体可参考 [Jekyll 官方教程](https://jekyllrb.com/docs/installation/)

1. 将远程仓库克隆到本地后，还需要再安装一次 theme,具体方法是打开根目录下的 Gemfile 文件（如果没有，就自己新建一个），然后将这两行写在文件中(theme-name 为主题的实际名称)：
	
	```
	gem "theme-name"
	source 'https://rubygems.org'
	```
2. 然后在当前目录运行命令（请确保提前安装了 bundle）：

	```
	bundle install
	```
	
3. 修改根目录下的配置文件`_config.yml`中启用该主题：

	```
	theme: theme-name
	```
	
4. 在根目录路径下运行终端，执行以下命令：

	```
	jekyll build //如果报错的话，试试 bundle exec jekyll build
	jekyll serve  //同上
	```
5. 在浏览器中打开 http://127.0.0.1:4000,即可查看本地安装后的效果。

6. 然后，就可以根据自己的需要对主题做个性化定制了，可以先到主题的 Github 页面把文件都下载下来，方便在本地修改。比如我选用的 jekyll-theme-hacker 主题，在 Github 的仓库地址为：[pages-themes/hacker](https://github.com/pages-themes/hacker)

7. 然后就是具体的个性化配置，如何配置模板，可以参考[这里的教程](http://jmcglone.com/guides/github-pages/)。需要注意的是，这个教程是原理参考，具体怎么修改 default.html 等等文件的代码，需要你根据自己的需求来修改，所以需要懂一些 html 知识。

## 给网站添加 favicon 图标

1. 参考 medium 上的这篇文章就可以了[How to add a Favicon to GitHub pages](https://medium.com/@LazaroIbanez/how-to-add-a-favicon-to-github-pages-403935604460)，里面介绍得很详细。需要注意的是，favicon 用 png 格式，不要用 ico 格式。

2. 上面这篇文章里提到的工具网站 https://realfavicongenerator.net 非常好用，不光会给你生成浏览器的图标，还会生成 iPhone App 的图标等等。


## 添加 Disqus 插件

1. 在根目录 _includes 文件夹下新建 disqus.html 文件，复制[这里的代码](https://github.com/jmcglone/jmcglone.github.io/blob/master/_includes/disqus.html)（shortname 根据自己的实际情况修改，需提前注册一个 disqus）

2. 在日志布局文件 post.html 里面添加如下代码[参考链接](https://github.com/jmcglone/jmcglone.github.io/blob/master/_layouts/post.html)：
	
	```
	<!-- Post comments -->
	<div class="notecomments">
		{% include disqus.html %}	 
	</div>
	```

3. 在 _config.yml 中启用评论：
	
	```
	comments: true
	```
	
## 绑定个性域名并启用 https:

绑定比较简单，在仓库的 setting 页面绑定域名然后设置 DNS 记录就可以了，重点说一下启用 https：

1. 注册 [Cloudflare](https://www.cloudflare.com)，然后 Add Websites，系统会自动扫描 DNS 记录，设置好对应的记录就可以了；

2. 在域名注册商那里修改最新的 Nameserver;

3. 在 Cloudflare 的 Crypto 菜单中，设置 SSL 为 full 状态；

4. 在 Cloudflare 的 Page Rules 菜单中设置
	 `http://yourdomain/*`  为 **Always Use Https** 状态, 最后保存设置。
