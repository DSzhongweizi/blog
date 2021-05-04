---
title: GIT——分布式版本控制
date: 2018-01-05 00:00:00
---

{% tabs Git版本控制 %}
<!-- tab 结构与流程 -->
更新git  `git update-git-for-windows`
![git工作流程](https://cdn.jsdelivr.net/gh/DSzhongweizi/Resources/article/git1.png)

![git存储结构](https://cdn.jsdelivr.net/gh/DSzhongweizi/Resources/article/git2.jpg)
<!-- endtab -->
<!-- tab 配置 -->
### 配置
> Git 提供了一个叫做 `git config` 的工具，专门用来配置或读取相应的工作环境变量。

这些环境变量，决定了 Git 在各个环节的具体工作方式和行为，通常存放在以下三个不同的地方：
- /etc/gitconfig 文件：安装目录下，对所有用户都普遍适用的配置，`git config --system`命令可以读写。
- ~/.gitconfig 文件：用户目录下，只适用于该用户的配置，`git config --global` 命令可以读写。
- .git/config 文件：项目目录下，仅针对当前项目有效，`git config`命令可以读写。

**以上每一个级别的配置都会覆盖上层的相同配置（项目配置>用户配置>全局配置）**。
- `git config -l`可以查看所有的配置
- `git config [user.name]`可以查看具体属性的具体值

使用git之前，通常需要配置用户信息：
- `git config --global user.name 'runoob'` 
- `git config --global user.email test@runoob.com`

链接远程github仓库，采用SSH加密时：
- `ssh-keygen -t rsa -C "youremail@example.com"`
- `ssh -T git@github.com`
<!-- endtab -->

<!-- tab  基础命令-->
### 新建仓库
- `git init [dir]`：初始化仓库，生成了`.git`目录
  > 默认生成`main`分支
- `git clone <repo> [dir]` ：克隆仓库

### 添加/删除
- `git add <filename>`：添加需要跟踪的文件到暂存区，通常为变动文件     
- `git reset HEAD [-- filename]`：**取消**已添加到暂存区的内容
- `git rm [command] <filename>`：**删除**已添加到暂存区和工作区的内容
  - `--cached`：仅删除暂存区的文件
- `git mv <filename> [newfilename]`：改名文件，并且将这个改名放入暂存区
  > 重命名工作区文件并添加到暂存区（`rm + add`）

### 提交
- `git status [command]`：查看项目文件改动状态
  - `-s`：简短输出
- `git diff [command]`：查看未缓存的改动
  - `--cached`：查看已缓存的改动
  - `HEAD`：查看已缓存的与未缓存的所有改动
  - `--stat`：显示摘要而非整个diff
- `git commit [command]`：记录缓存内容的快照
  - `-m`：提供提交注释
  - `-a`：提交工作区自上次commit之后的变化，直接到仓库区（不包含新文件）
  - `-v`：提交时显示所有diff信息


### 分支管理
- `git branch [command]` ：列出本地分支
  - `-r`：列出所有远程分支
  - `-a`：列出所有本地分支和远程分支
- `git branch [command] <branchname>` ：新建分支
  - `-d`：删除本地分支
  - `git push origin --delete remoteBranchName`：删除远程分支
- `git checkout [command] <branchname>`：切换分支，更新工作区
  - `-b`：新建并切换分支
- `git merge <branch>`：合并指定分支到当前分支

### 提交历史
- `git log [command] [branch]`：查看提交历史
  - `[--oneline]`：查看历史记录的简洁
  - `[--graph]`：查看分支、合并记录
  - `[--reverse]`：逆向显示记录
  - `[--author=username]`:指定用户记录

### 打标签
- `git tag [command] [SHA]`：查看所有标签
  - `[-a]`：创建一个带注解的标签(推荐)
<!-- endtab -->

<!-- tab 远程仓库-->
### 远程仓库github
- `git remote [command]`：罗列、添加和删除远程仓库
  - `-v`：显示每个别名的实际链接地址
  - `add [repo] [url]`：为项目添加一个新的远端仓库
  - `rm [repo]`：删除现存的某个别名
- `git fetch`：从远端仓库下载新分支与数据
  > 该命令还需要配合`git merge`合并远程分支到你所在的分支
- `git pull`：从远端仓库提取数据并尝试合并到当前分支
- `git push [command]`：推送新分支与数据到远程仓库
  - `[repo] [branch]`
  > 在你上次提取、合并之后，另有人推送了，Git 服务器会拒绝你的推送，知道你是最新的为止(pull)。

### Git服务器搭建
`Github`的项目是免费公开的，需要私有仓库需要收费（现在好像也免费了）

自己可以搭建一个私有`git`远程服务器作为私用
1. 安装Git

```
$ yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel perl-devel
$ yum install git
```
接下来我们 创建一个`git`用户组和用户，用来运行`git`服务：
```
$ groupadd git
$ adduser git -g git
```

2. 创建证书登录

收集所有需要登录的用户的公钥，公钥位于`id_rsa.pub`文件中，把我们的公钥导入到`/home/git/.ssh/authorized_keys`文件里，一行一个。

如果没有该文件创建它：
```
$ cd /home/git/$ mkdir .ssh
$ chmod 700 .ssh
$ touch .ssh/authorized_keys
$ chmod 600 .ssh/authorized_keys
```

3. 初始化Git仓库

首先我们选定一个目录作为`Git`仓库，假定是`/home/gitrepo/runoob.git`，在`/home/gitrepo`目录下输入命令：
```
$ cd /home
$ mkdir gitrepo
$ chown git:git gitrepo/$ cd gitrepo

$ git init --bare runoob.gitInitialized empty Git repository in /home/gitrepo/runoob.git/
```
以上命令`Git`创建一个空仓库，服务器上的`Git`仓库通常都以`.git`结尾。然后，把仓库所属用户改为`git`：
```
$ chown -R git:git runoob.git
```

4. 克隆仓库

```
$ git clone git@192.168.45.4:/home/gitrepo/runoob.gitCloning into 'runoob'...warning: You appear to have cloned an empty repository.Checking connectivity... done.
```
`192.168.45.4` 为 `Git` 所在服务器 `ip` ，你需要将其修改为你自己的 `Git` 服务 `ip`。

这样我们的 `Git` 服务器安装就完成了，接下来我们可以禁用 `git` 用户通过`shell`登录，可以通过编辑`/etc/passwd`文件完成。找到类似下面的一行：
```
git:x:503:503::/home/git:/bin/bash
```
改为：
```
git:x:503:503::/home/git:/sbin/nologin
```
<!-- endtab -->

{% endtabs %}



参考：
- [Git 参考手册](http://gitref.justjavac.com/inspect/)
- [#GIT](https://chinese.freecodecamp.org/news/tag/git/)
