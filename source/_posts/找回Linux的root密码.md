---
title: 找回Linux的root密码
abbrlink: 9c94a23c
date: 2022-01-25 22:03:54
author: 小小千辰
updated: 2022-01-25 22:03:54
tags:
  - Linux
categories:
  - 千辰的小小笔记
---





> 首先，启动系统，进入开机界面，在界面中按“e”进入编辑界面。如图

![image-20220125220715772](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241751768.png)

> 进入编辑界面，使用键盘上的上下键把光标往下移动，找到以““Linux16”开头内容所在的行数”，在行的最后面输入：`init=/bin/sh`。如图

![image-20220125220802056](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241752541.png)

> 接着，输入完成后，直接按快捷键：Ctrl+x 进入**单用户模式**。

> 接着，在光标闪烁的位置中输入：mount -o remount,rw /（注意：各个单词间有空格），完成后按键盘的回车键（Enter）。如图

![image-20220125220831432](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241752127.png)

> 在新的一行最后面输入：`passwd`， 完成后按键盘的回车键（Enter）。输入密码，**然后再次确认密码即**可(**密码长度最好8位以上,但不是必须的)**, 密码修改成功后，会显示passwd.....的样式，说明密码修改成功

![image-20220125220934168](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241752966.png)

> 接着，在鼠标闪烁的位置中（最后一行中）输入：touch /.autorelabel（注意：touch与 /后面有一个空格），完成后按键盘的回车键（Enter）

> 继续在光标闪烁的位置中，输入：exec /sbin/init（注意：exec与 /后面有一个空格），完成后按键盘的回车键（Enter）,等待系统自动修改密码(**这个过程时间可能有点长，耐心等待**)，完成后，系统会自动重启, **新的密码生效**了

![image-20220125221008225](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241752303.png)

