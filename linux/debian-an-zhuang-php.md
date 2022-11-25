---
description: 转载于 2022.11.25
---

# debian 安装 PHP

参考 [https://www.php.net/manual/zh/install.unix.debian.php](https://www.php.net/manual/zh/install.unix.debian.php)

#### 使用 APT[ ¶](https://www.php.net/manual/zh/install.unix.debian.php#install.unix.debian.apt)

注意其它有关的包可能需要 `libapache-mod-php` 集成入 Apache 2，以及 PEAR 的 `php-pear`。

首先运行 **apt update**。

**安装 PHP**&#x20;

```
apt install php-common libapache2-mod-php php-cli
```

apt 将自动安装 Apache 2 的 PHP 模块以及所有依赖的库并激活

**重启 Apache**

```
systemctl restart apache2
```

#### 更好地控制配置[ ¶](https://www.php.net/manual/zh/install.unix.debian.php#install.unix.debian.config)

上一节中 PHP 仅安装了核心模块。很可能还需要更多模块，例如 [MySQL](https://www.php.net/manual/zh/book.mysql.php)，[cURL](https://www.php.net/manual/zh/book.curl.php)，[GD](https://www.php.net/manual/zh/book.image.php) 等。这些模块也可以通过 `apt` 命令安装。

**示例 #3 取得 PHP 附加软件包的列表**

```
# apt-cache search php
# apt search php | grep -i mysql
# aptitude search php
```

以上命令的输出中列出了很多的包，其中有几个针对 PHP 的模块例如 php-cgi，php-cli 以及 php-dev。决定好要安装哪些之后可以用 `apt` 或者 `aptitude` 来安装。Debian 会进行倚赖性检查，会给出提示，例如安装 MySQL 和 cURL：

**示例 #4 安装 PHP 的 MySQL 和 cURL 支持**

```
# apt install php-mysql php-curl
```

APT 会自动把适当的行添加到不同的 php.ini 相关文件中去，例如 /etc/php/7.4/php.ini，/etc/php/7.4/conf.d/\*.ini 等，并且根据扩展，还会添加类似 `extension=foo.so` 的内容。不过还是需要重新启动 web 服务器（例如 Apache）以使这些改动生效。
