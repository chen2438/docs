---
description: 发布于 2022.12.30
---

# 从零开始搭建 Lsky Pro 兰空图床

[官方教程](https://docs.lsky.pro/docs/free/v2/)

### 安装环境

使用[OneinStack](https://oneinstack.com/)安装环境(PHP+MySQL+Nginx)

![image-20221230212627538](https://media.opennet.top/i/2022/12/30/63aee6c6000c3.png)

命令如下(注意更换 ‘你的Mysql密码’ ):

```
 wget -c http://mirrors.linuxeye.com/oneinstack-full.tar.gz && tar xzf oneinstack-full.tar.gz && ./oneinstack/install.sh --nginx_option 1 --php_option 10 --phpcache_option 1 --php_extensions imagick,fileinfo --db_option 2 --dbinstallmethod 1 --dbrootpwd 你的Mysql密码 --ssh_port 22 --reboot 
```

安装时间可能较长, 请使用tmux以防止ssh断连.

安装完成后访问IP地址(确认防火墙放通80端口)即可看到面板.

![image-20221230181055468](https://media.opennet.top/i/2022/12/30/63aee6c8ba024.png)

此时在执行命令的目录可看到onesinstack

![image-20221230181346540](https://media.opennet.top/i/2022/12/30/63aee6c892970.png)

执行`./vhost.sh`创建虚拟站点

![《Interactive install》](https://media.opennet.top/i/2022/12/30/63aee6c611b80.png)

根据自己需求设置即可.

设置完成后会提示路径:

![image-20221230181807185](https://media.opennet.top/i/2022/12/30/63aee6c6080bb.png)

### 配置环境

#### 配置nginx

```
 vim /usr/local/nginx/conf/vhost/media.opennet.top.conf(你的Virtualhost conf)
```

如果要使用自己的SSL证书, 注意配置证书路径. 不使用HTTPS则忽略.

![截屏2022-12-30 18.25.20](https://media.opennet.top/i/2022/12/30/63aee6c605ec3.png)

更改root目录, 在后面添加`/public`

添加伪静态

```
 location / {
   try_files $uri $uri/ /index.php?$query_string;
 }
```

![截屏2022-12-30 18.27.26](https://media.opennet.top/i/2022/12/30/63aee6c5f1945.png)

修改完成后`nginx -t`测试配置文件是否正确

重启nginx

```
 systemctl restart nginx
```

#### 配置php

```
 vim /usr/local/php/etc/php.ini
```

找到disable\_functions

![image-20221230194414969](https://media.opennet.top/i/2022/12/30/63aee6c5f26fb.png)

删掉exec, chown, shell\_exec, readlink, symlink

重启php

```
 systemctl restart php-fpm
```

### 安装图床

进入网站目录

```
 cd /data/wwwroot/media.opennet.top(你的网址)
```

在[Releases](https://github.com/lsky-org/lsky-pro/releases)下载最新版本

```
 wget https://github.com/lsky-org/lsky-pro/releases/download/2.1/lsky-pro-2.1.zip
```

解压

```
 unzip lsky-pro-2.1.zip
```

修改文件权限

```
 chmod -R 755 /data/wwwroot/media.opennet.top
 chown -R www:www /data/wwwroot/media.opennet.top
```

访问网站域名

![image-20221230202540140](https://media.opennet.top/i/2022/12/30/63aee6c60142f.png)

根据提示安装即可

![image-20221230205242271](https://media.opennet.top/i/2022/12/30/63aee6c605954.png)

![image-20221230205321391](https://media.opennet.top/i/2022/12/30/63aee6c6023d1.png)

\
