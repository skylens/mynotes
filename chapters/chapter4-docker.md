# Docker

*ubuntu*

[ubuntu 安装](https://docs.docker.com/engine/installation/linux/ubuntulinux/)

```shell
$ sudo apt-get update
$ sudo apt-get install apt-transport-https ca-certificates
$ sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
$ sudo echo "deb https://apt.dockerproject.org/repo ubuntu-trusty main" > /etc/apt/sources.list.d/docker.list
$ sudo apt-get update
$ sudo apt-get install docker-engine
$ sudo service docker start
$ sudo docker run hello-world
```


*`docker`链接*

[官方安装文档](https://docs.docker.com/engine/installation/linux/)

[Docker Hub 镜像中心](https://hub.docker.com/)

[daocloud 加速器](https://www.daocloud.io/mirror#accelerator-doc)

[daocloud 加速器文档](http://guide.daocloud.io/dcs/daocloud-9153151.html)

[蜂巢镜像中心](https://c.163.com/hub#/m/home/)

[阿里云参考](https://yq.aliyun.com/articles/7695?spm=5176.100239.blogcont29941.14.ZE3kQk)


*`docker`设置*

- 设置加速器

```shell
# vim /etc/docker/daemon.json
{
    "registry-mirrors": [
        "加速地址"
    ],
    "insecure-registries": []
}
```


*`docker` 命令*

- 测试

`docker run hello-world`

- 拉取镜像(ubuntu为镜像名)

`docker pull ubuntu`

- 列出镜像

`docker images`

- 运行镜像并激活`shell`

`docker run -it ubuntu bash`

- 后台运行容器

`docker run -d ubuntu`

- 进入后台运行的容器

`docker attach 33d032089db6` 连接终止，自动退出后台

`docker exec -it 33d032089db6 /bin/sh`  不会退出后台

