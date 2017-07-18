# LDAP 

### debian 安装配置

1.安装

```
# apt-get install slapd ldap-utils phpldapadmin -y
```

2.配置

```
# vim /etc/
```

### ubuntu server 安装配置

1.安装

```
$ sudo apt-get install slapd ldap-utils phpldapadmin -y
```

2.配置

```
$ su -
# cd /etc/ldap/
# vim ldap.conf
做如下修改（ubuntu.io为域名）

#
# LDAP Defaults
#

# See ldap.conf(5) for details
# This file should be world readable but not world writable.

#BASE	dc=example,dc=com
#URI	ldap://ldap.example.com ldap://ldap-master.example.com:666
BASE	dc=ubuntu,dc=io
URI	ldap://localhost:389

#SIZELIMIT	12
#TIMELIMIT	15
#DEREF		never

# TLS certificates (needed for GnuTLS)
TLS_CACERT	/etc/ssl/certs/ca-certificates.crt

# dpkg-reconfigure slapd 

No
ubuntu.io
ubuntu.io
654321
654321
BDB
No
Yes
No

# cd /etc/phpldapadmin/
# vim config.php
做如下修改（192.168.42.152为本机IP地址，ubuntu.io为域名）

$servers->setValue('server','host','192.168.42.152');
$servers->setValue('server','base',array('dc=ubuntu,dc=io'));
$servers->setValue('login','bind_id','cn=admin,dc=ubuntu,dc=io');

# /etc/init.d/apache2 restart
浏览器访问"192.168.42.152/phpldapadmin/"
```

### centos 安装配置

```
$ sudo yum install openldap-servers openldap-clients phpldapadmin-y
```
