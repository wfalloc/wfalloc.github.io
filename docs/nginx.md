# NGINX 编译安装

## 下载

[http://nginx.org/en/download.html](http://nginx.org/en/download.html)

## 解压

> tar -zxvf nginx-1.21.0.tar.gz

> tar -zxf nginx-1.21.0.tar.gz

## 安装

```
./configure

make && make install
```

## nginx 常用命令

```
// 检查配置文件
./nginx -t
// 帮助
./nginx -h
// 设置默认配置文件
./nginx -c
// 启动
./nginx
// 重载配置
./nginx -s reload

./nginx -s stop
```

### 添加软连接

> ln -s /usr/local/nginx/sbin/nginx /usr/bin/nginx

### 检查防火墙设置

> 开放 80 端口

### 添加开机自启

```
vim /etc/rc.d/rc.local

添加 nginx

// 添加执行权限
chmod -x /etc/rc.d/rc.local
chmod -x /etc/rc.local
chmod -x /usr/local/nginx/sbin/nginx

```

### nginx 管理脚本

创建脚本

```
vi  /etc/init.d/nginx
```

```
#! /bin/sh
# chkconfig: 2345 55 25
# Description: Startup script for nginx webserver on Debian. Place in /etc/init.d and
# run 'update-rc.d -f nginx defaults', or use the appropriate command on your
# distro. For CentOS/Redhat run: 'chkconfig --add nginx'
 
### BEGIN INIT INFO
# Provides:          nginx
# Required-Start:    $all
# Required-Stop:     $all
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# Short-Description: starts the nginx web server
# Description:       starts nginx using start-stop-daemon
### END INIT INFO
 
# Author:   licess
# website:  http://lnmp.org
 
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin
NAME=nginx
NGINX_BIN=/usr/local/nginx/sbin/$NAME
CONFIGFILE=/usr/local/nginx/conf/$NAME.conf
PIDFILE=/usr/local/nginx/logs/$NAME.pid
 
case "$1" in
    start)
        echo -n "Starting $NAME... "
 
        if netstat -tnpl | grep -q nginx;then
            echo "$NAME (pid `pidof $NAME`) already running."
            exit 1
        fi
 
        $NGINX_BIN -c $CONFIGFILE
 
        if [ "$?" != 0 ] ; then
            echo " failed"
            exit 1
        else
            echo " done"
        fi
        ;;
 
    stop)
        echo -n "Stoping $NAME... "
 
        if ! netstat -tnpl | grep -q nginx; then
            echo "$NAME is not running."
            exit 1
        fi
 
        $NGINX_BIN -s stop
 
        if [ "$?" != 0 ] ; then
            echo " failed. Use force-quit"
            exit 1
        else
            echo " done"
        fi
        ;;
 
    status)
        if netstat -tnpl | grep -q nginx; then
            PID=`pidof nginx`
            echo "$NAME (pid $PID) is running..."
        else
            echo "$NAME is stopped"
            exit 0
        fi
        ;;
 
    force-quit)
        echo -n "Terminating $NAME... "
 
        if ! netstat -tnpl | grep -q nginx; then
            echo "$NAME is not running."
            exit 1
        fi
 
        kill `pidof $NAME`
 
        if [ "$?" != 0 ] ; then
            echo " failed"
            exit 1
        else
            echo " done"
        fi
        ;;
 
    restart)
        $0 stop
        sleep 1
        $0 start
        ;;
 
    reload)
        echo -n "Reload service $NAME... "
 
        if netstat -tnpl | grep -q nginx; then
            $NGINX_BIN -s reload
            echo " done"
        else
            echo "$NAME is not running, can't reload."
            exit 1
        fi
        ;;
 
    configtest)
        echo -n "Test $NAME configure files... "
 
        $NGINX_BIN -t
        ;;
 
    *)
        echo "Usage: $0 {start|stop|force-quit|restart|reload|status|configtest}"
        exit 1
        ;;
 
esac
```

添加可执行权限

```
chmod +x /etc/init.d/nginx
```

使用

```
service nginx start
service nginx stop
service nginx status
```

## 常见问题

* ./configure: error: C compiler cc is not found

  未安装 gcc 。使用 apt install gcc

* ./configure: error: the HTTP rewrite module requires the PCRE library.
  
  apt install libpcre3 libpcre3-dev

* ./configure: error: the HTTP gzip module requires the zlib library.

  apt install zlib1g-dev

* make：未找到命令

  apt install make

* make: 警告:检测到时钟错误。您的创建可能是不完整的
  
  系统时间问题。原因是有文件的时间比当前时间还要晚。更新一下系统时间

* nginx: [error] invalid PID number 无法正常启动

    解决方法：

    需要先执行

    nginx -c /etc/nginx/nginx.conf

    nginx.conf文件的路径可以从nginx -t的返回中找到。

    nginx -s reload

* nginx: [emerg] getgrnam("nobody") failed

  在服务器系统中添加www用户组和用户www

  groupadd -f www

  useradd -g www www

  修改 /usr/local/nginx/conf/nginx.conf 

  user www;
