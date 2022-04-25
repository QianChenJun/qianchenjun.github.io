---
title: 通关Linux
abbrlink: 30b5ce3c
author: 小小千辰
date: 2022-01-24 14:59:31
tags:
  - Linux
categories:
  - 千辰的小小笔记
---





## Linux简介

### Linux使用在哪些地方

![image-20220124150823993](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241815675.png)

### Linux的应用领域

#### 个人应用的领域

​		此领域是传统 linux 应用薄弱的环节，近些年来随着 ubuntu、fedora [fɪˈdɔ:rə] 等优秀桌面环境的兴起，linux 在个人桌面领域的占有率在逐渐的提高。

![image-20220124150947251](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241815949.png)

#### 服务器领域

​		linux 在**服务器领域**的应用是最强的。
​		linux **免费、稳定、高效**等特点在这里得到了很好的体现，尤其在一些高端领域尤为广泛（c/c++/php/java/python/go）。

#### 嵌入式领域

​		linux 运行稳定、对网络的良好支持性、低成本，且可以根据需要进行**软件裁剪**，内核最小可以达到几百 KB 等特点，使其近些年来在**嵌入式领域**的应用得到非常大的提高。
​		主要应用：机顶盒、数字电视、网络电话、程控交换机、手机、PDA、智能家居、智能硬件等都是其应用领域。以后在**物联网中应用会更加广泛**。



### Linux入门

#### 概述

1. 吉祥物tux

   Linux的吉祥物

   <img src="https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241815074.png" alt="image-20220124151625321" style="zoom:50%;" />_tux_

2. Linux的主要发行版：

   **Ubuntu(乌班图)、RedHat(红帽)、CentOS**、Debain[蝶变]、Fedora、SuSE、OpenSUSE

#### Linux和Unix的关系

1. Unix怎么来的

   ![image-20220124151942293](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241815337.png)

2. Linux怎么来的

   ![image-20220124152003856](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241815839.png)_GNU计划_

3. Linux和Unix的关系

   ![image-20220124152030858](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241815754.png)



## VM和Linux的安装

### 安装vm和Centos

1. 先安装 virtual machine 15.5
2. 再安装 Linux (CentOS 7.6/centOS8.1)
3. <img src="https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241815095.png" alt="image-20220124152215266" style="zoom:80%;" />_原理示意图_

### vmware15.5 下载

