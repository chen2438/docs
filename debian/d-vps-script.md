---
description: 发布于 2022-11-23, 修订于 2023-01-14
categories:
- debian
date: 2022-11-23
slug: d-vps-script
title: Debian VPS 常用脚本
---

# Debian VPS 常用脚本

#### 初始化

```bash
apt -y update && apt -y install tmux curl sudo locales
```

#### 设置时区

```bash
timedatectl set-timezone Asia/Shanghai
```

#### 开启BBR

```bash
wget -N --no-check-certificate "https://github.000060000.xyz/tcpx.sh" && chmod +x tcpx.sh && ./tcpx.sh
```

#### Bench

```bash
wget -qO- bench.sh | bash
```

#### IO与CPU测试

```bash
curl -sL yabs.sh | bash -s -- -i
```

#### 流媒体检测

```bash
bash <(curl -L -s https://git.io/JRw8R)
```

#### Magic

```bash
bash <(curl -Ls https://raw.githubusercontent.com/vaxilu/x-ui/master/install.sh)
```

#### 重新配置LOCALE

```bash
dpkg-reconfigure locales
```

#### Smart Ping

国际

```bash
cd && mkdir smartping && cd smartping && wget https://github.com/smartping/smartping/releases/download/v0.8.0/smartping-v0.8.0.tar.gz && tar -zxvf smartping-v0.8.0.tar.gz && ./control start
```

国内

```bash
cd && mkdir smartping && cd smartping && wget https://media.opennet.top:8087/directlink/2/smartping-v0.8.0.tar.gz && tar -zxvf smartping-v0.8.0.tar.gz && ./control start
```

#### Docker

```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
```

#### 网心云

```bash
docker run -d --name=wxedge --restart=always --privileged --net=host  --tmpfs /run --tmpfs /tmp -v /root/wxedge/data:/storage:rw  registry.hub.docker.com/onething1/wxedge
```

