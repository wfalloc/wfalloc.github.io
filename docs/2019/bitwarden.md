---
title: Docker 自建 Bitwarden 密码服务器
date: 2019-11-29 00:00:00
---

密码管理工具最初接触的是 KeePass 。作为一个开源的密码管理器，KeePass 还是比较优秀的。奈何 KeePass 不支持密码库同步。接触到 Bitwarden 一款可以自己搭建的开源密码管理工具。瞬间有了替换密码管理工具的想法

### 准备工具

首先你需要一台服务器、一个域名。

#### 安装 Docker

```bash
curl -sSL https://get.docker.com/ | sh
```

启动 Docker 并设为开机启动

```bash
systemctl start docker && systemctl enable docker
```

测试 Docker

```bash
docker -v
// Docker version 19.03.1, build 74b1e89
```

#### 安装 docker-compose

```bash
sudo curl -L "https://github.com/docker/compose/releases/download/1.24.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

添加权限

```bash
sudo chmod +x /usr/local/bin/docker-compose
```

查看 docker-compose 版本

```bash
docker-compose --version
// docker-compose version 1.24.1, build 4667896b
```

### 开始折腾

Bitwarden 官方镜像使用 Mysql 作为密码数据库，对服务器配置要求较高。Github 上有个第三方项目 bitwarden_rs 使用 SQLite 数据库，对服务器要求较低。故我们使用 bitwarden_rs 搭建服务。

#### 新建 Bitwarden 文件夹

```bash
mkdir bitwarden && cd bitwarden
```

#### 配置文件

新建 config.env 配置文件

```bash
vim config.env
```

复制以下参数粘贴到 config.env 文件中

```
SIGNUPS_ALLOWED=true #是否开放用户注册
DOMAIN=https://xxx.com #Bitwarden 服务使用的域名
DATABASE_URL=/data/bitwarden.db #数据库在容器内的路径
ROCKET_WORKERS=10  #设置服务器线程
WEB_VAULT_ENABLED=true #是否开启 Web 客户端
```

新建 docker-compose.yml 构建文件

```
vim docker-compose.yml
```

复制以下配置粘贴到 docker-compose.yml 文件中

```
version: '3'

services:
  bitwarden:
    image: bitwardenrs/server:latest
    container_name: bitwarden
    restart: always
    volumes:
      - ./data:/data
    env_file:
      - config.env
    ports:
      - "82:80"  # 将容器的 80 端口映射到宿主机的 82 端口上
```

启动服务

```bash
docker-compose up -d
```

#### 添加反向代理

使用宝塔面板开启反向代理

### 数据导入

打开网页端注册用户并导入数据

### 关闭网页访问和用户注册

修改配置文件

```
SIGNUPS_ALLOWED=false #是否开放用户注册
WEB_VAULT_ENABLED=false #是否开启 Web 客户端
```

重启容器

```bash
docker-compose down && docker-compose up -d
```