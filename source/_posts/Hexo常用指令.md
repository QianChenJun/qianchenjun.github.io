---
title: Hexo常用指令
author: 小小千辰
tags:
  - Hexo
  - 命令行
categories:
  - 千辰的小小笔记
abbrlink: a5b8d333
date: 2021-09-19 15:27:24
updated: 2021-09-19 15:27:24
---

### 1、hexo init

`hexo init` 命令用于初始化本地文件夹为网站的根目录

```
$ hexo init [folder]
```

- `folder` 可选参数，用以指定初始化目录的路径，若无指定则默认为当前目录

### 2、hexo new

`hexo new` 命令用于新建文章，一般可以简写为 `hexo n`

```
$ hexo new [layout] <title>
```

- `layout` 可选参数，用以指定文章类型，若无指定则默认由配置文件中的 default_layout 选项决定
- `title` 必填参数，用以指定文章标题，如果参数值中含有空格，则需要使用双引号包围

### 3、hexo generate

`hexo generate` 命令用于生成静态文件，一般可以简写为 `hexo g`

```
$ hexo generate
```

- `-d` 选项，指定生成后部署，与 `hexo d -g` 等价

详细信息请参考：https://hexo.io/docs/generating

### 4、hexo server

`hexo server` 命令用于启动本地服务器，一般可以简写为 `hexo s`

```
$ hexo server
```

- `-p` 选项，指定服务器端口，默认为 4000
- `-i` 选项，指定服务器 IP 地址，默认为 0.0.0.0
- `-s` 选项，静态模式 ，仅提供 public 文件夹中的文件并禁用文件监视

**说明** ：运行服务器前需要安装 hexo-server 插件

```
$ npm install hexo-server --save
```

详细信息请参考：https://hexo.io/docs/server.html

### 5、hexo deploy

`hexo deploy` 命令用于部署网站，一般可以简写为 `hexo d`

```
$ hexo deploy
```

- `-g` 选项，指定生成后部署，与 `hexo g -d` 等价

**说明** ：部署前需要修改 _config.yml 配置文件，下面以 git 为例进行说明

```
deploy:
	type: git
	repo: <repository url>
	branch:	master
	message: 自定义提交消息，默认为Site updated: {{ now('YYYY-MM-DD HH:mm:ss') }}
```

详细信息请参考：https://hexo.io/docs/deployment.html

### 6、hexo clean

`hexo clean` 命令用于清理缓存文件，是一个比较常用的命令

```
$ hexo clean
```

**网站显示异常时可尝试此操作**

### 7、Option

#### （1）hexo --safe

`hexo --safe` 表示安全模式，用于禁用加载插件和脚本

```
$ hexo --safe
```

**安装新插件时遇到问题可尝试此操作**

#### （2）hexo --debug

`hexo --debug` 表示调试模式，用于将消息详细记录到终端和 `debug.log` 文件

```
$ hexo --debug
```

#### （3）hexo --silent

`hexo --silent` 表示静默模式，用于静默输出到终端

```
$ hexo --silent
```
