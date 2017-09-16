# LNMP 环境搭建

### ubuntu server 14.04 LNMP 服务搭建

1.安装nginx

```bash
$ sudo apt-get install -y nginx
```

2.安装php5-fpm

```bash
$ sudo apt-get install php5-fpm
```

3.配置php

```bash
$ sudo vim /etc/php5/fpm/php.ini

把 cgi.fix_pathinfo=1 改为 cgi.fix_pathinfo=0
$ sudo service php5-fpm restart
```

4.配置nginx

```bash
$ sudo vim /etc/nginx/sites-available/default

...
index index.html index.htm index.php;
...
location ~ \.php$ {
                fastcgi_split_path_info ^(.+\.php)(/.+)$;
        #       # NOTE: You should have "cgi.fix_pathinfo = 0;" in php.ini
        #
        #       # With php5-cgi alone:
        #       fastcgi_pass 127.0.0.1:9000;
        #       # With php5-fpm:
                fastcgi_pass unix:/var/run/php5-fpm.sock;
                fastcgi_index index.php;
                include fastcgi_params;
        }
  
$ sudo service nginx restart 
```