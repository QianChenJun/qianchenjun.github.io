---
title: Docker常用镜像
tags:
  - Docker
categories:
  - VPS
abbrlink: 11a8079c
date: 2022-08-06 09:58:42
updated: 2022-08-06 09:58:42
---



*<!--more-->*

## Docker安装

```shell
#卸载系统之前的docker 
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
                  
                  
sudo yum install -y yum-utils

# 配置镜像
sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
    
sudo yum install docker-ce docker-ce-cli containerd.io

sudo systemctl start docker
# 设置开机自启动
sudo systemctl enable docker

docker -v
sudo docker images

# 配置镜像加速（阿里云）
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://chqac97z.mirror.aliyuncs.com"]
}
EOF

sudo systemctl daemon-reload
sudo systemctl restart docker
```



## 设置镜像开机自启动

```shell
docker update xxx --restart=always
# 取消操作
docker update xxx --restart=no
```



## Mysql镜像

```shell
sudo docker pull mysql:5.7

# --name指定容器名字 -v目录挂载 -p指定端口映射  -e设置mysql参数 -d后台运行
sudo docker run -p 3307:3306 --name mysql \
-v /mydata/mysql/log:/var/log/mysql/ \
-v /mydata/mysql/data:/var/lib/mysql/ \
-v /mydata/mysql/conf:/etc/mysql/my.conf \
-e MYSQL_ROOT_PASSWORD=y123456... \
-d mysql:5.7
```

修改配置文件（外部映射）

```shell
vim /mydata/mysql/conf/my.conf 

[client]
default-character-set=utf8
[mysql]
default-character-set=utf8
[mysqld]
init_connect='SET collation_connection = utf8_unicode_ci'
init_connect='SET NAMES utf8'
character-set-server=utf8
collation-server=utf8_unicode_ci
skip-character-set-client-handshake
skip-name-resolve

docker restart mysql
```

**注意：后续重新测试的时候，mysql设置的默认密码没有生效，而是保持root不变**

修改密码：

1. 登录docker容器的mysql `docker exec -it mysql /bin/bash`
2. `SET PASSWORD FOR 'root'= PASSWORD('设置的密码');`
3. 重新启动mysql `docker restart mysql`



## Redis镜像

> 如果直接挂载的话docker会以为挂载的是一个目录，所以我们先创建一个文件然后再挂载，在虚拟机中。

```shell
# 在虚拟机中
mkdir -p /mydata/redis/conf
touch /mydata/redis/conf/redis.conf

docker pull redis

docker run -p 6379:6379 --name redis \
-v /mydata/redis/data:/data \
-v /mydata/redis/conf/redis.conf:/etc/redis/redis.conf \
-d redis redis-server /etc/redis/redis.conf

# 直接进去redis客户端。
docker exec -it redis redis-cli
```

> Redis默认是不持久化的**（但是目前新版好像自带持久化）**。在配置文件中输入`appendonly yes`，就可以aof持久化了。修改完`docker restart redis，docker -it redis redis-cli`

```shell
vim /mydata/redis/conf/redis.conf
# 插入下面内容
appendonly yes
#保存
docker restart redis
```

## Elasticsearch镜像

```shell
# 拉取镜像
docker pull elasticsearch:7.6.2
docker pull kibana:7.6.2

# 配置
mkdir -p /mydata/elasticsearch/config
mkdir -p /mydata/elasticsearch/data
echo "http.host: 0.0.0.0" >/mydata/elasticsearch/config/elasticsearch.yml

# 将mydata/elasticsearch/文件夹中文件都可读可写
chmod -R 777 /mydata/elasticsearch/

# 启动elasticsearch
docker run --name elasticsearch -p 9200:9200 -p 9300:9300 \
-e  "discovery.type=single-node" \
-e ES_JAVA_OPTS="-Xms128m -Xmx512m" \
-v /mydata/elasticsearch/config/elasticsearch.yml:/usr/share/elasticsearch/config/elasticsearch.yml \
-v /mydata/elasticsearch/data:/usr/share/elasticsearch/data \
-v /mydata/elasticsearch/plugins:/usr/share/elasticsearch/plugins \
-d elasticsearch:7.6.2 

# 开机自启动
docker update elasticsearch --restart=always

# 启动kibana
docker run --name kibana -e ELASTICSEARCH_HOSTS=http://服务器地址:9200 -p 5601:5601 -d kibana:7.6.2

# 开机自启动
docker update kibana --restart=always
```



## Nginx镜像

```shell
# 随便启动一个nginx实例，只是为了复制出配置
docker run -p 8081:80 --name nginx -d nginx:1.10

# 将容器内的配置文件拷贝到/mydata/nginx/conf/目录下
mkdir -p /mydata/nginx
docker container cp nginx:/etc/nginx /mydata/nginx/  
# 将nginx下面的nginx文件夹名称改成conf
mv /mydata/nginx/nginx /mydata/nginx/conf

# 终止原容器
docker stop nginx

# 执行命令删除原容器
docker rm nginx

# 创建新的Nginx，执行以下命令
docker run -p 80:80 --name nginx \
 -v /mydata/nginx/html:/usr/share/nginx/html \
 -v /mydata/nginx/logs:/var/log/nginx \
 -v /mydata/nginx/conf/:/etc/nginx \
 -d nginx:1.10
 
# 设置开机启动nginx
docker update nginx --restart=always

# 创建“/mydata/nginx/html/index.html”文件，测试是否能够正常访问
echo '<h2>hello nginx!</h2>' >/mydata/nginx/html/index.html
# 访问：http://ngix所在主机的IP:9090/index.html
```
