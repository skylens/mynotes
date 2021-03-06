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
$ ssh-keygen -t ed25519 -C "xxxx@mail.com"
$ ssh-keygen -t ecdsa -b 521 -C "xxxx@mail.com" 
$ ssh-keygen -t rsa -b 4096 -C "xxxx@mail.com"  //生成的`id_rsa.pub`可以放到`coding、github、oschina`上
$ ssh-keygen -f cloud.pem  //生成名为 cloud.pem 的密钥对
$ ssh-copy-id -i <-i指定文件路径> <user>@<ip>
```

**3.ssh 配置**

在`.ssh`目录下添加`config`

```bash
$ vim .ssh/config
Host 主机别名
    HostName 主机名或者ip
    Port 端口
    User 用户名
    IdentityFile 密钥文件的路径
$ ssh 主机别名
```

例子: 为 git 账号指定密钥

```bash
$ vim .ssh/config
host git.coding.net
    HostName git.coding.net
    User git
    IdentityFile ~/.ssh/git.pem
```

**4.[`proxychains4`](https://github.com/rofl0r/proxychains-ng)+ [`shadowsocks`](https://github.com/shadowsocks/shadowsocks/wiki/Shadowsocks-%E4%BD%BF%E7%94%A8%E8%AF%B4%E6%98%8E)代理上网**

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
$ sudo vim /etc/proxychains.conf
再末尾加上  socks5 127.0.0.1 1080
```

+ ubuntu 安装 shadowsocks

```bash
$ sudo apt-get install python-pip
$ sudo pip install shadowsocks
```

+ [图形界面版](https://github.com/shadowsocks/shadowsocks-qt5)

```bash
$ sudo add-apt-repository ppa:hzwhuang/ss-qt5
$ sudo apt-get update
$ sudo apt-get install shadowsocks-qt5
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

**socks5 转 http**

+ 安装

```bash
sudo pacman -S privoxy
```

+ 配置

```bash
sudo vim /etc/privoxy/config
...
listen-address 192.168.1.1:8118

forward-socks5 / localhost:1080
...
```

+ 启动服务

```bash
sudo systemctl start privoxy.service
sudo systemctl status privoxy.service
```

**5.ubuntu 下删除 PPA 源**

```bash
$ sudo add-apt-repository --remove ppa:PPA_Name/ppa
$ ls -al /etc/apt/sources.list.d
$ sudo rm -i /etc/apt/sources.list.d/PPA_Name.list
```

**6.解压命令**

+ zip/unzip

```bash
$ zip -r test.zip test/  //压缩 test 目录
$ unzip -v test.zip  //查看 test.zip 中的文件，但不解压
$ unzip -t test.zip  //查看 test.zip 的完整性
$ unzip test.zip  //解压 test.zip
$ unzip test.zip -d testd/  //解压 test.zip 到 testd 目录
```

*Linux下解压zip中文乱码问题*

- 方法一: 使用 `unzip` 指定字符集解压

```bash
unzip -O CP936 xxx.zip (CP936、GBK、GB18030都可以)
```

- 方法二: 使用 `unar` 解压

```bash
unar xxx.zip
```

- 方法三: 为 `p7zip` 打补丁，使用 `p7zip` 解压

```bash
sudo pacman -S p7zip-natspec
7za x LaTeX2e_manual.zip
```

+ 7z

```bash
$ sudo yum install p7zip
$ brew install p7zip

$ 7za a -t7z test.7z test/   //压缩
$ 7za x test.7z  //解压缩
```

+ tar

```bash
$ tar -cvf test.tar test/  //打包 test 目录
$ tar -xvf test.tar  //解压 test.tar
$ tar -tvf test.tar  //查看 test.tar 文件里的内容，但不解压( *.tar.gz 加 -z , *.tar.bz2 加 -j )
$ tar -zcvf test.tar.gz test/  //打包 test 目录，并且以 gz 格式压缩
$ tar -jcvf test.tar.bz2 test/  //打包 test 目录，并且以 bz2 格式压缩
$ tar -zxvf test.tar.gz  //解压 test.tar.gz test.tgz
$ tar -jxvf test.tar.bz2  //解压 test.tar.bz2
```

+ tar.xz

```bash
$ xz -d test.tar.xz  //先解压 xz 格式
$ tar -xvf test.tar  //再解包 tar 格式
```

**7.查看端口占用**

```bash
$ netstat -tln  //查看哪些端口被监听了
$
```

**8.递归删除文件**

很多级目录中都有相同的文件，我们想把它们都删除

```bash
$ find . -name "要删的文件名" -exec ls {} \;   //这样可以查看
$ find . -name "要删的文件名" -exec rm {} \;   //把ls替换为rm就能实现递归删除了
```

**9.lsof**

```bash
$ lsof -i :389
$ ps ef | grep ldap | grep -v grep
$ nesttat -tunlp | grep 389
```

**10.修改默认 shell**

```shell
$ echo $0   //查看当前 shell
$ whereis bash   //查看 bash shell 的位置
$ chsh -s /usr/local/bin/bash  //修改默认 shell 为 bash shell , 重新登陆生效
$ exit
```
