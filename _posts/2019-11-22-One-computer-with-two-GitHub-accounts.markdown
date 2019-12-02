---
layout: post
title:  "如何在一台电脑上配置两个 GitHub 账号"
date:   2019-11-22
---

## 第一步：给不同账号生成对应的密钥

```
cd ~/.ssh
ssh-keygen -t rsa -C "私人账号邮箱"
# 提示命名的时候可以以 id_rsa_personal 命名
ssh-keygen -t rsa -C "工作账号邮箱"
# 提示命名的时候可以以 id_rsa_work 命名
```

备注：提示输入密码的时候可以直接回车

## 第二步：将公钥添加到对应的 GitHub 账户

公钥即 .pub 文件用文本编辑器打开后的所有内容，全选复制就行了。

## 第三步：在 .ssh 目录下生成配置文件，内容如下：

```
Host personal
HostName github.com
User git
IdentityFile ~/.ssh/id_rsa_personal

Host work
HostName github.com
User git
IdentityFile ~/.ssh/id_rsa_work

# User 全部设置为 git，一开始我分别设置为 GitHub 上对应的 username，一直无法成功。

```
文件名为 config，没有的话就 `touch config` 新建一个。

## 第四步：更新 Identities

1. 清除缓存
	
	`ssh-add -D`
2. 添加 keys
	
	```
	ssh-add ~/.ssh/id_rsa_personal
	
	ssh-add ~/.ssh/id_rsa_work
	```
3. 查看 keys

	`ssh-add -l`
4. 测试 github 是否认可这些 key
	
	```
	
	$ ssh -T personal
	
	Hi XXX! You've successfully authenticated, but GitHub does not provide shell access.
	
	$ ssh -T work
	
	Hi XXX! You've successfully authenticated, but GitHub does not provide shell
	```

## 第五步：clone 远程项目

当再次 clone 一个新 Repos 时，如果其 ssh 地址为

`git@github.com:username/xxx.git`

使用 `git@work（或者 personal）:username/xxx.git` 即可。

## 第六步：进入项目目录，设置 email 和 username

1. 如果不确定是否已经配置过，可以使用下面的命令查看

	```
	git config --global user.name
	git config --global user.email
	```
2. 如果之前已经使用该命令进行配置，则先使用如下命令清除

	```
	git config --global --unset user.name
	git config --global --unset user.email
	```
	
3. 设置项目对应的 email 和 username

	```
	git config  user.name "xxx" // 配置全局用户名，如Github上注册的用户名
	
	git config  user.email "yyy@mail.com" // 配置全局邮箱，如Github上配置的邮箱	
	```

4. 设置项目的远程仓库

	```
	$ git remote rm origin
	# 远程仓库地址，注意Host名称
	$ git remote add origin git@personal:s32n/learingit.git
	$ git remote -v # 查看远程
	
	```

然后，就可以顺利地进行 git add commit push 等操作。

