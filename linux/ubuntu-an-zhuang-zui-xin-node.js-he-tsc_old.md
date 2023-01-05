---
description: 发布于 2022.10.07
---

# ubuntu 安装最新 Node.js 和 tsc

前往 [https://nodejs.org/](https://nodejs.org/) 查看最新版本号

![image-20221006235629453](http://nme-200t.oss-cn-hangzhou.aliyuncs.com/notes/2022-10-06-155630.png)\


添加对应版本安装源, 只需要改16为所需版本号

```bash
curl -sL https://deb.nodesource.com/setup_16.x | sudo -E bash -
```

apt 安装

```bash
sudo apt install -y nodejs
```

安装 tsc

```bash
sudo npm install -g typescript
```

如果安装完成后有提示可升级到新版本, 按照提示键入命令即可
