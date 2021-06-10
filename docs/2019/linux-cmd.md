---
title: Linux 命令
date: 2019-09-19 00:00:00
---

## Docker 安装

```bash
// Linux Docker 安装
curl -sSL https://get.docker.com/ | sh

// 启动Docker服务并设为开机启动
systemctl start docker && systemctl enable docker

// 测试Docker配置正常
docker run hello-world


// docker-compose 安装
sudo curl -L "https://github.com/docker/compose/releases/download/1.24.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

// 添加执行权限
sudo chmod +x /usr/local/bin/docker-compose

// 查看版本
docker-compose --version
```

## 系统通知

```bash
// You have new mail in /var/spool/mail/root

// 查看
cat /var/spool/mail/root

// 清空
cat /dev/null > /var/spool/mail/root
```

## Linux 磁盘清理

```bash
df -hl //查看磁盘剩余空间
 
df -h //查看每个根路径的分区大小
 
du -sh [目录名] //返回该目录的大小
 
du -sm [文件夹] //返回该文件夹总M数
 
du -h [目录名] //查看指定文件夹下的所有文件大小（包含子文件夹）
```