---
title: Git分布式版本控制工具
author: 小小千辰
tags:
  - Git
  - 命令行
categories:
  - 千辰的小小笔记
abbrlink: e01dd7d7
date: 2021-10-31 11:44:58
updated: 2021-10-31 11:44:58
---

## 概述

### 开发中的实际场景

```
场景一：备份 
	小明负责的模块就要完成了，就在即将Release之前的一瞬间，电脑突然蓝屏，硬盘光荣牺牲！几个月 来的努力付之东流 
	
场景二：代码还原 
	这个项目中需要一个很复杂的功能，老王摸索了一个星期终于有眉目了，可是这被改得面目全非的 代码已经回不到从前了。什么地方能买到哆啦A梦的时光机啊？ 
	
场景三：协同开发 
	小刚和小强先后从文件服务器上下载了同一个文件：Analysis.java。小刚在Analysis.java 文件中的第30行声明了一个方法，叫count()，先保存到了文件服务器上；小强在Analysis.java文件中的 第50行声明了一个方法，叫sum()，也随后保存到了文件服务器上，于是，count()方法就只存在于小刚的记忆中了
    
场景四：追溯问题代码的编写人和编写时间！ 
	老王是另一位项目经理，每次因为项目进度挨骂之后，他都不知道该扣哪个程序员的工资！就拿这 次来说吧，有个Bug调试了30多个小时才知道是因为相关属性没有在应用初始化时赋值！可是二胖、王东、刘 流和正经牛都不承认是自己干的！
```

<br>

### 版本控制器的方式

```
a、集中式版本控制工具 
	集中式版本控制工具，版本库是集中存放在中央服务器的，team里每个人work时从中央服务器下载代码，是必须联网才能工作，局域网或互联网。个人修改后然后提交到中央版本库。 
	举例：SVN和CVS 
b、分布式版本控制工具 
	分布式版本控制系统没有“中央服务器”，每个人的电脑上都是一个完整的版本库，这样工作的时候，无 需要联网了，因为版本库就在你自己的电脑上。多人协作只需要各自的修改推送给对方，就能互相看到对方的 修改了。
	举例：Git
```

<br>

### Git

```
	Git是分布式的,Git不需要有中心服务器，我们每台电脑拥有的东西都是一样的。我们使用Git并且有个 中心服务器，仅仅是为了方便交换大家的修改，但是这个服务器的地位和我们每个人的PC是一样的。我们可以 把它当做一个开发者的pc就可以就是为了大家代码容易交流不关机用的。没有它大家一样可以工作，只不 过“交换”修改不方便而已。 
	git是一个开源的分布式版本控制系统，可以有效、高速地处理从很小到非常大的项目版本管理。Git是 Linus Torvalds 为了帮助管理 Linux 内核开发而开发的一个开放源码的版本控制软件。 同生活中的许多伟大事物一样，Git 诞生于一个极富纷争大举创新的年代。Linux 内核开源项目有着为数众 多的参与者。 绝大多数的 Linux 内核维护工作都花在了提交补丁和保存归档的繁琐事务上（1991－2002 年间）。 到 2002 年，整个项目组开始启用一个专有的分布式版本控制系统 BitKeeper 来管理和维护代 码。
	到了 2005 年，开发 BitKeeper 的商业公司同 Linux 内核开源社区的合作关系结束，他们收回了 Linux 内核社区免费使用 BitKeeper 的权力。 这就迫使 Linux 开源社区（特别是 Linux 的缔造者 Linus Torvalds）基于使用 BitKeeper 时的经验教训，开发出自己的版本系统。 他们对新的系统制订 了若干目标： 
	速度
	简单的设计 
	对非线性开发模式的强力支持（允许成千上万个并行开发的分支） 
	完全分布式 
	有能力高效管理类似 Linux 内核一样的超大规模项目（速度和数据量）
```

![image-20211031115201670](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204262021579.png)_Git工作图示_

<br>

### Git工作流程图

![image-20211031115301829](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241829906.png)

命令如下：

1. clone（克隆）：从远程仓库中克隆代码到本地仓库
2. checkout（检出）：从本地仓库中检出一个仓库分支然后进行修订
3. add（添加）：在提交前先将代码提交到暂存区
4. commit（提交）：提交到本地仓库。本地仓库中保存修改的各个历史版本
5. fetch（抓取）：从远程库，抓取到本地仓库，不进行任何的合并动作，一般操作比较少
6. pull（拉取）：从远程库拉到本地库，自动进行合并（merge），然后放到工作区，相当于fetch+merge
7. push（推送）：修改完成后，需要和团队成员共享代码时，将代码推送到远程仓库

<br>

## Git安装与常用命令

* ls/ll 查看当前目录
* cat 查看文件内容
* touch 创建文件

### Git环境配置

#### 下载与安装

这个地方就直接省略了

<br>

#### 为常用指令配置别名

1. 打开用户目录，创建`.bashrc`文件

部分windows系统不允许用户创建点号开头的文件，可以打开gitBash,执行 `touch ~/.bashrc`

![image-20211031120503148](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241829228.png)

2. 在`.bashrc`文件中输入如下内容：

```bash
#用于输出git提交日志 
alias git-log='git log --pretty=oneline --all --graph --abbrev-commit' 
#用于输出当前目录所有文件及基本信息 
alias ll='ls -al'
```

3. 打开gitBash，执行`source ~/.bashrc`

![image-20211031121213364](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241829857.png)

<br>

