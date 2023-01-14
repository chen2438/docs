---
description: 转载于 2022-11-25
categories:
- debian
date: 2022-11-25
slug: d-mysql-uninstall
title: Debian 卸载 MySQL
---

# Debian 卸载 MySQL

通过 `dpkg --get-selections | grep mysql` 命令列出你电脑上安装的和MySQL相关的软件，然后purge卸载

```bash
sudo apt-get --purge remove mysql-server
sudo apt-get --purge remove mysql-client
sudo apt-get --purge remove mysql-common
```

清理残余

```bash
apt-get autoremove #慎用
apt-get autoclean #慎用
rm /etc/mysql/ -R
rm /var/lib/mysql/ -R
```