1. [官方地址](https://www.vmware.com/cn.html)
2. [其他地址](https://www.nocmd.com/windows/740.html)

### Centos 下载地址

> 迅雷复制即可

1. CentOS-7-x86_64-DVD-1810.iso CentOS 7.6 DVD 版 4G (目前主流的生产环境)
   [7.6下载地址](http://mirrors.163.com/centos/7.6.1810/isos/x86_64/CentOS-7-x86_64-DVD-1810.iso)
2. CentOS-8.1.1911-x86_64-dvd1.iso CentOS 8.1 DVD 版 8G (未来的主流.)
   [8.1下载地址](https://mirrors.aliyun.com/centos/8.1.1911/isos/x86_64/CentOS-8.1.1911-x86_64-dvd1.iso)

### CentOS 安装的步骤

> [vmware和CentOS安装](https://www.qianchen.xyz/posts/41fd3eb0/)

1. 创建虚拟机

2. 安装CentOS

3. CentOS 安装难点-网络连接方式

   ![image-20220124153505674](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241816420.png)_网络连接的三种模式_

   



### 虚拟机克隆

1. 直接拷贝一份安装好的虚拟机文件

2. 使用vmware的克隆操作，克隆的时候需要先关闭linux系统（右键虚拟机->管理->克隆）

   ![image-20220124201414920](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241816781.png)_没有将linux系统关机_



### 虚拟机快照

> 　可以理解为游戏的存档，windows的系统还原点

![image-20220124202410519](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241816824.png)_图示_

> 右键虚拟机->快照->拍摄快照/快照管理器

![image-20220124202425700](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241816072.png)_快照管理器_

### 虚拟机的迁移和删除

* 迁移：把安装好的虚拟系统这个**文件夹整体拷贝或者剪切**到另外位置使用。
* 删除：用**vmware进行删除**，再点击菜单->从磁盘删除即可（或者**直接手动删除虚拟机所在的文件夹**即可）

### 安装vmtools

> 目的：让 windows 和 centos 共享文件夹
>
> ![image-20220124204913715](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241816383.png)

#### 安装 vmtools 步骤

1. 进入 centos

2. 点击 vm 菜单的->install vmware tools

3. centos 桌面会出现一个 vm 的光盘

4. 打开这个光盘找到一个压缩包 `xxx.tar.gz`

   ![image-20220224151727395](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241816058.png)_示意图_

5. 拷贝到 /opt

6. 使用解压命令 tar, 得到一个安装文件

   ```shell
   cd /opt [进入到 opt 目录]
   tar -zxvf xx.tar.gz [解压]
   ```

7. 进入该 vm 解压的目录 , /opt 目录下 `cd vmware...`

8. 安装 `./vmware-install.pl`

9. 全部使用默认设置即可（一直回车）, 就可以安装成功

10. 注意：安装 vmtools 需要有 gcc . 

   ```shell
   gcc -v 查看gcc版本
   ```

#### 设置共享文件夹

![image-20220124205743063](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241816556.png)

1. windows 和 centos 可共享 E:\Linux\MyShare 目录可以读写文件了

2. 共享文件在 centos 的 /mnt/hgfs下

   ![image-20220124205931072](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241816974.png)



<b style="color: #FF0000">注意：在实际开发中，文件的上传下载是需要使用远程方式完成的</b>

## 目录结构

### 基本介绍

1. linux 的文件系统是采用级层式的树状目录结构，在此结构中的最上层是根目录“/”，然后在此目录下再创建其他的目录。
3. 记住一句经典的话：**在 Linux 世界里，一切皆文件！！**
4. ![image-20220124210310432](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241816146.png)_Linux目录结构_

### 具体目录结构

1. <font color="#62d1d2"><b>/bin [常用]</b></font> (/usr/bin 、 /usr/local/bin) 是 Binary 的缩写, 这个目录存放着最经常使用的命令
2. /sbin (/usr/sbin 、 /usr/local/sbin) s 就是 Super User 的意思，这里存放的是系统管理员使用的系统管理程序。
3. <font color="#62d1d2"><b>/home [常用]</b></font>  存放普通用户的主目录，在 Linux 中每个用户都有一个自己的目录，一般该目录名是以用户的账号命名
4. <font color="#62d1d2"><b>/root [常用] </b></font> 该目录为系统管理员，也称作超级权限者的用户主目录
5. /lib 系统开机所需要最基本的动态连接共享库，其作用类似于 Windows 里的 DLL 文件。几乎所有的应用程序都需要用到这些共享库
6. /lost+found 这个目录一般情况下是空的，当系统非法关机后，这里就存放了一些文件
7. <font color="#62d1d2"><b>/etc [常用] </b></font> 所有的系统管理所需要的配置文件和子目录, 比如安装 mysql 数据库 my.conf\
8. <font color="#62d1d2"><b>/usr [常用]</b></font> 这是一个非常重要的目录，用户的很多应用程序和文件都放在这个目录下，类似与 windows下的 program files 目录。
9. <font color="#62d1d2"><b>/boot [常用]</b></font> 存放的是启动 Linux 时使用的一些核心文件，包括一些连接文件以及镜像文件
10. <font color="#FF0000"><b>/proc [不能动] </b></font> 这个目录是一个虚拟的目录，它是系统内存的映射，访问这个目录来获取系统信息
11. <font color="#FF0000"><b>/srv [不能动] </b></font> service 缩写，该目录存放一些服务启动之后需要提取的数据
12. <font color="#FF0000"><b>/sys [不能动]</b></font> 这是 linux2.6 内核的一个很大的变化。该目录下安装了 2.6 内核中新出现的一个文件系统sysfs
13. /tmp 这个目录是用来存放一些临时文件的
14. /dev 类似于 windows 的设备管理器，把所有的硬件用文件的形式存储
15. <font color="#62d1d2"><b>/media [常用]</b></font> linux 系统会自动识别一些设备，例如 U 盘、光驱等等，当识别后，linux 会把识别的设备挂载到这个目录下
16. <font color="#62d1d2"><b>mnt [常用]</b></font> 系统提供该目录是为了让用户临时挂载别的文件系统的，我们可以将外部的存储挂载在/mnt/上，然后进入该目录就可以查看里的内容了。
17. /opt 这是给主机额外安装软件所存放的目录。如安装 ORACLE 数据库就可放到该目录下。默认为空
18. <font color="#62d1d2"><b>/usr/local </b></font> [常用] 这是另一个给主机额外安装软件所安装的目录。一般是通过编译源码方式安装的程序
19. <font color="#62d1d2"><b>/var [常用] </b></font> 这个目录中存放着在不断扩充着的东西，习惯将经常被修改的目录放在这个目录下。包括各种日志文件
20. /selinux [security-enhanced linux] SELinux 是一种安全子系统,它能控制程序只能访问特定文件, 有三种工作模式，可以自行设置.



## 远程登录到Linux服务器

> [官方下载地址（比较慢）](https://www.netsarang.com/en/free-for-home-school/)
>
> [Xshell7 和 Xftp7阿里云下载地址](https://www.aliyundrive.com/s/5KDWaC5kwSx)
>
> **两个软件的安装过程都是无脑冲！**

> 在登录之前先获取一下Linux虚拟机的IP地址`ifconfig`
>
> ![image-20220124211743811](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241816522.png)_ifconfig_
>
> 然后在windows这边`ping` 一下虚拟机的IP（正常来说都能ping通）

### 远程登陆 Linux-Xshell7

> Xshell 是目前最好的远程登录到 Linux 操作的软件，流畅的速度并且完美解决了中文乱码的问题， 是目前程序员首选的软件。

![image-20220124212011709](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241816007.png)

### 远程上传下载文件 Xftp7

> Xftp 是一个基于windows 平台的功能强大的 SFTP、FTP 文件传输软件。使用了 Xftp 以后，windows 用户能安全地在 UNIX/Linux 和 Windows PC 之间传输文件。

![image-20220124212220854](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241816574.png)

> Xftp连接上可能会出现乱码
>
> 属性->选项
>
> ![image-20220124212356724](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241816369.png)



## Vi 和 Vim编译器

> ​		Linux 系统会内置 vi 文本编辑器
> ​		Vim 具有程序编辑的能力，可以看做是 Vi 的增强版本，可以主动的以字体颜色辨别语法的正确性，方便程序设计。代码补完、编译及错误跳转等方便编程的功能特别丰富，在程序员中被广泛使用。

<img src="https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241816098.png" alt="image-20220124222635054" style="zoom:50%;" />_vim_

### vim 和 vim 常用的三种模式

#### 正常模式

​		以 vim 打开一个档案就直接进入一般模式了(这是默认的模式)。在这个模式中， 你可以使用『上下左右』按键来移动光标，你可以使用『删除字符』或『删除整行』来处理档案内容， 也可以使用『复制、粘贴』来处理你的文件数据。

#### 插入模式

​		按下` i, I, o, O, a, A, r, R `等任何一个字母之后才会进入编辑模式, 一般来说按 i 即可.

#### 命令行模式

​		输入 esc 再输入：在这个模式当中， 可以提供你相关指令，完成读取、存盘、替换、离开 vim 、显示行号等的动作则是在此模式中达成的！

#### 各个模式之间的切换

![image-20220124222908599](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241816801.png)

### vi 和 vim 快捷键

1. 拷贝当前行 `yy `, 拷贝当前行向下的 5 行 `5yy`，并粘贴（输入 p）。
2. 删除当前行 `dd`, 删除当前行向下的 5 行 `5dd`
3. 在文件中查找某个单词` [命令行下 /关键字 ， 回车 查找 ,输入 n 就是查找下一个 ]`
4. 设置文件的行号，取消文件的行号` [命令行下: :set nu 和:set nonu]`
5. 编辑 /etc/profile 文件，在一般模式下, 使用快捷键到该文档的`最末行[G]`和`最首行[gg]`
6. 在一个文件中输入 "hello" ,在一般模式下, `撤销这个动作 u`，`反撤销ctrl + r`
7. 编辑/etc/profile 文件，在一般模式下,并将光标移动到 , `输入 20,再输入 shift+g`

![image-20220124223208642](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241816820.png)_快捷键图_





## 开机、重启和用户登录注销

### 关机和重启

1. `shutdown –h now` 立刻进行关机
2. `shutdown -h 1` "hello, 1 分钟后会关机了"
3. `shutdown -r now` 现在重新启动计算机
4. `halt` 关机, 作用和上面一样
5. `reboot `现在重新启动计算机
6. `sync `把内存中的数据同步到磁盘

> <font color="#FF0000"><b>注意：</b></font>不管是重启系统还是关闭系统，首先要运行 `sync `命令，把内存中的数据写到磁盘中。

### 登录和注销

1. 登录时尽量少用 root 帐号登录，因为它是系统管理员，最大的权限，避免操作失误。可以利用普通用户登录，登录后再用`su - 用户名`命令来切换成系统管理员身份。
2. 在提示符下输入 `logout `即可注销用户。

> <font color="#FF0000"><b>注意：</b></font>`logout `注销指令在图形运行级别无效，在运行级别 3 下有效。



## 用户管理

> Linux 系统是一个多用户多任务的操作系统，任何一个要使用系统资源的用户，都必须首先向系统管理员申请一个账号，然后以这个账号的身份进入系统。

### 添加用户

基本语法：`useradd 用户名`

> 1.  当创建用户成功后，会自动的创建和用户同名的家目录
> 2. 可以通过 `useradd -d 指定目录 新的用户名`，给新创建的用户指定家目录

### 指定/修改密码

基本语法：`passwd 用户名`

> 显示当前用户所在的目录 `pwd`
>
> ![image-20220125132425392](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241816639.png)

### 删除用户

基本语法：`userdel 用户名` 

> 1. 删除用户tom，但是需要保留家目录 `userdel tom`
> 2. 删除用户tom及其家目录， `userdel -r tom`

### 查询用户信息

![image-20220125132726076](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241817249.png)

### 切换用户

基本语法：`su - 用户名`

> 1. 从权限高的用户切换到权限低的用户，不需要输入密码，反之需要。
> 2. 当需要返回到原来用户时，使用 `exit/logout` 指令

### 查看当前用户/登录用户

基本语法：`whoami / who am i`

![image-20220125132943581](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241817174.png)

### 用户组

> 类似于角色，系统可以对有共性/权限的多个用户进行统一的管理
>
> ![image-20220125134007106](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241817939.png)

#### 常用指令

```shell
新增组：groupadd 组名
删除组：groupdel 组名
修改组：usermod -g 用户组名 用户名
```

![image-20220125134333087](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241817142.png)_创建用户的时候指定组_

![image-20220125134512939](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241817075.png)_修改用户的组_

### 用户和组相关的文件

1. `/etc/passwd` 文件

   用户（user）的配置文件，记录用户的各种信息
   每行的含义：用户名:口令:用户标识号:组标识号:注释性描述:主目录:登录 Shell

   ![image-20220125134704309](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241817930.png)

2. `/etc/shadow` 文件

   口令的配置文件
   每行的含义：登录名:加密口令:最后一次修改时间:最小时间间隔:最大时间间隔:警告时间:不活动时间:失效时间:标志

   ![image-20220125134748715](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241817616.png)

3. `/etc/group` 文件

   组(group)的配置文件，记录 Linux 包含的组的信息
   每行含义：组名:口令:组标识号:组内用户列表

   ![image-20220125134850349](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241817021.png)



## 实用指令

### 指定运行级别

> 运行级别说明：
>
> * 0：关机
> * 1：单用户【找回丢失密码】
> * 2：多用户状态没有网络服务
> * **3：多用户状态有网络服务**
> * 4：系统未使用保留给用户
> * 5：图形界面
> * 6：系统重启
>
> 常用运行级别是 3 和 5 ，也可以指定默认运行级别。

#### 应用实例

命令：`init[0123456] `

#### 指定默认运行级别

> 在 centos7 以前， /etc/inittab 文件中 .
> 进行了简化 ，如下:
> `multi-user.target: analogous to runlevel 3`  级别3
> `graphical.target: analogous to runlevel 5` 级别5

```shell
systemctl get-default 查看默认运行级别
systemctl set-default TARGET.target 设置默认运行级别
```



### 找回root密码

> 查看这篇文章：[找回root密码](https://www.q/posts/9c94a23c/)



### 帮助指令

1. `man`获得帮助信息

   基本语法：`man [命令或配置文件]`（获得帮助信息）

   应用实例：查看 ls 命令的帮助信息 `man ls`

2. `help `指令

   基本语法：`help 命令`（获得 shell 内置命令的帮助信息）

   应用实例：查看 cd 命令的帮助信息 `help cd`



### 文件目录类

#### pwd 指令

> 显示当前工作目录的绝对路径

基本语法：`pwd `

应用实例：显示当前工作目录的绝对路径

![image-20220125210823786](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241817994.png)

#### ls 指令

>  显示文件

基本语法：`ls [选项] [目录或是文件]`

常用选项：

* -a：显示当前目录所有的文件和目录，包括隐藏的。
* -l：以列表的方式显示信息

应用实例：查看当前目录的所有内容信息

![image-20220125211116198](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241817855.png)

#### cd 指令

>  切换到指定目录

基本语法：`cd [参数]`
`cd ~ 或者 cd` ：回到自己的家目录, 比如 你是 root ， cd ~ 到 /root

![image-20220125211315635](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241817754.png)`cd ../ `回到当前目录的上一级目录

#### mkdir 指令

>  创建目录

基本语法：`mkdir [选项] 要创建的目录`

常用选项：

* -p：创建多级目录

应用实例：

1. 创建多级目录 `/home/animal/tiger`

   ```shell
   mkdir -p /home/animal/tiger
   ```

   

#### rmdir 指令

>  删除空目录

基本语法：`rmdir [选项] 要删除的空目录`

**使用细节：**rmdir 删除的是空目录，如果目录下有内容时无法删除的。

**提示：**如果需要删除非空目录，需要使用`rm -rf`要删除的目录比如： `rm -rf /home/animal`

#### touch 指令

> 创建空文件

基本语法：`touch 文件名称`

#### cp 指令

> 拷贝文件到指定目录

基本语法：`cp [选项] source dest`

常用选项：

* -r：递归复制整个文件夹

应用实例：

1. 将 /home/hello.txt 拷贝到/home/bbb 目录下

   ```shell
   cp hello.txt /home/bbb
   ```

2. 递归复制整个文件夹，举例, 比如将 /home/bbb 整个目录， 拷贝到 /opt

   ```shell
   cp -r /home/bbb /opt
   ```

使用细节：强制覆盖不提示的方法：`\cp`    `\cp -r /home/bbb /opt`



#### rm 指令

> 移除文件或目录

基本语法：`rm [选项] 要删除的文件或目录`

常用选项：

* -r ：递归删除整个文件夹
* -f ： 强制删除不提示



#### mv 指令

> 移动文件与目录或重命名

基本语法：

* `mv oldNameFile newNameFile` (重命名)
* `mv /temp/movefile /targetFolder` (移动文件)



#### cat 指令

> 查看文件内容

基本语法：`cat [选项] 要查看的文件`

常用选项：

* -n ：显示行号

应用实例：

1. 显示/etc/profile文件内容，并显示行号

   ```shell
   cat -n /etc/profile
   ```

   ![image-20220125212558047](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241819958.png)

使用细节：cat 只能浏览文件，而不能修改文件，为了浏览方便，一般会带上管道命令 | more



#### more 指令

> more 指令是一个基于 VI 编辑器的文本过滤器，它以全屏幕的方式按页显示文本文件的内容。more 指令中内置了若干快捷键(交互的指令)

基本语法：`more 要查看的文件`

操作说明 ：

![image-20220125212804537](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241820179.png)_more_



#### less 指令

> less 指令用来分屏查看文件内容，它的功能与 more 指令类似，但是比 more 指令更加强大，支持各种显示终端。less指令在显示文件内容时，并不是一次将整个文件加载之后才显示，而是根据显示需要加载内容，对于显示大型文件具有较高的效率。

基本语法：`less 要查看的文件`

操作说明 ：

![image-20220125212912899](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241820117.png)_less_

#### echo 指令

> 输出内容到控制台

基本语法：`echo [选项] [输出内容]`

应用实例：使用 echo 指令输出环境变量, 比如输出 $PATH $HOSTNAME

```shell
echo $HOSTNAME
```

![image-20220125213109837](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241820025.png)

#### head 指令

> 用于显示文件的开头部分内容，默认情况下 head 指令显示文件的前 10 行内容

基本语法：

1. `head 文件` (查看文件头 10 行内容)
2. `head -n 5 文件` (查看文件头 5 行内容，5 可以是任意行数)

应用实例：查看/etc/profile 的前面 5 行代码

```shell
head -n 5 /etc/profile
```

![image-20220125213934609](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241820573.png)

####  tail 指令

> 用于输出文件中尾部的内容，默认情况下 tail 指令显示文件的尾 10 行内容。

基本语法：

1. `tail 文件`（查看文件尾 10 行内容）
2. `tail -n 5 文件`（查看文件尾 5 行内容，5 可以是任意行数）
3. `tail -f 文件`（实时追踪该文档的所有更新）



#### > 指令 和 >> 指令

> `> `输出重定向
>
> ` >>` 追加

基本语法：

1. `ls -l > 文件`（列表的内容写入文件 a.txt 中（覆盖写））
2) `ls -al >> 文件` （列表的所有内容(包括隐藏)追加到文件 a.txt 的末尾）
3. `cat 文件 1 > 文件 2`（将文件 1 的内容覆盖到文件 2）
4) `echo "内容" >> 文件` (将内容追加进文件)

#### ln 指令

> 软链接也称为符号链接，类似于 windows 里的快捷方式，主要存放了链接其他文件的路径

基本语法：`ln -s [原文件或目录] [软链接名] `（给原文件创建一个软链接）

应用实例：在/home 目录下创建一个软连接 myroot，连接到 /root 目录

```shell
ln -s /root /home/myroot
```

![image-20220125214356801](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241820850.png)

#### history 指令

> 查看已经执行过历史命令,也可以执行历史指令

基本语法：`history `（查看已经执行过历史命令）



### 时间日期类

#### date 指令-显示当前日期

基本语法：

1. `date`（显示当前时间）
2. `date +%Y`（显示当前年份）
3. `date +%m`（显示当前月份）
4. `date +%d` （显示当前是哪一天）
5. `date "+%Y-%m-%d %H:%M:%S"`（显示年月日时分秒）

#### date 指令-设置日期

基本语法：`date -s 字符串时间`

#### cal 指令

> 查看日历指令

基本语法：

1. `cal [选项]`（不加选项，显示本月日历）

   ![image-20220125214711875](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241820236.png)

2. `cal 2022` （显示2022年历）

   ![image-20220125214807618](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241820338.png)

### 搜索查找类

#### find 指令

> find 指令将从指定目录向下递归地遍历其各个子目录，将满足条件的文件或者目录显示在终端。

基本语法：`find [搜索范围] [选项]`

选项说明：

![image-20220125214911536](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241820643.png)_find_

应用实例：

1. 按文件名：根据名称查找/home 目录下的 hello.txt 文件

   ```shell
   find /home -name hello.txt
   ```

2. 按拥有者：查找/opt 目录下，用户名称为 nobody 的文件

   ```shell
   find /opt -user nobody
   ```

3. 查找整个 linux 系统下大于 200M 的文件（+n 大于 -n 小于 n 等于, 单位有 k,M,G）

   ```shell
   find / -size +200M
   ```



#### locate 指令

> locate 指令可以快速定位文件路径。locate 指令利用事先建立的系统中所有文件名称及路径的 locate 数据库实现快速定位给定的文件。Locate 指令无需遍历整个文件系统，查询速度较快。为了保证查询结果的准确度，管理员必须定期更新 locate 时刻

基本语法：`locate 搜索文件`

<span><b style="color: #FF0000">特别说明：由于 locate 指令基于数据库进行查询，所以第一次运行前，必须使用 updatedb 指令创建 locate 数据库。</b></span>

#### grep 指令和 管道符号 |

> grep 过滤查找 ， 管道符，“|”，表示将前一个命令的处理结果输出传递给后面的命令处理。

基本语法：`grep [选项] 查找内容 源文件`

常用选项：

![image-20220125215326293](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241820192.png)_grep_

应用实例：请在 hello.txt 文件中，查找"yes"所在行，并且显示行号

```shell
写法 1：cat /home/hello.txt | grep"yes"
写法 2：grep -n "yes" /home/hello.txt
```



### 压缩和解压类

#### gzip/gunzip 指令

> gzip 用于压缩文件， gunzip 用于解压的

基本语法：

1. `gzip 文件`（功能描述：压缩文件，只能将文件压缩为*.gz 文件）
2. `gunzip 文件.gz `（功能描述：解压缩文件命令）

应用实例：

1. gzip 压缩， 将 /home 下的 hello.txt 文件进行压缩

   ```shell
   gzip /home/hello.txt
   ```

2. gunzip 压缩， 将 /home 下的 hello.txt.gz 文件进行解压缩

   ```shell
   gunzip /home/hello.txt.gz
   ```

#### zip/unzip 指令

> zip 用于压缩文件
>
> unzip 用于解压

基本语法：

1. `zip [选项] XXX.zip 将要压缩的内容`（功能描述：压缩文件和目录的命令）
2. `unzip [选项] XXX.zip`（功能描述：解压缩文件）

zip 常用选项：

* -r：递归压缩，即压缩目录

unzip 的常用选项：

* -d<目录> ：指定解压后文件的存放目录

应用实例：

1. 将 /home 下的 所有文件/文件夹进行压缩成 myhome.zip

   ```shell
   zip -r myhome.zip /home/ [将 home 目录及其包含的文件和子文件夹都压缩]
   ```

   

2. 将 myhome.zip 解压到 /opt/tmp 目录下

   ```shell
   mkdir /opt/tmp
   unzip -d /opt/tmp /home/myhome.zip
   ```

   

#### tar 指令

> tar 指令是打包指令，最后打包后的文件是 .tar.gz 的文件。

基本语法：`tar [选项] XXX.tar.gz 打包的内容` (功能描述：打包目录，压缩后的文件格式.tar.gz)

常用选项：

![image-20220125220025876](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241820475.png)_tar_

应用实例：

1. 压缩多个文件，将 /home/pig.txt 和 /home/cat.txt 压缩成 pc.tar.gz

   ```shell
   tar -zcvf pc.tar.gz /home/pig.txt /home/cat.txt
   ```

2. 将/home 的文件夹 压缩成 myhome.tar.gz

   ```shell
   tar -zcvf myhome.tar.gz /home/
   ```

3. 将 pc.tar.gz解压到当前目录

   ```shell
   tar -zxvf pc.tar.gz
   ```

4. 将 myhome.tar.gz解压到 /opt/tmp2目录下

   ```shell
   mkdir /opt/tmp2
   tar -zxvf /home/myhome.tar.gz -C /opt/tmp2
   ```





## 组管理和权限管理

### 组的基本介绍

> 在 linux 中的每个用户必须属于一个组，不能独立于组外。
>
> 在 linux 中每个文件有所有者、所在组、其它组的概念。

![image-20220127212346536](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241820382.png)_一个文件对应的关系_



### 文件/目录 所有者

> 一般为文件的创建者，谁创建了该文件，就自然的成为该文件的所有者。

#### 查看文件所有者

![image-20220127212550852](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241820576.png)_ll_

#### 修改文件所有者

基本语法：`chown 用户名 文件名`

![image-20220127212755551](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241820170.png)_chown tom abc_

参数：

* <span><b style="color: #FF0000">-R  如果是目录 则使其下所有子文件或目录递归生效</b></span>



### 组的创建

基本语法：`groupadd 组名`

应用实例：创建一个用户 fox ，并放入到 monster 组中

```shell
groupadd monster
useradd -g monster fox
```

![image-20220127213044339](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241820779.png)

### 文件/目录 所在组

> 当某个用户创建了一个文件后，这个文件的所在组就是该用户所在的组(默认)。

#### 查看文件/目录所在组

![image-20220127213349021](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241820456.png)_ll_

#### 修改文件/目录所在组

基本语法：`chgrp 组名 文件名/目录`

应用实例：使用 root 用户创建文件 orange.txt ,看看当前这个文件属于哪个组，然后将这个文件所在组，修改到 fruit 组。

```shell
groupadd fruit
touch orange.txt
chgrp fruit orange.txt
```

![image-20220127213616857](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241821840.png)_root_

![image-20220127213733933](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241821918.png)_fruit_

### 改变用户所在的组

1. `usermod -g 新组名 用户名`
2. `usermod -d 目录名 用户名` 改变该用户登陆的初始目录。<span><b style="color: #FF0000">特别说明：用户需要有进入到新目录的权限。</b></span>

### 权限的基本介绍

`ll` 后显示的内容如下：

![image-20220127214214560](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241821447.png)

* 第 0 位确定文件类型`(d, -, l, c, b)`
  * <span><b style="color: #55ff55">l 是链接，相当于 windows 的快捷方式</b></span>
  * <span><b style="color: #55ff55">d 是目录，相当于 windows 的文件夹</b></span>
  * <span><b style="color: #55ff55">c 是字符设备文件，鼠标，键盘</b></span>
  * <span><b style="color: #55ff55">b 是块设备，比如硬盘</b></span>
* 第 1-3 位确定<b>所有者</b>（该文件的所有者）拥有该文件的权限。---User
* 第 4-6 位确定<b>所属组</b>（同用户组的）拥有该文件的权限，---Group
* 第 7-9 位确定<b>其他用户</b>拥有该文件的权限 ---Other




### rwx 权限

#### rwx 作用到文件

1. [ r ]代表可读(read): 可以读取,查看
2. [ w ]代表可写(write): 可以修改,但是不代表可以删除该文件,删除一个文件的前提条件是对该文件所在的目录有写权限，才能删除该文件.
3. [ x ]代表可执行(execute):可以被执行



#### rwx 作用到目录

1. [ r ]代表可读(read): 可以读取，ls 查看目录内容
2. [ w ]代表可写(write): 可以修改, 对目录内创建+删除+重命名目录
3. [ x ]代表可执行(execute):可以进入该目录



### 修改权限-chmod

> 通过 `chmod` 指令，可以修改**文件或者目录**的权限。

#### + - = 变更权限

> u:所有者
> g:所有组
> o:其他人
> a:所有人(u、g、o 的总和)

应用实例：

1. 给 abc 文件 的所有者读写执行的权限，给所在组读执行权限，给其它组读执行权限。

   ```shell
   chmod u=rwx,g=rx,o=rx abc
   ```

2. 给 abc 文件的所有者除去执行的权限，增加组写的权限

   ```shell
   chmod u-x,g+w abc
   ```

3. 给 abc 文件的所有用户添加读的权限

   ```shell
   chmod a+r abc
   ```

   

####  通过数字变更权限

> `r=4 w=2 x=1   rwx=4+2+1=7`

应用实例：将 /home/abc.txt 文件的权限修改成rwxr-xr-x, 使用给数字的方式实现

```shell
chmod 755 /home/abc.txt
```





## 定时任务调度

### crond 任务调度

> 任务调度：是指系统在某个时间执行的特定的命令或程序。
> 任务调度分类：
>
> 1. 系统工作：有些重要的工作必须周而复始地执行。如病毒扫描等
> 2. 个别用户工作：个别用户可能希望执行某些程序，比如对 mysql 数据库的备份。
>
> ![image-20220128203532411](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241821970.png)_任务调度示意图_

#### 基本用法

`crontab [选项]`

#### 常用选项

`service crond restart`：重启任务调度

![image-20220128203648706](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241821732.png)_crontan常用选项_

#### 参数细节

* 5个占位符的说明

  ![image-20220128203849088](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241821158.png)

* 特殊符号的说明

  ![image-20220128203903311](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241821820.png)

  

#### 应用实例

1. 每隔 1 分钟，就将当前的日期信息，追加到 /tmp/mydate 文件中

   ```shell
   */1 * * * * date >> /tmp/mydate
   ```

2. 每隔 1 分钟， 将当前日期和日历都追加到 /home/mycal 文件中

   ```shell
   (1) vim /home/my.sh  写入内容 date >> /home/mycal 和 cal >> /home/mycal
   (2) 给 my.sh 增加执行权限，chmod u+x /home/my.sh
   (3) crontab -e  增加 */1 * * * * /home/my.sh
   ```

3. 每天凌晨 2:00 将 mysql 数据库 testdb ，备份到文件中。

   ```shell
   (1) crontab -e
   (2) 0 2 * * * mysqldump -u root -proot testdb > /home/db.bak
   ```

   



### at 定时任务

> 1. at 命令是一次性定时计划任务，at 的守护进程 atd 会以后台模式运行，检查作业队列来运行。
>
> 2. 默认情况下，atd 守护进程每 60 秒检查作业队列，有作业时，会检查作业运行时间，如果时间与当前时间匹配，则运行此作业。
>
> 3. at 命令是一次性定时计划任务，执行完一个任务后不再执行此任务了
>
> 4. 在使用 at 命令的时候，一定要保证 atd 进程的启动 , 可以使用相关指令来查看 `ps -ef`
>    `| grep atd`   可以检测 atd 是否在运行
>
>    ![image-20220128204542062](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241821170.png)
>
> ![image-20220128204458236](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241821639.png)

#### 基本用法

`at [选项] [时间]`

<b style="color: #FF0000">Ctrl + D 结束 at 命令的输入, 输出两次</b>

#### 命令选项

![image-20220128204736332](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241821721.png)

#### at 时间定义

1. 接受在当天的 hh:mm（小时:分钟）式的时间指定。假如该时间已过去，那么就放在第二天执行。 例如：04:00
2. 使用 midnight（深夜），noon（中午），teatime（饮茶时间，一般是下午 4 点）等比较模糊的词语来指定时间。
3. 采用 12 小时计时制，即在时间后面加上 AM（上午）或 PM（下午）来说明是上午还是下午。 例如：12pm
4. 指定命令执行的具体日期，指定格式为 month day（月 日）或 mm/dd/yy（月/日/年）或 dd.mm.yy（日.月.年），指定的日期必须跟在指定时间的后面。 例如：04:00 2021-03-1
5. 使用相对计时法。指定格式为：now + count time-units ，now 就是当前时间，time-units 是时间单位，这里能够是 minutes（分钟）、hours（小时）、days（天）、weeks（星期）。count 是时间的数量，几天，几小时。 例如：now + 5 minutes
6. 直接使用 today（今天）、tomorrow（明天）来指定完成命令的时间。

#### 应用实例

1. 2 天后的下午 5 点执行 /bin/ls /home

   ![image-20220128204949188](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241821854.png)

2. 明天 17 点钟，输出时间到指定文件内 比如 /root/date100.log

   ![image-20220128205047806](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241821338.png)

3. 2 分钟后，输出时间到指定文件内 比如 /root/date200.log

   ![image-20220128205133664](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241821799.png)

4. 删除已经设置的任务 , atrm 编号

   ```shell
   atrm 4 //表示将 job 队列，编号为 4 的 job 删除.
   ```

   



## Linux磁盘分区、挂载

### Linux分区

1. Linux 来说无论有几个分区，分给哪一目录使用，它归根结底就只有一个根目录，一个独立且唯一的文件结构 , Linux中每个分区都是用来组成整个文件系统的一部分。
2. Linux 采用了一种叫“载入”的处理方法，它的整个文件系统中包含了一整套的文件和目录，且将一个分区和一个目录联系起来。这时要载入的一个分区将使它的存储空间在一个目录下获得
3. ![image-20220130210351865](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241821413.png)_分区示意图_



#### 硬盘说明

1. Linux 硬盘分 IDE 硬盘和 SCSI 硬盘，目前基本上是 SCSI 硬盘。

2. 对于 IDE 硬盘，驱动器标识符为“hdx~”,其中“hd”表明分区所在设备的类型，这里是指 IDE 硬盘了。“x”为盘号（a 为基本盘，b 为基本从属盘，c 为辅助主盘，d 为辅助从属盘）,“~”代表分区，前四个分区用数字 1 到 4 表示，它们是主分区或扩展分区，从 5 开始就是逻辑分区。例，hda3 表示为第一个 IDE 硬盘上的第三个主分区或扩展分区,hdb2表示为第二个 IDE 硬盘上的第二个主分区或扩展分区。

3. 对于 SCSI 硬盘则标识为“sdx~”，SCSI 硬盘是用“sd”来表示分区所在设备的类型的，其余则和 IDE 硬盘的表示方法一样。

   ![image-20220130210508183](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241822377.png)

#### 查看所有分区挂在情况

基本语法：`lsblk `或者 `lsblk -f`

![image-20220130210818145](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241822069.png)



### 挂载的经典案例

> 增加硬盘的5个步骤
>
> 1. 虚拟机添加硬盘
> 2. 分区
> 3. 格式化
> 4. 挂载
> 5. 设置可以自动挂载

#### 虚拟机添加硬盘

> 在【虚拟机】菜单中，选择【设置】，然后设备列表里添加硬盘，然后一路【下一步】，中间只有选择磁盘大小的地方需要修改，至到完成。<b style="color: #FF0000">然后重启系统才能识别！</b>

![image-20220130211124539](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241822898.png)

#### 分区

分区命令：`fdisk /dev/sdb`

开始对/sdb 分区
m&emsp;&emsp;显示命令列表
p&emsp;&emsp;显示磁盘分区 同 `fdisk –l`
n&emsp;&emsp;新增分区
d&emsp;&emsp;删除分区
w&emsp;&emsp;写入并退出

<b style="color: #FF0000">注意：开始分区后输入 n，新增分区，然后选择 p ，分区类型为主分区。两次回车默认剩余全部空间。最后输入 w
写入分区并退出，若不保存退出输入 q。</b>

![image-20220130211511638](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241822310.png)

#### 格式化

分区命令：`mkfs -t ext4 /dev/sdb1`
其中 ext4 是分区类型



#### 挂载

> 挂载: 将一个分区与一个目录联系起来

基本语法：

1. 挂载：`mount 设备名称 挂载目录 `

   例如： `mount /dev/sdb1 /newdisk`

2. 取消挂载：`umount 设备名称 或者 挂载目录`

   例如：`umount /dev/sdb1 或者 umount /newdisk`

<b style="color: #FF0000">注意：使用命令行挂载，重启后会失效</b>

#### 设置自动挂载（永久挂载）

通过修改/etc/fstab 实现挂载

![image-20220130212008391](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241822282.png)_vim /etc/fstab_



### 磁盘情况查询

#### 查询系统整体磁盘使用情况

基本语法：`df -h`

应用实例：查询系统整体磁盘使用情况

![image-20220130212140686](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241822375.png)

#### 查询指定目录的磁盘占用情况

基本语法：`du -h`

可选参数：

|     选项      |            功能            |
| :-----------: | :------------------------: |
|      -s       |    指定目录占用大小汇总    |
|      -h       |         带计量单位         |
|      -a       |           含文件           |
| --max-depth=1 |         子目录深度         |
|      -c       | 列出明细的同时，增加汇总值 |

应用实例：查询 /opt 目录的磁盘占用情况，深度为 1

![image-20220130212323543](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241822531.png)

### 工作实用指令

1. 统计/opt 文件夹下文件的个数 

   `ls -l /opt | grep "^-" | wc -l`

2. 统计/opt 文件夹下目录的个数 

   `ls -l /opt | grep "^d" | wc -l`

3. 统计/opt 文件夹下文件的个数，包括子文件夹里的

   `ls -lR /opt | grep "^-" | wc -l`

4. 统计/opt 文件夹下目录的个数，包括子文件夹里的

   `ls -lR /opt | grep "^d" | wc -l`

5. 以树状显示目录结构 tree 目录 ， 注意，如果没有 tree ,则使用 `yum install tree` 安装

![image-20220130212433360](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241822159.png)



## 网络配置

![image-20220206191613738](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241822130.png)_网络配置原理图_

### 查看网络 IP 和网关

#### 查看虚拟网络编辑器和修改 IP 地址

> 虚拟机客户端->编辑->虚拟网络编译器

![image-20220206192724864](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241822311.png)

#### 查看网关

![image-20220206192846589](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241822081.png)

### 查看 windows 环境的中 VMnet8 网络配置 (ipconfig 指令)

![image-20220206192929185](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241822851.png)

### 查看 linux 的网络配置 ifconfig

![image-20220206193004027](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241822267.png)

### Linux 网络环境配置

#### 第一种方法(自动获取)

<blockquote class="success">
    登陆后，通过界面的来设置自动获取 ip，特点：linux 启动后会自动获取 IP,缺点是每次自动获取的 ip 地址可能不一样
</blockquote>



![image-20220206193118298](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241822959.png)

#### 第二种方法(指定 ip)

<blockquote class="success">
    直接修改配置文件来指定 IP,并可以连接到外网(程序员推荐)
</blockquote>

##### 操作步骤

1. 编辑 `/etc/sysconfig/network-scripts/ifcfg-ens33`这个文件

2. 添加如下内容

   ![image-20220206193447458](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241822457.png)

   ```shell
   BOOTPROTO=static
   #IP 地址
   IPADDR=192.168.200.130
   #网关
   GATEWAY=192.168.200.2
   #域名解析器
   DNS1=192.168.200.2
   ```

3. 修改虚拟网络配置的这两个地方

   ![image-20220224151405967](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241822812.png)

4. 重启网络服务或者重启系统生效

   ```shell
   service network restart
   或者
   reboot
   ```

   



### 设置主机名和 hosts 映射

#### 设置主机名

1. 为了方便记忆，可以给 <b style="color: skyblue">linux 系统设置主机名</b> , 也可以根据需要修改主机名

2. 指令 `hostname` ： 查看主机名

   ![image-20220207194544989](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241822136.png)

3. 修改文件在 `/etc/hostname` 指定

4. 修改后，<b style="color: skyblue">重启</b>生效



#### 设置 hosts 映射

##### Windows

在`C:\Windows\System32\drivers\etc\hosts` 文件指定即可

![image-20220206194342395](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241822471.png)

<b style="color: #FF0000">注意：如果保存的时候显示没有权限，可以先将文件拖到桌面上修改，修改完成之后再拖回去替换文件</b>

##### Linux

在 `/etc/hosts` 文件 指定

![image-20220206194514968](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241823254.png)

### 主机名解析过程分析(Hosts、DNS)

#### Host

一个文本文件，用来记录 **IP 和 Hostname(主机名)**的映射关系

#### DNS

**DNS**，就是 **Domain Name System** 的缩写，翻译过来就是域名系统
是互联网上作为域名和 IP 地址相互映射的一个**分布式数据库**



#### 应用实例

> 用户在浏览器输入了 www.baidu.com

1. 浏览器先检查浏览器缓存中有没有该域名解析 IP 地址，有就先调用这个 IP 完成解析；如果没有，就检查 DNS 解析器缓存，如果有直接返回 IP 完成解析。这两个缓存可以理解为 本地解析器缓存。

2. 一般来说，当电脑第一次成功访问某一网站后，在一定时间内，浏览器或操作系统会缓存他的 IP 地址（DNS 解析记录）。如 在 cmd 窗口中输入

   `ipconfig /displaydns`&emsp;&emsp;// DNS 域名解析缓存
   `ipconfig /flushdns`&emsp;&emsp;// 手动清理 dns 缓存

3. 如果本地解析器缓存没有找到对应映射，检查系统中 hosts 文件中有没有配置对应的域名 IP 映射，如果有，则完成解析并返回。

4. 如果 本地 DNS 解析器缓存 和 hosts 文件 中均没有找到对应的 IP，则到域名服务 DNS 进行解析域

![image-20220206194952032](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241823087.png)



## 进程管理(重点)

> 1. 在 LINUX 中，每个执行的程序都称为一个进程。每一个进程都分配一个 ID 号(pid,进程号)。=>windows => linux
> 2. 每个进程都可能以两种方式存在的。前台与后台，所谓前台进程就是用户目前的屏幕上可以进行操作的。后台进程则是实际在操作，但由于屏幕上无法看到的进程，通常使用后台方式执行。
> 3. 一般系统的服务都是以后台进程的方式存在，而且都会常驻在系统中。直到关机才才结束。
> 4. ![image-20220212132159697](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241823104.png)_任务管理器_

### 显示系统执行的进程

> ps 命令是用来查看目前系统中，有哪些正在执行，以及它们执行的状况。可以不加任何参数。
>
> ![image-20220212132757316](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241823613.png)_ps - aux_

#### ps指令的使用

##### 基本用法

基本语法：`ps –aux | grep xxx`

![image-20220212132926540](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241823684.png)_查看sshd的执行状态_

指令说明：

|   表头   |                             含义                             |
| :------: | :----------------------------------------------------------: |
| System V |                           展示风格                           |
|   USER   |                           用户名称                           |
|   PID    |                            进程号                            |
|   %CPU   |                     进程占用CPU的百分比                      |
|   %MEM   |                   进程占用物理内存的百分比                   |
|   VSZ    |                 进程占用虚拟内存的大小（KB）                 |
|   RSS    |                 进程占用的物理内存大小（KB）                 |
|   TTY    |                        终端名称的缩写                        |
|   STAT   | 进程状态，其中 S-睡眠，s-表示该进程是会话的先导进程<br/>N-表示进程拥有比普通优先级更低的优先级，R-正在运行<br/>D-短期等待，Z-僵死进程，T-被跟踪或者被停止等等 |
| STARTED  |                        进程的启动时间                        |
|   TIME   |                CPU时间，即进程使用CPU的总时间                |
| COMMAND  |        启动进程所用的命令和参数，如果过长会被截断显示        |

##### 其他用法

基本语法：`ps -ef | grep xxx`

> ps -ef 是以全格式显示当前所有的进程
>
> | 选项 |     功能     |
> | :--: | :----------: |
> | `-e` | 显示所有进程 |
> | `-f` |    全格式    |

![image-20220212134008155](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241823424.png)_查看sshd的父进程信息_

![image-20220212134109539](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241823955.png)_全部进程_

指令说明：

| 表头  |                             含义                             |
| :---: | :----------------------------------------------------------: |
|  UID  |                            用户ID                            |
|  PID  |                            进程ID                            |
| PPID  |                           父进程ID                           |
|   C   | CPU 用于计算执行优先级的因子。<br/>数值越大，表明进程是 CPU 密集型运算，执行优先级会降低<br>数值越小，表明进程是 I/O 密集型运算，执行优先级会提高 |
| STIME |                        进程启动的时间                        |
|  TTY  |                        完整的终端名称                        |
| TIME  |                           CPU 时间                           |
|  CMD  |                   启动进程所用的命令和参数                   |



### 终止进程kill和killall

> 若是某个进程执行一半需要停止时，或是已消了很大的系统资源时，此时可以考虑停止该进程。使用 kill 命令来完成此项任务。

#### 基本语法

`kill [选项] 进程号`（功能描述：通过进程号杀死/终止进程）

`killall 进程名称` （功能描述：通过进程名称杀死进程，也支持通配符，这在系统因负载过大而变得很慢时很有用）

#### 常用选项

`-9`：表示强迫进程立即停止

#### 应用实例

1. 踢掉某个非法登录用户

   ```shell
   kill 进程号 , 比如 kill 11421
   ```

2. 终止远程登录服务 sshd, 在适当时候再次重启 sshd 服务

   ```shell
   kill sshd 对应的进程号; 
   systemctl start sshd.service
   ```

3. 终止多个 gedit

   ```shell
   killall gedit
   ```

4. 强制杀掉一个终端

   ```shell
   kill -9 bash 对应的进程号
   ```



### 查看进程树pstree

#### 基本语法

`pstree [选项]` ，可以更加直观的来看进程信息

![image-20220212134807561](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241823620.png)

#### 常用选项

| 选项 |        功能        |
| :--: | :----------------: |
| `-p` |   显示进程的 PID   |
| `-u` | 显示进程的所属用户 |

#### 应用实例

1. 以树状的形式显示进程的 pid

   ```shell
   pstree -p
   ```

   ![image-20220212134946235](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241823249.png)

2. 以树状的形式进程的用户

   ```shell
   pstree -u
   ```

   ![image-20220212135009446](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241823818.png)



### 服务(service)管理

> 服务(service) 本质就是进程，但是是运行在后台的，通常都会监听某个端口，等待其它程序的请求，比如(mysqld ,，sshd，防火墙等)，因此我们又称为守护进程。

#### service 管理指令

1. `service 服务名 [start | stop | restart | reload | status]`

2. 在 CentOS7.0 后 很多服务不再使用 service ,而是 <b style="color: #FF0000">systemctl</b>

3. service 指令管理的服务在 /etc/init.d 查看

   ![image-20220212135400523](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241823400.png)

#### service 应用实例

查看，关闭，启动 network

```shell
service network status
service network stop
service network start
```

#### 查看服务名

1. 使用 setup -> 系统服务 就可以看到全部

   ![image-20220212135544790](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241823279.png)

2. /etc/init.d 看到 service 指令管理的服务



#### chkconfig 指令

> 通过 chkconfig 命令可以给服务的<b style="color: #FF0000">各个运行级别</b>设置自启动/关闭
> chkconfig 指令管理的服务在 /etc/init.d 查看

##### 基本语法

1. `查看服务 chkconfig --list [| grep xxx]`
2. `chkconfig 服务名 --list`
3. `chkconfig --level 5 服务名 on/off`

##### 应用实例

把 network 在 3 运行级别,关闭自启动

```shell
chkconfig --level 3 network off
chkconfig --level 3 network on
```

<b style="color: #FF0000">chkconfig 重新设置服务后自启动或关闭，需要重启机器 reboot 生效.</b>

### systemctl 管理指令（重要）

#### 基本语法

`systemctl [start | stop | restart | status] 服务名`

systemctl 指令管理的服务在 `/usr/lib/systemd/system` 查看

![image-20220212140057121](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241823134.png)

#### systemctl 设置服务的自启动状态

```shell
systemctl list-unit-files [ | grep 服务名] (查看服务开机启动状态, grep 可以进行过滤)
systemctl enable 服务名 (设置服务开机启动)
systemctl disable 服务名 (关闭服务开机启动)
systemctl is-enabled 服务名 (查询某个服务是否是自启动的)
```



#### 应用实例

查看当前防火墙的状况，关闭防火墙和重启防火墙。=> firewalld.service（防火墙服务的名字）

```shell
systemctl status firewalld; 
systemctl stop firewalld; 
systemctl start firewalld
```

<blockquote class="danger">
    关闭或者启用防火墙后，立即生效。[telnet 测试某个端口即可] <br>
	这种方式只是临时生效，当重启系统后，还是回归以前对服务的设置。<br>
	如果希望设置某个服务自启动或关闭永久生效，要使用 systemctl [enable|disable] 服务名
</blockquote>



### 打开或者关闭指定端口

![image-20220212140315628](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241823648.png)

#### firewall 指令

```shell
(1)打开端口：firewall-cmd --permanent --add-port=端口号/协议
(2)关闭端口：firewall-cmd --permanent --remove-port=端口号/协议
(3)重新载入,才能生效 : firewall-cmd --reload
(4)查询端口是否开放:firewall-cmd --query-port=端口/协议
```

#### 应用实例

> 开放linux的111端口共windows使用

```shell
(1)启用防火墙的时候， 测试 111 端口是否能 telnet , 不行
(2)开放 111 端口 firewall-cmd --permanent --add-port=111/tcp; 需要 firewall-cmd --reload
(3)再次关闭 111 端口 firewall-cmd --permanent --remove-port=111/tcp; 需要 firewall-cmd --reload
```



### 动态监控进程

> top 与 ps 命令很相似。它们都用来显示正在执行的进程。
>
> Top 与 ps 最大的不同之处，在于 top 在执行一段时间可以更新正在运行的的进程。

#### 基本语法

`top [选项]`

![image-20220212141244287](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241823891.png)

#### 选项说明

|   选项    |                    功能                    |
| :-------: | :----------------------------------------: |
| `-d 秒数` |     指定top命令每隔几秒更新。默认是3秒     |
|   `-i`    |      使top不显示任何闲置或者僵死进程       |
|   `-p`    | 通过指定监控进程ID来仅仅监控某个进程的状态 |

#### 交互操作说明

| 操作 |                功能                 |
| :--: | :---------------------------------: |
|  P   | 以CPU的使用率进行排序，默认就是此项 |
|  M   |         以内存的使用率排序          |
|  N   |              以PID排序              |
|  q   |               退出top               |



#### 应用实例

1. 监视特定用户, 比如我们监控 tom 用户

   ```shell
   top：输入此命令，按回车键，查看执行的进程。
   u：然后输入“u”回车，再输入用户名，即可
   ```

2. 终止指定的进程, 比如我们要结束 tom 登录

   ```shell
   top：输入此命令，按回车键，查看执行的进程。
   k：然后输入“k”回车，再输入要结束的进程 ID 号
   ```

3. 指定系统状态更新的时间(每隔 10 秒自动更新), 默认是 3 秒

   ```shell
   top -d 10
   ```

   

### 监控网络状态

#### 基本语法

`netstat [选项]`



#### 常用选项

| 选项 |         功能         |
| :--: | :------------------: |
| -an  | 按一定的顺序排列输出 |
|  -p  |  显示哪个进程在调用  |



#### 应用实例

请查看服务名为 sshd 的服务的信息。

```shell
netstat -anp | grep sshd
```

![image-20220212142259217](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241823495.png)

![image-20220212142309940](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241823964.png)_内部地址和外部地址的关系_





## RPM 与 YUM

### rpm 包管理

> rpm 用于互联网下载包的打包及安装工具，它包含在某些 Linux 分发版中。它生成具有.RPM 扩展名的文件。RPM 是 RedHat Package Manager（RedHat 软件包管理工具）的缩写，类似 windows 的 setup.exe，这一文件格式名称虽然打上了 RedHat 的标志，但理念是通用的。
> Linux 的分发版本都有采用（suse,redhat, centos 等等），可以算是公认的行业标准了。

#### rpm 包的简单查询指令

查询已安装的 rpm 列表 `rpm -qa | grep xx`

![image-20220214145625881](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241823113.png)_rpm -qa | grep firefox_

#### rpm 包名基本格式

一个 rpm 包名：`firefox-60.2.2-1.el7.centos.x86_64`

名称: `firefox`
版本号：`60.2.2-1`
适用操作系统: `el7.centos.x86_64`
表示 `centos7.x` 的 64 位系统
如果是 `i686、i386` 表示 32 位系统，`noarch` 表示通用



#### rpm 包的其它查询指令

