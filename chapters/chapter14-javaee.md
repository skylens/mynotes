# JavaEE

### ubuntu 16.04 LTS 上搭建 `eclipse-jee` 开发环境

+ [下载](https://www.eclipse.org/downloads/eclipse-packages/)

  选择对应的版本 `Eclipse IDE for Java EE Developers`

+ 安装

  ```bash
  $ sudo apt-get install openjdk-8-jdk openjdk-8-jre -y  //安装 jdk
  $ sudo apt-get install mysql-server -y  //安装 mysql
  $ wget http://mirrors.hust.edu.cn/apache/tomcat/tomcat-8/v8.5.12/bin/apache-tomcat-8.5.12.zip
  $ eclipse-jee-neon-3-linux-gtk-x86_64.tar.gz
  $ tar -zxvf eclipse-jee-neon-3-linux-gtk-x86_64.tar.gz
  $ cd eclipse-jee-neon-3/
  $ ./eclipse-inst
  ```

+ [汉化](http://www.eclipse.org/babel/downloads.php)

  打开 help ==> Install new software , 添加 `p2` 源 `http://download.eclipse.org/technology/babel/update-site/R0.14.1/neon` ，等待几分钟，选择简体中文安装即可，然后重启
  
+ tomcat 设置




