---
title: 解决使用Clash无法进行One-Drive同步
tags:
  - Clash
  - OneDrive
categories:
  - 杂项
abbrlink: 833537f6
date: 2023-04-12 10:37
updated: 2023-04-12 10:37
---

*<!--more-->*

> 当我们打开代理软件的时候，一些微软服务会出现无法登陆的情况。

## 解决方案一（解决）

> loopback勾选你的**账户**和**工作或学校账户**后保存即可。

![image-20230413084022082](https://qianchen-image.oss-cn-beijing.aliyuncs.com/blog/202304130902173.png)

> 网上有方案说排除全部，经过测试只能实现登陆，没办法同步。

## 解决方案二

> ~~但是这种情况，我没办法打开Bypass Domain/IPNet，打开编辑一闪而过...~~      
> 解决了，是因为选择的默认文本编译器的原因。

1. 打开Clash for Windows界面
2. 打开`Settings`
3. 在`System Proxy`栏目下找到`Bypass Domain/IPNet`，点击行尾的`Edit`
4. 加上`- 'login.microsoftonline.com'`
5. 重启Clash与Onedrive
