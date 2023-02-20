---
description: 发布于 2022-11-23, 修订于 2023-01-14
categories:
- debian
date: 2022-11-23
slug: d-vps-script
title: Debian VPS 常用脚本
---

# Debian VPS 常用脚本

### 初始化

```bash
apt -y update && apt -y install tmux curl sudo locales vim apparmor
```

### 设置时区

```bash
timedatectl set-timezone Asia/Shanghai
```

### 开启BBR

```bash
wget -N --no-check-certificate "https://github.000060000.xyz/tcpx.sh" && chmod +x tcpx.sh && ./tcpx.sh
```

### 虚拟内存

```bash
wget https://www.moerats.com/usr/shell/swap.sh && bash swap.sh
```

### Bench

```bash
wget -qO- bench.sh | bash
```

### IO与CPU测试

```bash
curl -sL yabs.sh | bash -s -- -i
```

### 回程脚本

```bash
curl https://raw.githubusercontent.com/zhanghanyun/backtrace/main/install.sh -sSf | sh
```

### 流媒体检测

```bash
bash <(curl -L -s https://git.io/JRw8R)
```

### Magic

```bash
bash <(curl -Ls https://raw.githubusercontent.com/vaxilu/x-ui/master/install.sh)
```

### 重新配置LOCALE

```bash
dpkg-reconfigure locales
```

### Smart Ping

国际

```bash
cd && mkdir smartping && cd smartping && wget https://github.com/smartping/smartping/releases/download/v0.8.0/smartping-v0.8.0.tar.gz && tar -zxvf smartping-v0.8.0.tar.gz
```

国内

```bash
cd && mkdir smartping && cd smartping && wget https://media.opennet.top:8087/directlink/2/smartping-v0.8.0.tar.gz && tar -zxvf smartping-v0.8.0.tar.gz
```

设置开机启动

```bash
# vim /usr/lib/systemd/system/smartping.service
vim /etc/systemd/system/smartping.service
```

写入以下内容

```bash
[Unit]
Description=smartping

[Service]
Type=forking
User=root
ExecStart=/root/smartping/control start
ExecStop=/root/smartping/control stop
ExecReload=/root/smartping/control restart

[Install]
WantedBy=multi-user.target
```

启动并添加到系统服务(开机启动)

```bash
systemctl start smartping
systemctl enable smartping
```

### Docker

```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sh get-docker.sh
```

### 网心云

```bash
docker run -d --name=wxedge --restart=always --privileged --net=host  --tmpfs /run --tmpfs /tmp -v /root/wxedge/data:/storage:rw  registry.hub.docker.com/onething1/wxedge
```

### Proxy Peers

```bash
export P2P_EMAIL=eternal.cht@gmail.com #你的邮箱
docker run -d --restart always \
        -e P2P_EMAIL=$P2P_EMAIL \
        --name peer2profit \
        peer2profit/peer2profit_linux:latest 
```

### Traffmonetizer

```bash
docker run -d --restart=always --name traffmonetizer traffmonetizer/cli start accept --token EHaXuguX8Ae3GumOrdw1VqXJqYOWbbnrsKs+rLO4jzw= #你的token
```

### unzip批量解压

```bash
#! /bin/sh
for i in *.zip
do
k=$i
s=${k%.zip*}
echo $s
unzip $i -d $s
done
```

