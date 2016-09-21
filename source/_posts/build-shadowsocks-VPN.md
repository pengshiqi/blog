---
title: 翻墙教程：用国外VPS搭建自己的Shadowsocks服务器
date: 2016-09-21 16:22:57
tags:
---

作为一个百度黑，翻墙是生存下来的必要手段，之前一年都是购买的shadowsocks的代理服务，99元一年。前不久申请了GitHub Student Pack,听说里面有赠送免费的VPS，于是就自己动手搭建了一个海外服务器，来提供翻墙之用。

<!-- more -->

首先去申请[Github Student Pack](https://education.github.com/pack),用国内大学的edu邮箱就可以申请。里面有很多东西，Github 的 private repository、在namecheap一年免费的.me域名、以及[DigitalOcean](https://cloud.digitalocean.com/droplets)的50美元优惠券。然后去DigitalOcean就可以搭建自己的服务器啦。

注册DigitalOcean的时候需要绑定信用卡，或者是绑定PayPal，注册完成后就可以购买服务器啦。我购买的是5美元一个月的、旧金山的服务器，512MB Memory，20GB SSD。

然后就是配置服务器了。

#### SSH连接到服务器

```bash
ssh root@你的服务器 ip
```

初始密码会发到你邮箱。

如果没有，在 DigitalOcean 选中你的服务器，Access —— Reset Root Password，密码会再次发送到你邮箱。

第一次登陆需要修改密码

#### 安装与配置Shadowsocks

```bash
sudo apt-get update
```

更新apt-get

```bash
apt-get install python-pip
```

安装pip包管理工具

```bash
pip install shadowsocks
```

安装shadowsocks

```bash
vi /etc/shadowsocks.json
```

进入vi模式来修改配置文件

```bash
{
    "server":" 服务器 ip",
    "server_port":端口,
    "local_address":"127.0.0.1",
    "local_port":1080,
    "password":" 连接密码 ",
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open":false
}
```

按i进入编辑模式，esc退出编辑模式，修改完成后，退出编辑模式，输入:wq保存退出，:q!不保存退出。

应用配置文件启动服务。若需停止则把 start 换成 stop

大功告成！！！梯子已准备完毕！！

#### shadowsocks的使用

下载一个shadowsocks客户端，输入自己的配置参数，就可以使用啦！
