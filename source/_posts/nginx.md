---
title: nginx指南
date: 2016-10-25
tags: [nginx, backend]
---

## nginx安装及基础命令

```bash
# mac下安装nginx. 默认安装于/usr/local/etc/nginx
brew install nginx
```

常用命令:

```bash
sudo nginx -s stop  #关闭nginx
sudo nginx | sudo nginx -c /absolute/path/to/nginx-config-file  #启动nginx
sudo ngxin -s reload  #重启nginx
nginx -t #测试nginx配置文件是否有效
```

#### 此处有坑.
如果遇到这个错误： nginx: [emerg] bind() to 0.0 .0 .0: 80 failed(48: Address already in use)
可以使用 lsof -i :80查看使用该端口的进程然后用kill 端口号杀掉该进程。

<!-- more -->

## 创建nginx脚本
```bash
# !/bin/sh

###BEGIN INIT INFO# Provides: nginx
# Required - Start: $all
# Required - Stop: $all
# Default - Start: 2 3 4 5
# Default - Stop: 0 1 6
# Short - Description: starts the nginx web server
# Description: starts nginx using start-stop-daemon
### END INIT INFO

PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin
DAEMON=/usr/local/bin/nginx
PID_DIR="${DAEMON}/logs"
NAME=nginx
DESC=nginx

# exit immediately if a simple command exits with a non - zero status.
set -e

# ensure PID_DIR
# -d: test whether directory exists
if [ ! -d $PID_DIR ];
then
    mkdir $PID_DIR
fi

#$1:second argument.
case "$1" in  
    start)
        echo -n "Starting $DESC:"
        start-stop-daemon --start --quiet --pidfile ${PID_DIR}/$NAME.pid --exec $DAEMON
        echo "$NAME."
        ;;
    stop)
        echo - n "Stopping $DESC:"
        start-stop-daemon --stop --quiet --pidfile ${PID_DIR}/$NAME.pid --exec $DAEMON
        echo "$NAME."
        ;;
    restart|force-reload)
        echo -n "Restarting $DESC:"
        start-stop-daemon --stop --quiet --pidfile ${PID_DIR}/$NAME.pid --exec $DAEMON
        sleep 1
        start-stop-daemon --start --quiet --pidfile ${PID_DIR}/$NAME.pid --exec $DAEMON
        echo "$NAME."
        ;;
    reload)
        echo -n "Reloading $DESC configuration:"
        start-stop-daemon --stop --signal HUP --quiet --pidfile ${PID_DIR}/$NAME.pid --exec $DAEMON
        echo "$NAME."
        ;;
    *)
        N=/etc/init.d/$NAME
        echo "Usage: $N {start|stop|restart|reload|force-reload}"
        exit 1
        ;;
    esac
exit 0
```

## nginx配置

```bash
# conf主文件: /usr/local/etc/nginx/nginx.conf
user www-data;
worker_processes auto;
pid /run/nginx.pid; #进程id文件

events {
    worker_connections 768;
}

http {
    # Basic Settings
    sendfile on;
    tcp_nopush on;
    tcp_nodelay on;
    keepalive_timeout 65;
    types_hash_max_size 2048;

    # import mime types
    include /etc/nginx/mime.types;
    default_type application/octet-stream;

    # SSL Settings
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2; # Dropping SSLv3, ref: POODLE
    ssl_prefer_server_ciphers on;

    # Logging Settings
    access_log /var/log/nginx/access.log;
    error_log /var/log/nginx/error.log;

    # Gzip Settings
    gzip on;
    gzip_disable "msie6";

    # Virtual Host Configs
    include /etc/nginx/conf.d/*.conf;
    include /etc/nginx/sites-enabled/*; #引入sites-enabled下的所有文件
}

## /usr/local/etc/nginx/sites-enabled/default文件:
server {
    listen 80 default_server;
    listen [::]:80 default_server;

    server_name mzjh.github.io;
    return 301 https://$server_name$request_uri;
}

server {
    listen 443;

    ssl on;
    ssl_certificate /path/to/certificate;
    ssl_certificate_key /path/to/crt-private-key;

    root /root/for/your/app;

    # Add index.php to the list if you are using PHP
    index index.html index.htm index.nginx-debian.html;

    server_name mzjh.github.io;

    # location ~ uri路径正则：区分大小写
    location ~ ^/site/(r|res)/(.+)$ {
        # 匹配 /site/r/hello, 不匹配 /site/R/hello
        expires 10y;
        alias /my-site-app/public/$1/$2;
    }

    # location ~* uri路径正则：不区分大小写
    location ~* ^/(r|res)/(.+)$ {
        # 匹配 /r/hello和/R/hello
        expires 10y;
        alias /my-main-app/public/$1/$2;
    }

    # location = uri路径: 精确匹配制定路径，不包括子路径.
    location = /favicon.ico {
        root /my-site-app/public/res/;
    }

    # location uri路径：对当前路径以及路径下的所有资源有效
    location / {
        # 匹配 /blog, /blog/blabla, /hello
        expires -1;
        proxy_pass http://localhost:8082;
    }
}

```
location匹配优先级:
    1. 精确匹配. i.e. =
    2. 正则匹配. i.e. ~, ~*
    3. 路径匹配.


## ssl certificate
```bash
#生成秘钥
openssl genrsa -out my-key.pem 2048

#生成一个证书请求
openssl req -new -key my-key.pem -out cert.csr

#生成证书/公钥：
openssl req -new -x509 -key my-key.pem -out my-pub-key.pem -days 1095
```

## 参考资料:
[linode](https://www.linode.com/docs/websites/nginx/how-to-configure-nginx)
[nginx核心模块](http://nginx.org/en/docs/http/ngx_http_core_module.html)

