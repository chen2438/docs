---
description: 发布于 2022.10.07
---

# ubuntu 安装 MongoDB

#### 脚本安装

```bash
cat <<"EOF" | bash                              
sudo apt update && \
sudo apt install -y dirmngr gnupg apt-transport-https ca-certificates software-properties-common gnupg && \
wget -qO - https://www.mongodb.org/static/pgp/server-5.0.asc | sudo apt-key add - && \
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/5.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-5.0.list && \
sudo apt update && \
sudo apt install -y mongodb-org && \
sudo systemctl enable --now mongod
EOF
```

配置开机启动

```bash
sudo systemctl enable --now mongod
```

#### Docker 安装

(1) docker run 安装

```bash
docker pull mongo
docker run -it --name mongo -p 27017:27017 -v ~/data:/data/db mongo
```

(2) docker-compose 安装

编写 docker-compose.yml

```yaml
version: '3.3'
services: 
    mongo: 
      image: mongo:latest
      container_name: "mongo"
      ports: 
        - "27017:27017"
      volumes: 
        - ~/data:/data/db
      restart: always
```

启动并运行

```bash
docker-compose up -d
```
