## 创建用户密码

1. 安装

```
# Debian，Ubuntu
apt install apache2-utils

# RHEL / CentOS / Oracle Linux 安装 httpd-tools
```

2. 创建用户

```
# 第一次创建需要带上 -c 参数
# 如果没有/etc/apache2目录可以先手动创建
sudo htpasswd -c /etc/apache2/.htpasswd user1
sudo htpasswd /etc/apache2/.htpasswd user2
```

3.查看生成的文件

```
dong@dong:~$ cat /etc/apache2/.htpasswd
user1:$apr1$tHDCn.3J$cb8PbjVEEgXyyg08Rd4TJ.
user2:$apr1$wiOGlgfd$lOyuwqBJL5weLXFMCXb040
```

## 配置 nginx

修改 nginx.conf

```
location / {
    auth_basic "Administrator’s Area";      # 开启认证
    auth_basic_user_file /etc/apache2/.htpasswd;    # 上面指定的密码文件
    root   html;
    index  index.html index.htm;
}
```

重启

```
./nginx -s reload
```

## 访问

浏览器中打开 [http://localhost/](http://localhost/)

![](https://user-gold-cdn.xitu.io/2020/3/28/1711e85049527029?w=462&h=239&f=png&s=7065)

## 配合 IP 使用

使用 allow 和 deny 允许或拒绝特定 IP 地址的访问  
satisfy 指令使用

- all 要同时满足 ip 和 Authentication 才可以访问
- any 只要满足一个条件就可以访问

## 实例

指定 IP 不用密码访问，其他 IP 需要使用密码

```
location / {
    satisfy any;        # 满足其中一个条件即可
    allow 127.0.0.1;    # 不用密码就可以直接访问
    deny all;           # 其他IP都需要密码访问
    auth_basic "Administrator’s Area";
    auth_basic_user_file /etc/apache2/.htpasswd;
    root   html;
    index  index.html index.htm;
}
```

指定 IP 和使用密码访问，其他的都不可以访问

```
location / {
    satisfy all;        # 需求满足以下两个条件
    allow 127.0.0.1;    # 只有127.0.0.1和提供密码才可以访问
    deny all;           # 其他IP都不能访问，直接返回403
    auth_basic "Administrator’s Area";
    auth_basic_user_file /etc/apache2/.htpasswd;
    root   html;
    index  index.html index.htm;
}
```

可以通过 access.log 查看访问的 IP

```
tail -n 5 -f access.log
```

参考:

- [https://docs.nginx.com/nginx/admin-guide/security-controls/configuring-http-basic-authentication/](https://docs.nginx.com/nginx/admin-guide/security-controls/configuring-http-basic-authentication/)
