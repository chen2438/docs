---
description: 发布于 2022.11.25
---

# debian 安装 MySQL 并配置 root 远程登录

参考 [https://serverspace.io/support/help/how-to-install-mysql-on-debian-10/](https://serverspace.io/support/help/how-to-install-mysql-on-debian-10/)

以下默认 root 权限执行

#### 安装 MySQL

更新软件包

```bash
apt update
```

从[官方页面](https://dev.mysql.com/downloads/repo/apt/)下载 MySQL 或使用 wget 命令

```bash
wget https://dev.mysql.com/get/mysql-apt-config_0.8.24-1_all.deb
```

安装 deb

```bash
dpkg -i mysql-apt-config_0.8.24-1_all.deb
```

在弹出的窗口用方向键选择 ok, 回车

![image-20221125183242040](debian-an-zhuang-mysql-bing-pei-zhi-root-yuan-cheng-deng-lu.assets/image-20221125183242040.png)

更新 apt 存储库

```bash
apt update
```

安装 MySQL

```bash
apt install mysql-server
```

在弹出的窗口设置数据库的 root 密码

![image-20221125202500907](debian-an-zhuang-mysql-bing-pei-zhi-root-yuan-cheng-deng-lu.assets/image-20221125202500907.png)

![image-20221125202515593](debian-an-zhuang-mysql-bing-pei-zhi-root-yuan-cheng-deng-lu.assets/image-20221125202515593.png)

在接下来的窗口选择加密方式，依据客户端版本选择对应方式（强加密/传统加密），我选择了默认选项强加密

![image-20221125202529629](debian-an-zhuang-mysql-bing-pei-zhi-root-yuan-cheng-deng-lu.assets/image-20221125202529629.png)

检查服务状态

```bash
systemctl status mysql
```

![image-20221125202635034](debian-an-zhuang-mysql-bing-pei-zhi-root-yuan-cheng-deng-lu.assets/image-20221125202635034.png)

#### 配置安全性和远程访问

```bash
mysql_secure_installation
```

根据问题回答“是”（Y/y 按钮）或“否”（任何其他键）

在 Disallow root login remotely? 问题时回答 no 以允许远程 root 访问

![image-20221125202847592](debian-an-zhuang-mysql-bing-pei-zhi-root-yuan-cheng-deng-lu.assets/image-20221125202847592.png)

![image-20221125202919613](debian-an-zhuang-mysql-bing-pei-zhi-root-yuan-cheng-deng-lu.assets/image-20221125202919613.png)

编辑 mysqld.cnf 来允许远程访问

```bash
vim /etc/mysql/mysql.conf.d/mysqld.cnf
```

在文件末尾添加 bind-address = 0.0.0.0 以监听所有端口，或 127.0.0.1 以监听本机

![image-20221125184318702](debian-an-zhuang-mysql-bing-pei-zhi-root-yuan-cheng-deng-lu.assets/image-20221125184318702.png)

记得在防火墙放通 3306 端口

登入 MySQL 

```bash
mysql -uroot -p
```

然后输入前面设置的 root 密码

![image-20221125184937542](debian-an-zhuang-mysql-bing-pei-zhi-root-yuan-cheng-deng-lu.assets/image-20221125184937542.png)

依次输入以下两行代码来允许远程 root 登录

```mysql
GRANT ALL PRIVILEGES ON *.* TO 'root'@'localhost';
UPDATE mysql.user SET host='%' WHERE user='root';
```

![image-20221125203053969](debian-an-zhuang-mysql-bing-pei-zhi-root-yuan-cheng-deng-lu.assets/image-20221125203053969.png)

输入 exit 登出 MySQL，然后重新启动 MySQL

```bash
systemctl restart mysql
```

![image-20221125203208475](debian-an-zhuang-mysql-bing-pei-zhi-root-yuan-cheng-deng-lu.assets/image-20221125203208475.png)

然后再 `mysql -uroot -p` 登录一次 MySQL（以避免奇怪的客户端连接问题 Public Key Retrieval is not allowed ）

