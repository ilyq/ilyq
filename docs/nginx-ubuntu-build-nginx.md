# Ubuntu 编译安装 nginx

## 环境

```
Ubuntu 18.04.2 LTS
```

## 下载源码

```
# 官网下载地址 http://nginx.org/en/download.html
wget http://nginx.org/download/nginx-1.17.9.tar.gz
```

## 解压源码

```
tar xvf nginx-1.16.1.tar.gz
```

查看 config 参数, 里面是一些我们可以添加编译的模块

```
cd nginx-1.17.9/
./configure --help | less

常见模块
--with-http_ssl_module
--without-http_gzip_module
--without-http_proxy_module
```

## 编译

1.  生成 Makefile

```
--prefix:  用于指定安装目录
./configure --prefix=/home/dong/code/nginx

# 如果需要gzip可以这样写
./configure --prefix=/home/dong/code/nginx --without-http_gzip_module
```

2. 编译

```
make
```

3. 安装

```
make install
```

4. 验证目录

```
# 查看安装目录
ls /home/dong/code/nginx
conf  html  logs  sbin
# nginx 在sbin目录下面
```

查看 nginx 版本

```
./nginx -V
nginx version: nginx/1.17.9
built by gcc 7.5.0 (Ubuntu 7.5.0-3ubuntu1~18.04)
configure arguments: --prefix=/home/dong/code/nginx
```

## 访问

1.  启动 nginx

```
# 默认使用80端口，需要管理员权限启动
./nginx
```

2.  访问

```
# 浏览器中访问
http://localhost/
```
