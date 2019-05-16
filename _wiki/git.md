---
layout: wiki
title: Git
categories: Git
description: Git 使用与技巧
keywords: Git
---

Git 实在是太好用了。来写写遇到的配置问题和常用命令。。

### 基础配置知识：
用户目录的config file， 适用于这个用户。 --global写的是这个文件
```
~/.git/config
```
比如设置用户名和邮箱和unset

```
$ git config --global user.name "John Doe"
$ git config --global user.email johndoe@example.com
$ git config --global --unset user.name
$ git config --global --unset user.email
```

除此之外， 每一个git repo有自己的config file
```
/repo_path/.git/config
```

可以用这个命令看看配置信息
```
git config --list
```

### 如何在本机配置多个Github账号
Steps:

1. 新建ssh-key,记得如果已经有ssh key的话，记得重命名，否则会覆盖。
```
cd ~/.ssh
ssh-keygen -t rsa -C  "xx@email.com"
```
此时会看到两个新文件， 一个私钥 id_rsa2,一个公钥 id_rsa2.pub
2. 新ssh-key 添加至 ssh agent中
```
eval `ssh-agent -s`
ssh-add ~/.ssh/id_rsa2
```
3. 创建一个config file 在./ssh 目录下, 添加新的账号
```
#account 1
Host github.com
HostName github.com
User git
IdentityFile C:/Users/Administrator/.ssh/id_rsa
#account 2
Host github2
HostName github.com
User git
IdentityFile  C:\Users\Administrator\.ssh\id_rsa2
```
4. 添加 .pub 到github setting ssh 里面
5. 测试 ssh -T git@github.com
点击此处，查看[原文](https://blog.csdn.net/gdutxiaoxu/article/details/53573426 )
