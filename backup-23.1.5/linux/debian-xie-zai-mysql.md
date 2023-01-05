---
description: 转载于 2022.11.25
---

# debian 卸载 MySQL

通过 `dpkg --get-selections | grep mysql` 命令罗列出你电脑上安装的和MySQL相关的软件，然后purge卸载

```bash
sudo apt-get --purge remove mysql-server
sudo apt-get --purge remove mysql-client
sudo apt-get --purge remove mysql-common
```

最后再通过下面的命令清理残余：

```bash
apt-get autoremove
apt-get autoclean
rm /etc/mysql/ -R
rm /var/lib/mysql/ -R
```

好了，至此卸载清理工作全部完成，下面可以重新安装了:-)
