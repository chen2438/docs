---
description: 转载于 2022.10.26
---

# ubuntu 搭建 http 文件服务器

[参考链接](https://blog.csdn.net/mcsbary/article/details/105312864)

首先安装 Apache2

```bash
sudo apt install apache2
```

Apache2的默认访问端口为80，当端口被占用时需要更改其访问端口

进入apache2的安装目录 /etc/apache2/，修改ports.conf文件

```bash
# If you just change the port or add more ports here, you will likely also
# have to change the VirtualHost statement in
# /etc/apache2/sites-enabled/000-default.conf

#Listen 80
Listen 8001

<IfModule ssl_module>
        Listen 443
</IfModule>

<IfModule mod_gnutls.c>
        Listen 443
</IfModule>

# vim: syntax=apache ts=4 sw=4 sts=4 sr noet
12345678910111213141516
```

进入 目录 /etc/apache2/sites-available， 修改000-default.conf文件

```bash
#<VirtualHost *:80>
<VirtualHost *:8001>
        # The ServerName directive sets the request scheme, hostname and port that
        # the server uses to identify itself. This is used when creating
        # redirection URLs. In the context of virtual hosts, the ServerName
        # specifies what hostname must appear in the request's Host: header to
        # match this virtual host. For the default virtual host (this file) this
        # value is not decisive as it is used as a last resort host regardless.
        # However, you must set it for any further virtual host explicitly.
        #ServerName www.example.com

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/html

        # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
        # error, crit, alert, emerg.
        # It is also possible to configure the loglevel for particular
        # modules, e.g.
        #LogLevel info ssl:warn

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

        # For most configuration files from conf-available/, which are
        # enabled or disabled at a global level, it is possible to
        # include a line for only one particular virtual host. For example the
        # following line enables the CGI configuration for this host only
        # after it has been globally disabled with "a2disconf".
        #Include conf-available/serve-cgi-bin.conf
</VirtualHost>

# vim: syntax=apache ts=4 sw=4 sts=4 sr noet
1234567891011121314151617181920212223242526272829303132
```

然后重启apache服务器

```bash
sudo /etc/init.d/apache2 restart
```

apache服务器的默认目录在/var/www/html，如果想利用http服务器下载文件，需要删除其index.html文件，然后把文件放在该目录即可\
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200404165624996.png)
