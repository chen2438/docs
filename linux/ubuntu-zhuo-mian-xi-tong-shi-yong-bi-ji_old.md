---
description: 发布于 2022.09.25, 修订于 2022.10.05
---

# ubuntu 桌面系统使用笔记

#### 添加右键新建文件

在 主目录/模板 下新建文件

![image-20220924231948971](https://picgo-1303840613.cos.ap-shanghai.myqcloud.com/image-20220924231948971.png)

![image-20220924232001579](https://picgo-1303840613.cos.ap-shanghai.myqcloud.com/image-20220924232001579.png)

#### 安装罗技鼠标软件

`sudo apt install piper`

![image-20220924232139686](https://picgo-1303840613.cos.ap-shanghai.myqcloud.com/image-20220924232139686.png)

#### 终端复制粘贴

`ctrl + shift + c，ctrl + shift + v`

#### AppImage 格式安装与卸载

`chmod u+x <AppImage File>`

`sudo apt install fuse -y`

双击后选择 Run once

![image-20220924232740629](https://picgo-1303840613.cos.ap-shanghai.myqcloud.com/image-20220924232740629.png)

卸载只要删掉 .Appimage 文件即可

#### 设置截图快捷键

设置/键盘/键盘快捷键

搜索截图

![image-20220924232950667](https://picgo-1303840613.cos.ap-shanghai.myqcloud.com/image-20220924232950667.png)

#### 限制电池充电

[TLP - Optimize Linux Laptop Battery Life](https://linrunner.de/tlp/index.html)

[TLP (简体中文) - ArchWiki](https://wiki.archlinux.org/title/TLP\_\(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87\))

[TLP-1.4-Test\_Battery-Care\_Lenovo.md](https://gist.github.com/linrunner/4a6876648765fac5e141f15d0582a945)

#### 禁用默认密钥环解锁密码

终端 `seahorse`

![image-20220925004754751](https://picgo-1303840613.cos.ap-shanghai.myqcloud.com/image-20220925004754751.png)

右键默认密钥环，更改密码

在输入新密码时留空

#### 创建目录快捷方式

`ln -s /home/chen/Github/posts /home/chen/Desktop/posts`

#### 开机自启动蓝牙

`sudo systemctl enable bluetooth`

#### 查看CPU温度

安装psensor

`sudo apt install psensor`

#### 查看CPU架构

`lscpu`
