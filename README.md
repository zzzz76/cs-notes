## 前言

本项目用于对网络资源进行整合和精化，所有非原创文章，均提供原文链接。



## 开始

一个敲笔记的新方法，足够的简洁、轻量级、易于管理。

* Typora 文字编写，唯美轻量
* Git 版本控制，快速迭代
* GitBook 结构管理，简洁有序



### GitBook 使用

```sh
gitbook init //初始化目录文件
gitbook help //列出gitbook所有的命令
gitbook --help //输出gitbook-cli的帮助信息
gitbook build //生成静态网页
gitbook serve //生成静态网页并运行服务器
gitbook build --gitbook=2.0.1 //生成时指定gitbook的版本, 本地没有会先下载
gitbook ls //列出本地所有的gitbook版本
gitbook ls-remote //列出远程可用的gitbook版本
gitbook fetch 标签/版本号 //安装对应的gitbook版本
gitbook update //更新到gitbook的最新版本
gitbook uninstall 2.0.1 //卸载对应的gitbook版本
gitbook build --log=debug //指定log的级别
gitbook builid --debug //输出错误信息
```



### Git 使用

```sh
git init
git clone
git config --list
git config -e --global
git config --global user.name "name"
git config --global user.email "email"
git add
git rm
git commit -m [message]
git branch -a
git checkout branch-name
git checkout -b branch-name
git cherry-pick branch-name
git status
git log
git diff
git push [remote] [branch]
git reset --hard
git reset --hard [commit]
git commit --amend
git config --global core.editor vim
```



### Typora 使用

