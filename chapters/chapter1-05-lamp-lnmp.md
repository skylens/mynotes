# LAMP/LNMP 服务

## LAMP 服务

### CentOS 7 LAMP 服务搭建

1.安装 `httpd php mariadb`

```bash
# yum install -y httpd php mariadb-server mariadb
```

2.配置 `httpd php`

```bash
# systemctl start httpd  //启动 Apache 服务
# systemctl status httpd  //查看 Apache 服务状态
// 浏览器输入 CentOS 7 的 IP 验证 Apache
# vim /etc/www/html/test.php
<?php phpinfo(); ?>

// 浏览器输入 CentOS 7 的 IP 验证 PHP
```

3.Mariadb 数据库配置

```bash
# mysql_secure_installation
# mysql -u root -p  //测试登录

```

4.基于 LAMP 安装 owncloud

```bash
# yum install php-mysql sqlite php-dom php-mbstring php-gd php-pdo
# wget https://download.owncloud.org/community/owncloud-9.1.3.zip
# unzip owncloud-9.1.3.zip
# cd owncloud/
# cp -r * /var/www/html/
# cd /var/www/
# chown -R apache:apache html/
# mysql -u root -p 
MariaDB [(none)]> create database owncloud;
//创建 owncloud 数据库
MariaDB [(none)]> CREATE USER 'owncloud'@'localhost'IDENTIFIED BY 'owncloudadmin';
//创建 owncloud 用户, 密码为 owncloudadmin
MariaDB [(none)]> grant all privileges on owncloud.* to 'owncloud'@localhost identified by 'owncloudadmin' with grant option;
//授权用户 owncloud 拥有数据库 owncloud 的所有权限允许本地访问
MariaDB [(none)]> flush privileges;
//刷新系统权限表
MariaDB [(none)]> quit;

//浏览器输入 CentOS 7 的 IP 安装 owncloud , 刷新登录

//通过 ngrok.cc 外网访问内网
# vim /var/www/html/config/config.php  //添加

'trusted_domains' =>
array (
   'demo.example.org',
   'otherdomain.example.org',
),

# ./sunny clientid 客户端id
//浏览器输入 sunny 生成的地址
```

## LNMP 服务