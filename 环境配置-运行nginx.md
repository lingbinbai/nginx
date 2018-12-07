# 本例以/root/nginx目录为例
### 初始化目录
```bash
mkdir /root/nginx/
cd /root/nginx/
mkdir bin
mkdir logs
mkdir conf
mkdir temp
mkdir template
```

### 编写工具脚本
#### 启动nginx
cd /root/nginx/bin

touch start
``` bash
#!/bin/bash

CWD=`pwd`
APP_ROOT=$CWD/..
NGINX_APP=/usr/local/nginx/nginx

$NGINX_APP -p $APP_ROOT -c conf/nginx.conf
echo "nginx started!"
```
#### 关闭nginx
touch stop
```bash
#!/bin/bash

CWD=`pwd`
APP_ROOT=$CWD/..

kill -QUIT $( cat $APP_ROOT/logs/nginx.pid )

echo "nginx stopped!"
```

#### 重新载入配置文件
touch reload
```
#!/bin/bash

CWD=`pwd`
APP_ROOT=$CWD/..

kill -HUP $( cat $APP_ROOT/logs/nginx.pid )
echo "nginx reloaded!"
```
#### 分割日志
touch splite
```bash
#!/bin/bash

#crontab -e
#05 00 * * * /root/nginx/bin/splitlog
#

APPROOT=$HOME/nginx/logs

ACCLOG=$APPROOT/access.log
ERRLOG=$APPROOT/error.log
ACCLOG_DATE=$APPROOT/access_$(date -d "yesterday" +"%Y%m%d").log
ERRLOG_DATE=$APPROOT/error_$(date -d "yesterday" +"%Y%m%d").log

mv $ACCLOG $ACCLOG_DATE
mv $ERRLOG $ERRLOG_DATE

kill -USR1 $( cat $APPROOT/nginx.pid )
```

#### 配置工具执行权限
cd /root/nginx/bin
chmod 700 *

### 添加nginx配置文件
```
user  root root;
worker_processes  2;

pid /root/nginx/logs/nginx.pid;

events {
     worker_connections 1024;
}

http {
    server {
        listen          80;
        server_name     www.domain.com;
        location / {
            index index.html;
            root  /root/www/domain.com;
        }
    }
}
```

### 创建web资源

```sh
mkdir -p /root/www/domain.com
echo "Hello Nginx" > /root/www/domain.com/index.html
```

### 运行nginx
cd /root/nginx/bin
./start

### 查看nginx运行状态
ps -ef|grep nginx

### 关闭nginx
cd /root/nginx/
./stop
