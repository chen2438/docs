---
description: 发布于 2022.11.23
---

# linux vps 常用脚本

以下默认 root 权限运行，在 Debian 10 / Ubuntu 20.04 以上可用

一键BBR

```bash
wget --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh && chmod +x bbr.sh && ./bbr.sh
```

一键Docker

```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
```
