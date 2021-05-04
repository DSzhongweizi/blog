---
title: git教程
categories:
  - git教程
tags:
  - git
cover: 'https://cdn.jsdelivr.net/gh/DSzhongweizi/Resources/article/20200530121403.png'
abbrlink: a022e3d6
date: 2020-05-25 08:00:32
---

## 诞生
1991年，Linus创建了开源的Linux，而后全世界的志愿者参与Linux的开源开发。自那时候的十年里，由于出现的版本控制系统，要么是集中式（速度慢），要么是商业收费（违背Linux开源精神），导致Linux的版本管理一直是通过diff的方式把源代码文件发给Linus，然后由他本人通过手工方式合并代码。

2002年，Linux系统已经发展了十年了，代码库之大让Linus很难继续通过手工方式管理了。于是Linus选择了一个商业的版本控制系统BitKeeper，BitKeeper的东家BitMover公司出于人道主义精神，授权Linux社区免费使用这个版本控制系统。

这种白嫖的局面一直到2005年被打破，起因是社区的一些牛人试图破解BitKeeper的协议，不料事情败露。BitMover公司怒了，决定收回Linux社区的免费使用权。

随后Linus花费了几周的时间，用C写了一个分布式版本控制系统，这就是Git。
## 简介
### 安装
[安装Git](https://www.liaoxuefeng.com/wiki/896043488029600/896067074338496)
### 创建版本库
打开一个尽量不含中文的目录，使用`git init`命令把这个目录变成Git可以管理的本地仓库，目录下会生成git用来跟踪管理版本库的`.git`文件夹。
### 工作区和暂存区
Git管理的文件分为：工作区，版本库，版本库又分为暂存区stage和仓库分支master
<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);display:inline;margin:0" 
    src="https://cdn.jsdelivr.net/gh/DSzhongweizi/Resources/article/20200529224242.jpg
" width=400 />
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;">工作区和stage（暂存区）</div>
</center>

## 本地仓库管理
### 添加文件到版本库
> 注意，所有的版本控制系统，其实只能跟踪文本文件的改动，比如TXT文件，网页，所有的程序代码等等，对于图片视频这样的二进制文件，git无法跟踪具体的内容变化。
- 使用命令`git add <file>`把文件添加到仓库

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);display:inline;margin:0" 
    src="https://cdn.jsdelivr.net/gh/DSzhongweizi/Resources/article/20200529224807.jpg
" width=400 />
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;">git add file</div>
</center>

> 可反复多次使用，添加多个文件
- 使用命令`git commit -m <message>`把文件提交到仓库。

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);display:inline;margin:0" 
    src="https://cdn.jsdelivr.net/gh/DSzhongweizi/Resources/article/20200529224925.jpg
" width=400 />
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;">git commit -m message</div>
</center>

> 本次提交的说明，建议有意义内容，方便查看历史记录

### 查看文件状态
要随时掌握工作区的状态，使用`git status`命令。

如果`git status`告诉你有文件被修改过，用`git diff`可以查看修改内容。
- git diff：查看工作区和暂存区差异，
- git diff --cached：查看暂存区和仓库差异，
- git diff HEAD：查看工作区和仓库的差异，
### 版本回退
`HEAD`指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令`git reset --hard commit_id`。
> 上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上100个版本写100个^比较容易数不过来，所以写成`HEAD~100`

穿梭前，用`git log`可以查看提交历史，以便确定要回退到哪个版本。

要重返未来，用`git reflog`查看命令历史，以便确定要回到未来的哪个版本。
### 撤销修改
1. 没有`git add`时，用`git restore --file`（工作区回退）

2. 已经`git add`时，先`git restore --staged <file>`回退到1（暂存区回退），再按1操作（工作区回退）

3. 已经`git commit`时，用`git reset`回退版本（版本回退），前提是没有推送到远程库，否则GG

