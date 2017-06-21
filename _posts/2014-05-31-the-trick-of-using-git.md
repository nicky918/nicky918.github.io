---
layout: post
title: 编码训练：Ubuntu下git使用教程
categories: [Git]
tags: Git
---

## 安装git

```sh
sudo apt-get install git
```

## 检查SSH

GitHub用到了SSH，需要在shell里检查是否连接到GitHub:

```sh
vista@vistaMac ~/P/git> ssh -T git@github.com
Warning: Permanently added the RSA host key for IP address '192.30.255.113' to the list of known hosts.
Hi nicky918! You've successfully authenticated, but GitHub does not provide shell access.
```
上面表明添加成功。

## 克隆rep

比较常见的将远程版本库中的代码clone到本地的方式如下：

```sh
git clone https://git.oschina.net/vista918/SeetaFaceEngine.git
```

## 其他常用Git命令

```sh
git init # 初始化本地Git版本库
git add # 暂存文件，如果使用.表示当前目录及其子目录
git commit -m “first commit” # 提交，-m选项后跟内容为提交所用的注释
git remote -v # 查看当前项目远程连接的是哪个版本库地址
git push origin master # 将本地项目提交到远程版本库
git fetch origin # 取得远程更新（到origin/master），但还没有合并
git merge origin/master # 把更新的内容（origin/master）合并到本地分支（master）
git pull origin master # 相当于fetch和merge的合并，但分步操作更保险
git branch -r #查看分支
git checkout origin/master(gh-pages) #切换分支
git push origin :branch_you_want_to_delete #删除远程分支（注意空格，把一个空的branch赋值给已有的branch，这样就删除了）
```

## 示例

克隆下来后，便可以对项目的文件进行修改添加，修改添加完成后，提交到远程版本控制库：

```sh
vista@vistaMac ~/P/git> git add .
vista@vistaMac ~/P/git> git commit -m "update content in ch07"[gh-pages 221c8bc] update content in ch07
 1 file changed, 4 insertions(+)
```
在push之前，我们先来查看一下当前项目远程连接的是哪个版本库地址：

```sh
vista@vistaMac ~/P/g/nicky918.github.io> git remote -v
origin	https://github.com/nicky918/nicky918.github.io.git (fetch)
origin	https://github.com/nicky918/nicky918.github.io.git (push)
```
然后push:

```sh
git push origin master
or
git push oschina master
```
暂时就这样吧。

Reference:

[1].[Ubuntu下GitHub的使用](http://www.pythoner.com/263.html)

[2].[在Ubuntu下配置舒服的Python开发环境](http://xiaocong.github.io/blog/2013/06/18/customize-python-dev-environment-on-ubuntu/)
