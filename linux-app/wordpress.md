---
description: 发布于 2022-09-07, 修订于 2022-09-27
categories:
- linux-app
date: 2022-09-07
slug: wordpress
title: 基于 Docker 搭建 WordPress 并支持 HTTPS
---

# 基于 Docker 搭建 WordPress 并支持 HTTPS

### 安装 docker 与 docker compose

[Docker 安装与基本用法](https://chenhaotian.top/docker-%e5%9f%ba%e6%9c%ac%e7%94%a8%e6%b3%95/)

安装 docker compose

```bash
apt -y update
apt -y install docker-compose
```

### 拉取 mysql, wordpress 镜像

(可跳过, 启动时会自动下载)

```bash
docker pull mysql
docker pull wordpress
```

### 启动容器

编写 docker-compose.yml

注意参数前必须空一格

```yaml
version: '3.3'
services:
  db:
     image: mysql:latest
     container_name: "wordpress_mysql"
     ports:
       - "3307:3306"
     volumes:
       - ~/wordpress/mysql:/var/lib/mysql
     restart: always
     environment:
       MYSQL_ROOT_PASSWORD: 123456
       MYSQL_DATABASE: wordpress
       MYSQL_USER: admin
       MYSQL_PASSWORD: 123456
  wordpress:
     depends_on:
       - db
     image: wordpress:latest
     container_name: "wordpress"
     ports:
       - "80:80" 
       - "443:443"
     restart: always
     environment:
       WORDPRESS_DB_HOST: db:3306
       WORDPRESS_DB_USER: admin
       WORDPRESS_DB_PASSWORD: 123456
       WORDPRESS_DB_NAME: wordpress
       WORDPRESS_WPLANG: zh-CN
     volumes:
       - ~/wordpress/html:/var/www/html
```

运行容器

```bash
docker compose up -d
```

若要终止容器

```bash
docker compose down
rm -rf ~/wordpress
```

服务器防火墙开放 80 和 443 端口, 浏览器输入域名即可访问 WordPress

### 部署 SSL 证书

```bash
docker exec -it wordpress bash #进入容器
a2enmod ssl #加载 Apache SSL 模块
service apache2 restart
```

此时应该自动退出容器

重启容器

```bash
docker restart wordpress
```

用 `docker ps -a` 可以查看容器的运行状况, 如果 STATUS 是 Up xxx seconds 表示启动成功, 已运行了 xxx 秒

![image-20220911194133757](https://media.opennet.top/i/2023/01/05/63b6c95384a3a.png)

将下载好的 SSL 证书传入服务器, `.pem`文件命名为 `ssl-cert-snakeoil.pem`, `.key`文件命名为 `ssl-cert-snakeoil.key`.

如果使用 `.cer`，需要自行进入容器内修改 `/etc/apache2/sites-available/default-ssl.conf`

然后 copy 到容器的相应位置

```bash
docker cp ssl-cert-snakeoil.pem wordpress:/etc/ssl/certs/ssl-cert-snakeoil.pem
#docker cp ssl-cert-snakeoil.pem wordpress:/etc/ssl/certs/ssl-cert-snakeoil.cer
docker cp ssl-cert-snakeoil.key wordpress:/etc/ssl/private/ssl-cert-snakeoil.key
```

进入容器, 链接文件目录

```bash
docker exec -it wordpress bash
ln -s /etc/apache2/sites-available/default-ssl.conf /etc/apache2/sites-enabled/default-ssl.conf
```

退出容器, 然后重启容器

```bash
docker restart wordpress
```

最后，再加载一遍SSL模块

```bash
docker exec -it wordpress bash #进入容器
a2enmod ssl #加载 Apache SSL 模块
service apache2 restart
# 自动退出容器
docker restart wordpress
```

访问 WordPress 后台, 在设置中更改站点地址为 https://xxx.xxx

注意, 更改之前请尝试并确认可以通过 https 访问, 如果访问失败, 可以尝试清除浏览器缓存/强制刷新/换个浏览器

![image-20220911195047917](https://media.opennet.top/i/2023/01/05/63b6c9553f0a7.png)

此外, 可以强制 http 请求转到 https, 具体可见参考链接

### 参考链接

https://www.jianshu.com/p/e8ae8bb1ad0a
