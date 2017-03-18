# VPN



# shdowsocks

在 CentOS 7 上搭建[`shadowsocks`](https://github.com/shadowsocks/shadowsocks/wiki/)服务

[安装脚本]()

```bash
$ sudo yum update -y
$ sudo yum install epel-release -y
$ sudo yum install python-setuptools -y
$ sudo easy_install pip
$ sudo pip install shadowsocks
$ sudo yum isntall libsodium m2crypto -y  //使用 chacha20 加密传输
```

启动服务

```bash
$ sudo ssserver -p 8388 -k mypassword -m chacha20 --user nobody -d start  //后台运行
$ sudo ssserver -d stop  //停止
```