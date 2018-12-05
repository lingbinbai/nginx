# 概述
nginx 有三种安装方式：
1. 使用平台安装工具进行安装。
2. 使用二进制包。
3. 源代码安装。

一般来说，在生产环境上我们会选择源代码安装。因为可自定义nginx模块。本文也只介绍源代码安装。

# 预安装nginx运行环境
ububtu平台编译环境可以使用以下指令

```
apt-get install build-essential
apt-get install libtool
apt-get install openssl
apt-get install libssl-dev
```

centos平台编译环境使用如下指令
```
yum -y install gcc automake autoconf libtool make
yum install gcc gcc-c++
yum -y install openssl openssl-devel
```

#正式开始安装
1. 选定源码目录
可以是任何目录，本文选定的是/usr/local/src
```
cd /usr/local/src
```
2. 安装PCRE库
ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/ 下载最新的 PCRE 源码包，使用下面命令下载编译和安装 PCRE 包：
注意：暂时不要使用pcre2，否则nginx会安装失败.
```
cd /usr/local/src
wget ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/pcre-8.38.tar.gz
tar zxvf pcre-8.38.tar.gz
cd pcre-8.38
./configure
make
make install
```

3. 安装zlib库
http://zlib.net 下载最新的 zlib 源码包，使用下面命令下载编译和安装 zlib包：
```
cd /usr/local/src

wget http://zlib.net/zlib-1.2.11.tar.gz
tar -zxvf zlib-1.2.11.tar.gz
cd zlib-1.2.11
./configure
make
make install
```

4. 安装ssl（某些vps默认没装ssl)

```
cd /usr/local/src
wget https://www.openssl.org/source/openssl-1.0.2q.tar.gz
tar zxvf openssl-1.0.2q.tar.gz
cd openssl-1.0.2q/
./config
make
make install
```
5. 安装nginx
Nginx 一般有两个版本，分别是稳定版和开发版，您可以根据您的目的来选择这两个版本的其中一个，下面是把 Nginx 安装到 /usr/local/nginx 目录下的详细步骤：

```
cd /usr/local/src
wget http://nginx.org/download/nginx-1.13.12.tar.gz
tar zxvf nginx-1.13.12.tar.gz
cd nginx-1.13.12/

./configure --sbin-path=/usr/local/nginx/nginx \
--conf-path=/usr/local/nginx/nginx.conf \
--pid-path=/usr/local/nginx/nginx.pid \
--with-http_ssl_module \
--with-pcre=/usr/local/src/pcre-8.38 \
--with-zlib=/usr/local/src/zlib-1.2.11 \
--with-openssl=/usr/local/src/openssl-1.0.2q

make
make install
```
6. 启动
nginx 默认使用80端口，确保系统的 80 端口没被其他程序占用，运行/usr/local/nginx/nginx 命令来启动 Nginx.
如果要在其他电脑上访问，记得放开80端口.
```
/usr/local/nginx/nginx
netstat -ano|grep 80
ps -ef|grep nginx
```
