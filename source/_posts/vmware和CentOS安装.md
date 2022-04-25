---
title: vmware和CentOS安装
abbrlink: 41fd3eb0
author: 小小千辰
date: 2022-01-24 15:38:58
updated: 2022-01-24
tags:
  - Linux
categories:
  - 千辰的小小笔记
---

## vmware安装

![image-20220124164640954](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241801259.png)

![image-20220124164705690](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241801432.png)

![image-20220124164715792](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241801297.png)



![image-20220124164727399](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241801018.png)

![image-20220124164737352](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241801725.png)

![image-20220124164752043](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241801588.png)

![image-20220124164806160](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241801970.png)

![image-20220124164814756](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241801323.png)

![image-20220124164828036](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241802993.png)

> 密钥：UG5J2-0ME12-M89WY-NPWXX-WQH88

![image-20220124164840286](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241802836.png)

## CentOS安装

### 检查BIOS虚拟化支持

> 开机状态按 F10 进入BIOS界面 找到虚拟化设置打开即可。

![image-20220124154108861](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241802868.png)_bios检查_

> 这个地方我在安装的时候没有管，但是还是安装成功了。



### 新建虚拟机

![image-20220124154243759](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241802479.png)

![image-20220124154254231](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241802809.png)

![image-20220124154303320](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241802430.png)_稍后安装_

![image-20220124154310577](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241802774.png)_linux_

![image-20220124154319042](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241802634.png)_选择路径_



> 这个地方看自己的实际

![image-20220124154551691](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241802975.png)

![image-20220124154601134](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241802546.png)

![image-20220124154613541](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241802024.png)_NAT模式_

![image-20220124154632296](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241802267.png)

### 正式安装Centos系统

![image-20220124154653285](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241802400.png)_选择下载好的CentOS_

![image-20220124154704516](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241802019.png)_开启虚拟机_

> 选择第一个，不需要 Test this media ，否则检测时间很长

![image-20220124154713178](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241802865.png)

![image-20220124154808510](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241802658.png)

![image-20220124154817206](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241802915.png)



![image-20220124154825215](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241802146.png)

<blockquote class="danger">
	等上一步加载完之后再去选择分区
</blockquote>




![image-20220124154832214](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241802984.png)

![image-20220124154839559](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241802269.png)

![image-20220124154846131](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241802736.png)

![image-20220124154925931](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241803947.png)

![image-20220124154933341](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241802256.png)

![image-20220124154939598](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241802095.png)

![image-20220124154945796](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241802679.png)

![image-20220124154952903](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241802137.png)

![image-20220124154958544](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241803265.png)

![image-20220124155005426](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241803580.png)

![image-20220124155011860](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241803156.png)

![image-20220124155020263](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241803621.png)

![image-20220124155027062](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241803712.png)

![image-20220124155033579](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241803401.png)

![image-20220124155040032](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241803855.png)

![image-20220124155047530](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241803058.png)

![image-20220124155053804](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241803547.png)

![image-20220124155101837](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241803534.png)

![image-20220124155108344](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241803329.png)

![image-20220124155115067](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241803886.png)

![image-20220124155121133](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241803793.png)

![image-20220124155126903](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241803657.png)

![image-20220124155136739](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241803301.png)

![image-20220124155147076](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241803926.png)

![image-20220124155153817](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241803035.png)

![image-20220124155203384](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241803282.png)

![image-20220124155208110](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241803446.png)

![image-20220124155214211](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241803648.png)

![image-20220124155219092](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241803201.png)
