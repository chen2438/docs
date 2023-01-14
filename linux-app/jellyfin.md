---
description: 发布于 2022-10-27
categories:
- linux-app
date: 2022-10-27
slug: jellyfin
title: 基于 Docker 搭建 Jellyfin 媒体服务器
---

# 基于 Docker 搭建 Jellyfin 媒体服务器

[官方教程](https://jellyfin.org/docs/general/administration/installing)

如果未安装 docker-compose, 只需要 `sudo apt install docker-compose` 即可

编写 docker-compose.yml

```yaml
version: '3.5'
services:
  jellyfin:
    image: jellyfin/jellyfin
    container_name: jellyfin
    network_mode: 'host' #与宿主机共享网络
    volumes:
      - /path/to/config:/config
      - /path/to/cache:/cache
      - /path/to/media:/media
      - /path/to/media2:/media2:ro
    restart: 'unless-stopped'
```

启动服务

```bash
sudo docker-compose up -d
```

启动后可以通过 docker ps 查看容器状态

![image-2022102720470887](https://media.opennet.top/i/2023/01/05/63b6c87a0ed6d.png)

此时就可以通过 http://你的ip:8096 访问 jellyfin 服务（先确认服务器防火墙已放行8096端口）
