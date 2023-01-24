---
title: 翻墙教程：用国外VPS搭建自己的Shadowsocks服务器
date: 2016-09-21 16:22:57
tags: 
- Shadowsocks
categories:
- 教程
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

启动 shadowsocks 服务器的命令如下:

```bash
ssserver -c /etc/shadowsocks.json
```

后台启动和停止shadowsocks服务器:

```
ssserver -c /etc/shadowsocks.json -d start
ssserver -c /etc/shadowsocks.json -d stop
```

大功告成！！！梯子已准备完毕！！

#### shadowsocks的使用

下载一个shadowsocks客户端，输入自己的配置参数，就可以使用啦！



----



🌀 ***2018-11-19 14:41 Update***

今天又去新开一个droplet，按上面的步骤操作，结果报错了...

报错信息：

```
root@ubuntu-s-1vcpu-1gb-sfo2-01:~# ssserver -c /etc/shadowsocks.json
INFO: loading config from /etc/shadowsocks.json
2018-11-19 06:29:00 INFO     loading libcrypto from libcrypto.so.1.1
Traceback (most recent call last):
  File "/usr/local/bin/ssserver", line 11, in <module>
    sys.exit(main())
  File "/usr/local/lib/python2.7/dist-packages/shadowsocks/server.py", line 34, in main
    config = shell.get_config(False)
  File "/usr/local/lib/python2.7/dist-packages/shadowsocks/shell.py", line 262, in get_config
    check_config(config, is_local)
  File "/usr/local/lib/python2.7/dist-packages/shadowsocks/shell.py", line 124, in check_config
    encrypt.try_cipher(config['password'], config['method'])
  File "/usr/local/lib/python2.7/dist-packages/shadowsocks/encrypt.py", line 44, in try_cipher
    Encryptor(key, method)
  File "/usr/local/lib/python2.7/dist-packages/shadowsocks/encrypt.py", line 83, in __init__
    random_string(self._method_info[1]))
  File "/usr/local/lib/python2.7/dist-packages/shadowsocks/encrypt.py", line 109, in get_cipher
    return m[2](method, key, iv, op)
  File "/usr/local/lib/python2.7/dist-packages/shadowsocks/crypto/openssl.py", line 76, in __init__
    load_openssl()
  File "/usr/local/lib/python2.7/dist-packages/shadowsocks/crypto/openssl.py", line 52, in load_openssl
    libcrypto.EVP_CIPHER_CTX_cleanup.argtypes = (c_void_p,)
  File "/usr/lib/python2.7/ctypes/__init__.py", line 379, in __getattr__
    func = self.__getitem__(name)
  File "/usr/lib/python2.7/ctypes/__init__.py", line 384, in __getitem__
    func = self._FuncPtr((name_or_ordinal, self))
AttributeError: /usr/lib/x86_64-linux-gnu/libcrypto.so.1.1: undefined symbol: EVP_CIPHER_CTX_cleanup
```



查了一下， 发现原因是：

`Ubuntu 18.04下由于升级 openssl 至1.1.0以上版本导致的 shadowsocks 服务出现 undefined symbol: EVP_CIPHER_CTX_cleanup 错误而无法启动的问题`。

这是由于在openssl 1.1.0中废弃了 `EVP_CIPHER_CTX_cleanup()` 函数而引入了 `EVE_CIPHER_CTX_reset()` 函数所导致的。

因此，可以通过将 `EVP_CIPHER_CTX_cleanup()` 函数替换为 `EVP_CIPHER_CTX_reset()` 函数来解决该问题。具体解决方法如下：

1、根据错误信息定位到文件 `/usr/local/lib/python2.7/dist-packages/shadowsocks/crypto/openssl.py` 。

2、搜索 cleanup 并将其替换为 reset 。（我这个版本中有两处，分别是在52行和111行。）

3、重新启动 shadowsocks, 该问题解决。

