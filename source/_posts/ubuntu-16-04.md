---
title: ubuntu 16.04 搭建lnmp环境 
date: 2017-5-20 14:48
tags: [ubuntu,nginx,php,mysql,phpmyadmin]
---

### 前语

首先切换到root下安装

```
$ sudo -i
```
不一定要在root下，自己视情况而定，这里我是在root下安装

### 安装nginx


``` bash
$ apt-get update
$ apt-get install nginx
```
首先执行 apt-get update 更新软件源，然后安装 apt-get install nginx

这时候检验一下是否搭建成功，在浏览器输入

> localhost

如果浏览器显示 welcome to nginx 则说明nginx已经安装成功

![Alt text](http://upload-images.jianshu.io/upload_images/414166-5f9090e16db9a358.png?imageMogr2/auto-orient/strip%7CimageView2/2)

### 安装mysql

``` bash
$ apt-get install mysql-server
```
安装过程中会让你输入两次密码,这是root密码,一定要记下来,后面安装phpmyadmin会用到

### 安装php7.0

``` bash
$ apt-get -y install php7.0-fpm php7.0-mysql php7.0-curl 
```
安装完之后,要改掉php配置中的 fix_pathinfo 潜在安全漏洞

``` bash
$ vim /etc/php/7.0/fpm/php.ini

```
找到下面这条语句 

> ;cgi.fix_pathinfo=1

改成

> cgi.fix_pathinfo=0

然后重启php,记住修改配置文件一定要重启一下才会生效

``` bash
$ systemctl restart php7.0-fpm
```
下面添加nginx配置文件,以支持php

``` bash
$ vim /etc/nginx/sites-available/default
```
下面是要添加的内容

`````` 
server {
    listen 80;
    listen [::]:80;

    # listen [::]:443 ssl http2;
    # listen 443 ssl http2;

    # include ssl.conf;
    # ssl_certificate /path/to/crt;
    # ssl_certificate_key /path/to/key;

    #server_domain_or_IP改成你的网站目录名称
    root /var/www/server_domain_or_IP;
    index index.html index.htm index.php;

    #server_domain_or_IP改成你的域名或ip
    server_name server_domain_or_IP;

    location / {
        try_files $uri $uri/ =404;
    }

    location /phpmyadmin {
       index index.php;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php7.0-fpm.sock;
    }

    location ~ /\.ht {
        deny all;
    }
}
``````

然后重启nginx

```
$ systemctl restart nginx
```
接下来测试PHP是否已完成安装配置,上面nginx配置root /var/www/html;

```
$ vi /var/www/html/info.php
```
添加以下内容

```
<?php
    phpinfo();
?>
```
在浏览器中输入

>localhost/info.php

出现下图则说明安装成功

![Alt text](http://upload-images.jianshu.io/upload_images/414166-0b8673bf19fa6c5c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 安装phpMyAdmin

```
$ apt-get update
$ apt-get install phpmyadmin php-mbstring php-gettext
```
创建phpmyadmin链接

```
$ ln -s /usr/share/phpmyadmin/  /var/www/html/
```
重启nginx

```
$ systemctl restart nginx
```
在浏览器重输入

>localhost/phpmyadmin

出现下图则说明安装成功

![Alt text](http://upload-images.jianshu.io/upload_images/414166-38361bfd5ed9e2ab.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 注意事项
1. vim /etc/php/7.0/fpm/php.ini 可能会提示您没有安装vim,安装就行,安装命令 apt-get install vim
2. vi和vim编辑文件,首先按 i 开始编辑,编辑完之后 按Esc键,然后按 shift+: 最后输入wq保存




