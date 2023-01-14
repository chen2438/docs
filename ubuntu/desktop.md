---
description: 发布于 2022-09-25
categories:
- ubuntu
date: 2022-09-25
slug: desktop
title: Ubuntu 安装桌面
---

# Ubuntu 安装桌面

```bash
sudo -i

apt install tasksel
tasksel install ubuntu-desktop
```

此时在腾讯云的 VNC 远程登陆开始显示安装的图形界面

可能会卡在 59% 一段时间，等待即可

![image-20220922174610802](https://media.opennet.top/i/2023/01/05/63b6cb6dce798.png)

安装完毕后重新显示命令行界面，键入`reboot`命令重启服务器

![image-20220922175143462](https://media.opennet.top/i/2023/01/05/63b6cb6f0e9cb.png)

如图所示完成ubuntu图形界面安装

在设置应用可安装中文

![image-20220922175444121](https://media.opennet.top/i/2023/01/05/63b6cb7138337.png)

安装完成后需要重启才能选择中文，然后再次重启应用中文
