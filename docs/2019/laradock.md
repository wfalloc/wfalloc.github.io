
## Laradock 首次使用

---

```
// 克隆仓库
git clone https://github.com/Laradock/laradock.git

// 生成配置文件
cp env-example .env

// env配置
DB_HOST=mysql
REDIS_HOST=redis
QUEUE_HOST=beanstalkd

// 修改env项目目录
APP_CODE_PATH_HOST=../project-z/

// nginx构建环境
docker-compose up -d nginx mysql redis workspace 

// apache构建环境
docker-compose up -d apache2 mysql redis workspace 
```

## 其他命令

---

```
// 进入工作区容器
docker-compose exec workspace bash

// 使用指定用户进入工作区容器
docker-compose exec --user=laradock workspace bash

// 进入容器
docker exec -it {workspace-container-id} bash

// 重启容器
docker-compose up -d nginx

// nginx配置文件修改
laradock/nginx/sites  default.conf 文件

// 查看项目容器状态
docker-compose ps

// 停止项目所有容器
docker-compose stop

// 停止单一容器
docker-compose stop {container-name}

// 删除项目所有容器
docker-compose down

// 重新构造所有容器
docker-compose build

// 重新构造容器
docker-compose build mysql
```

---

date: 2019-07-28 00:00:00