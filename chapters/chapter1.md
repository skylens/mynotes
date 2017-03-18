# GUN/Linux

**1.添加用户**

* `adduser`
```bash
$ sudo adduser admin
```

* [`useradd`](http://man.linuxde.net/useradd)
```bash
$ sudo useradd -m -s /bin/bash admin
$ sudo passwd admin
```

**2.`ssh`免密码登录**

```bash
$ ssh-keygen -t rsa -b 4096 -C "xxxx@mail.com"  //生成的`id_rsa.pub`可以放到`coding、github、oschina`上
$ ssh-copy-id -i <-i指定文件路径> <user>@<ip>
```

**3.[`proxychains4`](https://github.com/rofl0r/proxychains-ng)+ [`shadowsocks`](https://github.com/shadowsocks/shadowsocks/wiki/Shadowsocks-%E4%BD%BF%E7%94%A8%E8%AF%B4%E6%98%8E)代理上网**

+ 安装 `proxychains4` [下载](https://sourceforge.net/projects/proxychains-ng/)
```bash
$ tar -jxvf proxychains-4.10.tar.bz2
$ cd proxychains-4.10
$ ./configure --prefix=/usr --sysconfdir=/etc
$ make
$ sudo make install 
$ sudo make install-config
```
+ 修改配置文件
```bash
$ sudo vim /etc/procxychains.conf
再末尾加上  socks5 127.0.0.1 1080
```
+ ubuntu 安装 shadowsocks
```bash
$ sudo apt-get install python-pip
$ sudo pip install shadowsocks
```
+ [配置文件](https://github.com/shadowsocks/shadowsocks/wiki/Configuration-via-Config-File)
```bash
$ vim node.josn
{
    "server":"my_server_ip",
    "server_port":8388,
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"mypassword",
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": false
}
```
+ 使用方法
```bash
$ sudo sslocal -c node.json (Ctrl + c 结束 shadowsocks 代理)
$ sudo proxychains4 ping www.google.com -c 3
$ sudo proxychains4 apt-get update 
```

**4.ubuntu 下删除 PPA 源**

```bash
$ sudo add-apt-repository --remove ppa:PPA_Name/ppa
$ ls -al /etc/apt/sources.list.d
$ sudo rm -i /etc/apt/sources.list.d/PPA_Name.list
```

**5.解压命令**

+ zip/unzip

```bash
$ zip -r test.zip test/  //压缩 test 目录
$ unzip -v test.zip  //查看 test.zip 中的文件，但不解压 
$ unzip -t test.zip  //查看 test.zip 的完整性
$ unzip test.zip  //解压 test.zip 
$ unzip test.zip -d testd/  //解压 test.zip 到 testd 目录
```

+ tar 

```bash
$ tar -cvf test.tar test/  //打包 test 目录
$ tar -xvf test.tar  //解压 test.tar 
$ tar -tvf test.tar  //查看 test.tar 文件里的内容，但不解压( *.tar.gz 加 -z , *.tar.bz2 加 -j )
$ tar -zcvf test.tar.gz test/  //打包 test 目录，并且以 gz 格式压缩
$ tar -jcvf test.tar.bz2 test/  //打包 test 目录，并且以 bz2 格式压缩
$ tar -zxvf test.tar.gz  //解压 test.tar.gz 
$ tar -jxvf test.tar.bz2  //解压 test.tar.bz2
```

+ tar.xz

```bash
$ xz -d test.tar.xz  //先解压 xz 格式
$ tar -xvf test.tar  //再解包 tar 格式
```