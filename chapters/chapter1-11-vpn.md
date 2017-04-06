# VPN  服务搭建



# shdowsocks 服务搭建

**Linux 服务端附带[客户端](../chapters/chapter1.md)**

+ 在 CentOS 7 上搭建[`shadowsocks`](https://github.com/shadowsocks/shadowsocks/wiki/)服务

 ```bash
 $ sudo yum update -y
 $ sudo yum install epel-release -y
 $ sudo yum install python-setuptools -y
 $ sudo easy_install pip
 $ sudo pip install shadowsocks
 $ sudo yum isntall libsodium m2crypto -y  //使用 chacha20 加密传输
 ```

+ 启动服务

 ```bash
 $ sudo ssserver -p 8388 -k mypassword -m chacha20 --user nobody -d start  //后台运行
 $ sudo ssserver -d stop  //停止
 ```
 + 在 Debian/Ubuntu 上搭[`shadowsocks`](https://github.com/shadowsocks/shadowsocks/wiki/)服务
 
 ```bash
$ sudo apt-get install python-pip python-dev python-m2crypto build-essential -y 
$ sudo pip install shadowsocks
$ wget https://github.com/jedisct1/libsodium/releases/download/1.0.8/libsodium-1.0.8.tar.gz //使用 chacha20 加密传输
$ tar xf libsodium-1.0.8.tar.gz && cd libsodium-1.0.8
$ ./configure && make -j2
$ sudo make install
$ sudo ldconfig
 ```