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
做如下修改（example.com为域名）

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

# ldapsearch -x -b '' -s base '(objectclass=*)'   //查看根条目
# ldapsearch -x -b "dc=example,dc=com" "(objectclass=*)"
# dpkg-reconfigure slapd 

No
example.com
example.com
secret
secret
BDB
No
Yes
No

# ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/ldap/schema/cosine.ldif
# ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/ldap/schema/nis.ldif
# ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/ldap/schema/inetorgperson.ldif
# vim admin.ldif

dn: dc=example,dc=com
objectClass: dcObject
objectClass: organization
dc: example
o: Example Company

dn: cn=admin,dc=example,dc=com
objectClass: organizationalRole
objectClass: top
cn: admin

# ldapadd -x -D "cn=admin,dc=example,dc=com" -w secret -f admin.ldif
# slapcat 
# cd /etc/phpldapadmin/
# vim config.php
做如下修改（192.168.42.152为本机IP地址，example.com为域名）

$servers->setValue('server','host','192.168.42.152');
$servers->setValue('server','base',array('dc=example,dc=com'));
$servers->setValue('login','bind_id','cn=admin,dc=example,dc=com');

# /etc/init.d/apache2 restart
浏览器访问"192.168.42.152/phpldapadmin/"
```

### centos 安装配置

```
# yum install openldap-servers openldap-clients phpldapadmin-y
# cp /usr/share/openldap-servers/DB_CONFIG.example /var/lib/ldap/DB_CONFIG
# chown ldap:ldap /var/lib/ldap/DB_CONFIG
# systemctl start slapd
# slappasswd -s admin
{SSHA}bxijyOo2PFH6p9eW89JlumN+AXVdWtRg
# vim chrootpw.ldif


# specify the password generated above for "olcRootPW" section
dn: olcDatabase={0}config,cn=config
changetype: modify
add: olcRootPW
olcRootPW: 加密的密码字符串

# ldapadd -Y EXTERNAL -H ldapi:/// -f chrootpw.ldif
# ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/cosine.ldif
# ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/nis.ldif
# ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/inetorgperson.ldif
# vim chdomain.ldif

# replace to your own domain name for "dc=***,dc=***" section
# specify the password generated above for "olcRootPW" section
dn: olcDatabase={1}monitor,cn=config
changetype: modify
replace: olcAccess
olcAccess: {0}to * by dn.base="gidNumber=0+uidNumber=0,cn=peercred,cn=external,cn=auth"
  read by dn.base="cn=admin,dc=example,dc=com" read by * none

dn: olcDatabase={2}hdb,cn=config
changetype: modify
replace: olcSuffix
olcSuffix: dc=example,dc=com

dn: olcDatabase={2}hdb,cn=config
changetype: modify
replace: olcRootDN
olcRootDN: cn=admin,dc=example,dc=com

dn: olcDatabase={2}hdb,cn=config
changetype: modify
add: olcRootPW
olcRootPW: {SSHA}bxijyOo2PFH6p9eW89JlumN+AXVdWtRg

dn: olcDatabase={2}hdb,cn=config
changetype: modify
add: olcAccess
olcAccess: {0}to attrs=userPassword,shadowLastChange by
  dn="cn=admin,dc=example,dc=com" write by anonymous auth by self write by * none
olcAccess: {1}to dn.base="" by * read
olcAccess: {2}to * by dn="cn=admin,dc=example,dc=com" write by * read

# ldapmodify -Y EXTERNAL -H ldapi:/// -f chdomain.ldif
# vim basedomain.ldif

# Add entry (dc=example,dc=com cn=admin,dc=example,dc=com ou=people,dc=example,dc=com ou=group,dc=example,dc=com)
dn: dc=example,dc=com
objectClass: dcObject
objectClass: organization
dc: example
o: Example Company

dn: cn=admin,dc=example,dc=com
objectClass: organizationalRole
objectClass: top
cn: admin

dn: ou=people,dc=example,dc=com
objectClass: organizationalUnit
ou: people

dn: ou=group,dc=example,dc=com
objectClass: organizationalUnit
ou: group

# ldapadd -x -D "cn=admin,dc=example,dc=com" -w admin -f basedomain.ldif   //admin 是密码
# vim /etc/openldap/ldap.conf
# systemctl restart slapd
# 修改phpldapadmin配置文件
# systemctl restart httpd

# 浏览器访问 http://ip/ldapadmin 不是 http://ip/phpldapadmin
```
