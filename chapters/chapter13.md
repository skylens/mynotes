# Raspberrypi 树莓派

### [下载](https://www.raspberrypi.org/downloads/)

+ NOOBS

+ RASPBIAN

+ 其他

### 安装

+ Linux 下
  
  先查看 SD 卡的文件格式，不是 FAT32 的需要格式化为 FAT32 ,安装 [etcher](https://github.com/resin-io/etcher#installers)

  ![etcher](../pictures/etcher_ubuntu.png)

+ windows 下

  先查看 SD 卡的文件格式，不是 FAT32 的需要格式化为 FAT32 ,下载 [win32DiskImager](https://sourceforge.net/projects/win32diskimager/) 并安装 
  
  ![win32diskimager](../pictures/win32DiskImager.png)

+ macOS 下

  先查看 SD 卡的文件格式，不是 FAT32 的需要格式化为 FAT32 ,下载 [etcher](https://etcher.io/) 并安装
 
  ![etcher](../pictures/etcher_macOS.png)

### 配置(以`RASPBIAN`为例)

+ 将镜像刷到 SD 卡之后，在 `boot` 目录下新建一个目录 `ssh` 

  2016.11.25日更新的系统默认不开启 `ssh` ，无显示器安装无法正常进行，解决方法就是在 `boot` 目录下新建一个目录 `ssh` 

+ 默认的用户名和密码

  用户名：```pi```
  
  密 码：```raspberry```
  
+ [Raspberrypi源](http://shumeipai.nxez.com/2013/08/31/raspbian-chinese-software-source.html) 改用国内的源加快下载安装软件速度

  ```
  $ sudo nano /etc/apt/sources.list
  注释掉每一行的代码（也就是在前面打 #），添加
    deb http://mirrors.ustc.edu.cn/raspbian/raspbian/ jessie main contrib non-free
  ```

+ `raspi-config` 配置

  ```
  $ sudo raspi-config
  ```

### 安装 `jdk` 

### 安装 `tomcat`

### 安装 `mysql`