### 删除文件
直接在文件管理器中把没用的文件删了，或者用rm命令删了
- 的确要把文件file删掉，那么可以执行 `git rm file` 和`git commit -m msg` 然后文件就被删掉了
- 删错文件了，不应该删文件file，注意这时只执行了rm file，还没有提交，所以可以执行`git checkout file`将文件恢复。

## 分支管理
分支对于多人协作很有用，可以帮助你较好地管理自己写的功能代码进度，而不影响别人，整个功能写完后合并到主分支就行。
> 功能代码有缺陷提交到主分支对项目整体有影响，不提交又没法进度管理，分支可以解决这样的问题。

### 创建分支
HEAD指向的就是当前分支，当前分支指向最新的提交。
<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);display:inline;margin:0" 
    src="https://cdn.jsdelivr.net/gh/DSzhongweizi/Resources/article/20200530001211.png
" width=400 />
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;">git commit -m message</div>
</center>

- 创建分支`git branch <name>`
- 切换分支`git checkout <name>`或者`git switch <name>`
>上面两个命令可以合并为`git checkout -b <name>`或者`git switch -c <name>`
### 查看当前分支
`git branch` 查看当前分支
### 合并分支
`git merge <name>` 将新分支合并到master上
> 默认`fast forward`合并方式

加上`--no-ff`参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并

在实际开发中，我们应该按照几个基本原则进行分支合并管理：
- `master`分支应该是非常稳定的，也就是仅用来发布新版本 
- `dev`是不稳定的，用于协作成员将各自的分支合并到这个分支
<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);display:inline;margin:0" 
    src="https://cdn.jsdelivr.net/gh/DSzhongweizi/Resources/article/20200530100922.png
" width=400 />
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;">分支合并管理</div>
</center>

### 删除分支
`git branch -d <name>` 删除新分支
### Bug分支
修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；

当手头工作没有完成时，先把工作现场`git stash`一下，然后去修复bug，bug修复后可以通过`git stash list`查看之前保存的若干现场，再使用`git stash pop stash@{0}`，回到工作现场；

在master分支上修复的bug，想要合并到当前dev分支，可以用`git cherry-pick <commit>`命令，把bug提交的修改“复制”到当前分支，避免重复劳动。
### Feature分支
每添加一个新功能，最好新建一个feature分支，在上面开发，完成后，合并，最后，删除该feature分支。
> 一般不建议删除，可以像bug分支一样stash隐藏起来，防止以后对新功能修改。

如果要丢弃一个没有被合并过的分支，可以通过`git branch -D <name>`强行删除。
### Rebase
[Rebase用法](http://gitbook.liuhui998.com/4_2.html)
## 远程仓库管理
要克隆一个仓库，首先必须知道仓库的地址，然后使用`git clone`命令克隆。

Git支持多种协议，包括https，但ssh协议速度最快，但是在某些只开放http端口的公司内部就无法使用ssh协议而只能用https。

当从远程仓库克隆时，实际上Git自动把本地的master分支和远程的master分支对应起来了，并且，远程仓库的默认名称是origin。

- 查看远程库的信息，使用`git remote`
> `git remote -v`显示更详细内容

- 本地新建的分支如果不推送到远程，对其他人就是不可见的；
- 从本地推送分支，使用`git push origin branch-name`，如果推送失败，则因为远程分支比你的本地更新，先用git pull抓取远程的新提交；
- 从远程抓取分支，使用`git pull`，如果有冲突，要先处理冲突。
> 提示no tracking information，则说明本地分支和远程分支的链接关系没有创建
- 在本地创建和远程分支对应的分支，使用`git checkout -b branch-name origin/branch-name`，本地和远程分支的名称最好一致；

- 建立本地分支和远程分支的关联，使用`git branch --set-upstream branch-name origin/branch-name`；

## 其他
当我们对Git的提交、分支已经非常熟悉，可以熟练使用命令操作Git后，再使用GUI工具，就可以更高效。

[使用SourceTree](https://www.liaoxuefeng.com/wiki/896043488029600/1317161920364578)

[Git Cheat Sheet中文版](https://www.kancloud.cn/thinkphp/git-cheat-sheet/39649)