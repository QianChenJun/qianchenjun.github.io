---
title: 使用不蒜子出现的问题
abbrlink: aa536622
date: 2022-02-09 19:42:30
update: 2022-02-09 19:42:30
tags:
  - hexo
  - 不蒜子
categories:
  - 千辰的小小笔记
  - 转载
---

## 不蒜子和live2d同时使用出现的问题

> 当二者同时使用的时候，不蒜子的统计会莫名其妙的添加上display：none的属性
>
> ![image-20220209194543209](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241800794.png)
>
> 这是个好久之前的bug，目前仍没有好的解决方案。

### 解决方法

我的资源目录：`\themes\yun\source\js\others\busuanzi.pure.mini.js`

![image-20220209194936778](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241800856.png)

```js
var bszCaller,bszTag;!function(){var c,d,e,a=!1,b=[];ready=function(c){return a||"interactive"===document.readyState||"complete"===document.readyState?c.call(document):b.push(function(){return c.call(this)}),this},d=function(){for(var a=0,c=b.length;c>a;a++)b[a].apply(document);b=[]},e=function(){a||(a=!0,d.call(window),document.removeEventListener?document.removeEventListener("DOMContentLoaded",e,!1):document.attachEvent&&(document.detachEvent("onreadystatechange",e),window==window.top&&(clearInterval(c),c=null)))},document.addEventListener?document.addEventListener("DOMContentLoaded",e,!1):document.attachEvent&&(document.attachEvent("onreadystatechange",function(){/loaded|complete/.test(document.readyState)&&e()}),window==window.top&&(c=setInterval(function(){try{a||document.documentElement.doScroll("left")}catch(b){return}e()},5)))}(),bszCaller={fetch:function(a,b){var c="BusuanziCallback_"+Math.floor(1099511627776*Math.random());window[c]=this.evalCall(b),a=a.replace("=BusuanziCallback","="+c),scriptTag=document.createElement("SCRIPT"),scriptTag.type="text/javascript",scriptTag.defer=!0,scriptTag.src=a,document.getElementsByTagName("HEAD")[0].appendChild(scriptTag)},evalCall:function(a){return function(b){ready(function(){try{a(b),scriptTag.parentElement.removeChild(scriptTag)}catch(c){bszTag.hides()}})}}},bszCaller.fetch("//busuanzi.ibruce.info/busuanzi?jsonpCallback=BusuanziCallback",function(a){bszTag.texts(a),bszTag.shows()}),bszTag={bszs:["site_pv","page_pv","site_uv"],texts:function(a){this.bszs.map(function(b){var c=document.getElementById("busuanzi_value_"+b);c&&(c.innerHTML=a[b])})},hides:function(){this.bszs.map(function(a){var b=document.getElementById("busuanzi_container_"+a);b&&(b.style.display="")})},shows:function(){this.bszs.map(function(a){var b=document.getElementById("busuanzi_container_"+a);b&&(b.style.display="inline")})}};

```

如上操作其实就是把其中的`b.style.display="none"`中`none`去掉。

然后找到自己的不蒜子`js`文件引用路径将其改为新的目标地址即可。

参考链接：https://blog.csdn.net/weixin_37891983/article/details/105362748)

## 使用不蒜子出现502问题

> 这个问题的出现个人认为是频繁的刷新导致的。

### 解决方案

浏览器访问该不蒜子的官网：[不蒜子](https://busuanzi.ibruce.info/)

禁用cookies使用权限即可

![image-20220209195446756](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241801553.png)

参考链接：https://blog.csdn.net/fanqiliang630/article/details/117302625

