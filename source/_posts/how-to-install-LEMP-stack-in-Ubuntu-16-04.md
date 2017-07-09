---
title: 如何在 Ubuntu 16.04 上安装 LEMP Stack(Linux, Nginx, MySQL, PHP)
date: 2016-11-08 14:50:43
tags: [Linux, Server, MySQL, PHP]
---

## Introduction

LEMP Stack 是一组用来运行动态网站或者服务器的开源软件。它们是 Linux 操作系统，Nginx web服务器，后端数据存储在 MySQL 数据库，动态处理则是由 PHP 来完成。

这篇文章就是来说明如何在 Ubuntu 16.04 的服务器上安装 LEMP Stack。首先你需要有一个Ubuntu操作系统，然后我们来说明后面的部分如何完成。

<!-- more -->

## Prerequisite

在完成这篇教程之前，你需要在你的服务器上有一个普通的、有sodu权限的非root用户账户。如果没有的话，就去看文末参考文献的第一篇教程吧。

然后，用此账户登陆你的服务器，就可以开始配置啦！

## Step 1: Install the Nginx Web Server

首先通过 apt-get 命令安装Nginx：

```
sudo apt-get update
sudo apt-get install nginx
```

如果你后台有运行 *ufw* 防火墙的话，就执行以下命令将 Nginx 服务加入到防火墙的允许列表中:

```
sudo ufw allow 'Nginx HTTP'
```

通过命令来确认结果：

```
sudo ufw status
```

结果应该是:

```
Status: active

To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere                  
Nginx HTTP                 ALLOW       Anywhere                  
OpenSSH (v6)               ALLOW       Anywhere (v6)             
Nginx HTTP (v6)            ALLOW       Anywhere (v6)
```

然后在你的浏览器中输入你的服务器地址，就可以看到 Nginx 的初始界面了：

```
http://server_domain_or_IP
```

![icon](http://obw22u9v2.bkt.clouddn.com/nginx.png)

## Step 2: Install MySQL to Manage Site Data

可以简单的通过 apt-get 命令安装:

```bash
sudo apt-get install mysql-server
```

你将会被询问，要求提供一个root账户的密码用来登录MySQL。

通过以下命令在服务器上进入MySQL服务界面:

```bash
mysql -u root -p
```

输入root账户的密码后，即可进入MySQL交互式界面。

此外，你还可以安装 *VALIDATE PASSWORD PLUGIN* 插件：

```bash
sudo mysql_secure_installation
```

在此就不做过多的介绍了。

## Step 3: Install PHP for Processing

我们需要PHP的服务来处理动态内容，所以我们需要安装 *php-fpm (fastCGI process manager)* ，还是通过apt-get命令安装:

```
sudo apt-get install php-fpm php-mysql
```

#### Configure the PHP Processor

现在php模块已经安装了，我们需要修改一点点配置来保证php服务的安全。打开配置文件:

```
sudo nano /etc/php/7.0/fpm/php.ini
```

我们找到设置 *cgi.fix_pathinfo* 参数的那一行，去掉注释的分号，并将它的值设置为0:

```
cgi.fix_pathinfo=0
```

关闭文件并保存。

现在，我们重启我们的PHP Processor:

```
sudo systemctl restart php7.0-fpm
```

这样我们的改动就会生效了。

## Step 4: Configure Nginx to Use the PHP Processor

现在，我们已经安装了所有必须的模块，最后一项工作就是告诉 Nginx 使用 PHP processor 来处理动态内容。

打开默认的Nginx服务器配置文件:

```
sudo nano /etc/nginx/sites-available/default
```

除去所有的注释，这个文件的内容应是这样：

```
server {
    listen 80 default_server;
    listen [::]:80 default_server;

    root /var/www/html;
    index index.html index.htm index.nginx-debian.html;

    server_name _;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

我们需要针对我们的站点将其修改为如下内容：

<pre>
server {
    listen 80 default_server;
    listen [::]:80 default_server;

    root /var/www/html;
    index <font color=red>index.php</font> index.html index.htm index.nginx-debian.html;

    server_name <font color=red>server_domain_or_IP</font>;

    location / {
        try_files $uri $uri/ =404;
    }
    <font color=red>
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php7.0-fpm.sock;
    }

    location ~ /\.ht {
        deny all;
    }
    </font>
}
</pre>

保存并关闭文件。

可以通过命令来测试你的Nginx是否配置成功：

```
sudo nginx -t
```

如果没有Error，就说明你配置成功了，否则，就需要检测上一步是否有拼写错误之类的。

当你准备好了，就可以重新加载Nginx配置文件:

```
sudo systemctl reload nginx
```

## Step 5: Create a PHP File to Test Configuration

你的 *LEMP Stack* 现在已经搭建完毕，我们可以做个简单的测试来看Nginx是否可以正确使用PHP processor来处理 *.php* 文件。

在我们的文件根目录（document root）下创建一个测试用的php文件，并命名为 *info.php* :

```
sudo nano /var/www/html/info.php
```

它的内容为：

```php
<?php
phpinfo();
```

保存并关闭文件，这段代码将会显示我们服务器的信息。

现在，你可以在浏览器中输入你的服务器IP + */info.php* 来访问这个网页：

```
http://server_domain_or_IP/info.php
```
你将会看到如下网页，显示的是你服务器的信息：

![icon](http://obw22u9v2.bkt.clouddn.com/php_info.png)

这代表着你的测试成功了。

## Conclusion

现在，你在你的 Ubuntu 16.04 的服务器上成功配置了 LEMP Stack，可以在它上面部署你自己的web应用啦~！

PS: 本文来自 reference 的第二篇 Digital Ocean tutorial，粗略地翻译了一下，感觉好蛋疼。。。这么好的英文教程不应该被翻译成中文的。。。下次争取自己写一些英文的博文...TAT

## Reference

* [Initial Server Setup with Ubuntu 16.04](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-16-04)

* [How To Install Linux, Nginx, MySQL, PHP (LEMP stack) in Ubuntu 16.04](https://www.digitalocean.com/community/tutorials/how-to-install-linux-nginx-mysql-php-lemp-stack-in-ubuntu-16-04?utm_source=legacy_reroute)
