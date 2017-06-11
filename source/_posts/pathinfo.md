---
title: nginx配置pathinfo方法
date: 2017-6-3 14:48
tags: [nginx,php,thinkphp,pathinfo] 
---

### 什么是pathinfo?
要想在nginx下配pathinfo,首先要先搞清楚什么是pathinfo?大家都知道一般我们访问php后台文件的时候,路径一般都是http://localhost/xxx.php, 假设我们的路径是 http://localhost/xxx ,而这时如果没有配置pathinfo的话,屏幕就会显示404,就是找不到该文件或该文件不存在。在thinkphp5框架中的路径就是这种模式。有些朋友可能会问：在apache下面没有配置pathinfo怎么没有出现问题。这是因为apache已经默认配置了pathinfo。说了这么多，那究竟什么是pathinfo呢？

首先要说明一点，pathinfo不是nginx的功能，pathinfo是php的功能。php中有两个pathinfo，一个是环境变量$_SERVER['PATH_INFO']；另一个是pathinfo函数，pathinfo() 函数以数组的形式返回文件路径的信息;。nginx能做的只是对$_SERVER['PATH_INFO]值的设置。

### nginx配置pathinfo
下面是一个完整的配置
``` bash
server {
    listen 80;
    listen [::]:80;

    # listen [::]:443 ssl http2;
    # listen 443 ssl http2;

    # include ssl.conf;
    # ssl_certificate /path/to/crt;
    # ssl_certificate_key /path/to/key;

    root /var/www; #网站根目录
    index index.html index.htm index.php;

    server_name localhost; #域名

    location / {
        index  index.html index.htm index.php;
        if (!-e $request_filename) {                
            rewrite  ^/(.*)$ /index.php?s=$1  last; 
            break;                                 
        } 
    }

    location /phpmyadmin {
       index index.php;
    }

    location ~ \.php$ {
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_pass unix:/var/run/php/php7.0-fpm.sock; #php7.0版本
        fastcgi_index index.php;
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_param PATH_INFO $request_uri;
    }

    location ~ /\.ht {
        deny all;
    }
}
```
只需要修改把下面三个地方修改成对应的东西即可
> 1. root /var/www; #网站根目录
> 2. server_name localhost; #域名
> 3. fastcgi_pass unix:/var/run/php/php7.0-fpm.sock; #php7.0版本
