---
title: Github Fork同步
tags:
  - Git
categories:
  - 千辰的小小笔记
abbrlink: d14bdd4f
date: 2023-04-13 10:37:57
updated: 2023-04-13 10:37:57
---

1. 查看远程仓库路径<br>
   `git remote -v`

2. 设置上游服务器<br>
   `git remote add upstream xxx.git`

3. 拉取上游服务器的代码，并且强制提交（适用于只是使用别人的项目而不提交自己代码的项目）<br>
   `git fetch upstream && git reset --hard upstream/main && git push -f`

*<!--more-->*
