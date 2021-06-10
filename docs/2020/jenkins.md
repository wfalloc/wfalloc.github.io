
### 准备工具

首先你需要一台服务器、一个域名。

#### 安装 Docker

```
curl -sSL https://get.docker.com/ | sh
```

启动 Docker 并设为开机启动

```
systemctl start docker && systemctl enable docker
```

测试 Docker

```
docker -v
// Docker version 19.03.1, build 74b1e89
```

### 开始折腾


#### 新建 Jenkins 文件夹

```
mkdir jenkins && cd jenkins
```

#### 启动文件

新建 start.sh 配置文件

```
vim start.sh
```

复制以下参数粘贴到 start.sh 文件中

```
docker run -d \
--name jenkins \
-p 80:8080 \
-v /www/wwwroot/jenkins/data:/var/jenkins_home \
jenkins/jenkins:lts
```

启动服务

```
sh start.sh
```

#### 错误解决

data 文件夹权限问题

```
chown -R www:www data/
```

管理 Jenkins 显示反向代理配置有误

 修改反向代理配置，增加 `proxy_set_header X-Forwarded-Proto $scheme;`

```
location /
{
    expires 12h;
    if ($request_uri ~* "(php|jsp|cgi|asp|aspx)")
    {
         expires 0;
    }
    proxy_pass http://127.0.0.1:84;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header REMOTE-HOST $remote_addr;
    
    #持久化连接相关配置
    #proxy_connect_timeout 30s;
    #proxy_read_timeout 86400s;
    #proxy_send_timeout 30s;
    #proxy_http_version 1.1;
    #proxy_set_header Upgrade $http_upgrade;
    #proxy_set_header Connection "upgrade";
    add_header X-Cache $upstream_cache_status;
    
    #Set Nginx Cache
    
    	add_header Cache-Control no-cache;
}
```

#### 添加反向代理

使用宝塔面板开启反向代理

---

date: 2020-01-16 00:00:00