#### 解决GitBash乱码问题

1. 打开GitBash执行如下命令

```
git config --global core.quotepath false
```

2. ${git_home}/etc/bash.bashrc 文件最后加入下面两行

```
export LANG="zh_CN.UTF-8" 
export LC_ALL="zh_CN.UTF-8"
```

<br>

### 获取本地仓库

要使用Git对我们的代码进行版本控制，首先需要获得本地仓库

1. 在电脑的任意位置创建一个空目录（例如test）作为我们的本地Git仓库
2. 进入这个目录中，点击右键打开Git bash窗口
3. 执行命令`git init`
4. 如果创建成功后可在文件夹下看到隐藏的.git目录。

<br>

### 基础操作命令

Git工作目录下对于文件的**修改**(增加、删除、更新)会存在几个状态，这些**修改**的状态会随着我们执行Git

的命令而发生变化。

![image-20211031121700662](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241829026.png)

1. `git add`（工作区->暂存区）
2. `git commit`（暂存区->本地仓库）

#### ***查看修改的状态（status）**

* 作用：查看的修改的状态（暂存区、工作区）
* 命令形式：`git status`

#### ***添加工作区到暂存区（add）**

* 作用：添加工作区一个或多个文件的修改到暂存区
* 命令形式：git add 单个文件名|通配符
  * 将所有的修改加入暂存区：`git add .`

#### ***提交暂存区到本地仓库（commit）**

* 作用：提交暂存区内容到本地仓库的当前分支
* 命令形式：`git commit -m “注释内容”`

#### ***查看提交日志**

在2.1.2中配置的别名`git-log`包含了这些参数，直接可以使用命令`git-log`

* 作用：查看提交记录
* 命令形式：`git log[option]`
  * options
    * --all 显示所有分支
    * --pretty=oneline 将提交信息显示为一行
    * --abbrev-commit 是的输出的commitId更简短
    * --graph 以图的形式显示

#### 版本回退

* 作用：版本切换
* 命令形式：`git reset --hard commitId`
  * `commitID` 可以使用`git-log`或`git log` 指令查看
* 如何查看已经删除的记录？
  * `git reflog`
  * 这个指令可以看到已经删除的提交记录

<br>

### 分支

几乎所有的版本控制系统都以某种形式支持分支。 使用分支意味着你可以把你的工作从开发主线上分离开来进行重大的Bug修改、开发新的功能，以免影响开发主线。

#### 查看本地分支

* 命令：`git branch`

#### 创建本地分支

* 命令：`git branch` 分支名

#### ***切换分支（checkout）**

* 命令：`git checkout` 分支名

我们该可以直接切换到一个不存在的分支（创建并切换）

* 命令：`git checkout -b` 分支名

#### ***合并分支（merge）**

一个分支上的提交可以合并到另一个分支

命令：`git merge 分支名称`

#### 删除分支

**不能删除当前分支，只能删除其他分支**

`git branch -d b1` 删除分支时，需要做各种检查

`git branch -D b1` 不做任何检查，强制删除

#### 解决冲突

当两个分支上对文件的修改可能会存在冲突，例如修改了同一个文件的同一行，这时就需要手动解决冲突，解决冲突步骤如下：

1. 处理文件中冲突的地方
2. 将解决完冲突的文件加入暂存区（add）
3. 提交到仓库（commit）

#### 开发中分支的使用原则与流程

![image-20211031123904655](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241829597.png)

<br>

## 在idea中使用Git

### 在idea中配置Git

安装好IntelliJ IDEA后，如果Git安装在默认路径下，那么idea会自动找到git的位置，如果更改了Git的安装位置则需要手动配置下Git的路径。选择File→Settings打开设置窗口，找到Version Control下的git选项：

![image-20211031124100453](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241829170.png)

点击Test按钮,现在执行成功，配置完成

![image-20211031124109763](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241829682.png)

### 在idea中操作Git

场景：本地已经有一个项目，但是并不是git项目，我们需要将这个放到码云的仓库里，和其他开发人员继续一起协作开发。

#### 创建项目远程仓库

![image-20211031124319046](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241829913.png)

#### 初始化本地仓库

![image-20211031124341663](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241829725.png)

#### 提交到本地仓库

![image-20211031124401866](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241829867.png)

#### 推送到远程仓库

![image-20211031124420819](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241830222.png)

#### 克隆远程仓库到本地

![image-20211031124446658](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241830358.png)

#### 创建分支

* 最常规的方式

![image-20211031124513171](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241830231.png)

* 最强大的方式

![image-20211031124528608](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241830761.png)

#### 切换分支及其他分支相关操作

![image-20211031124555656](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241830278.png)

#### 解决冲突

1. 执行merge或pull操作时，可能发生冲突

![image-20211031124646003](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241830167.png)

2. 冲入解决后加入暂存区

   略

3. 提交到本地仓库

   略

4. 推送到远程仓库

   略

<br>

### IDEA常用GIT操作入口

1. 第一张图的快捷入口可以基本满足开发需求

   ![image-20211031124843609](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241830778.png)

2. 第二张图是更多在IDEA操作git的入口。

   ![image-20211031124902880](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241830478.png)

<br>

<br>

## 附：几条贴令

1. **切换分支前先提交本地的修改**
2. 代码及时提交，提交过了就不会丢
3. 遇到任何问题都不要删除文件目录

<br>

## 附：IDEA集成GitBash作为Terminal

![image-20211031125235066](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241830643.png)

