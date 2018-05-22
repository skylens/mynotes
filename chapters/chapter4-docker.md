# Docker

[一般安装](https://docs.docker.com/install/)

使用 `get-docker.sh` 脚本安装

```sh
# curl -fsSL get.docker.com -o get-docker.sh
# sh get-docker.sh --mirror Aliyun
```

*`docker`链接*

[官方安装文档](https://docs.docker.com/engine/installation/linux/)

[中国官方文档](https://docs.docker-cn.com/)

[Docker Hub 镜像中心](https://hub.docker.com/)

[daocloud 加速器](https://www.daocloud.io/mirror#accelerator-doc)

[daocloud 加速器文档](http://guide.daocloud.io/dcs/daocloud-9153151.html)

[蜂巢镜像中心](https://c.163.com/hub#/m/home/)

[阿里云参考](https://yq.aliyun.com/articles/7695?spm=5176.100239.blogcont29941.14.ZE3kQk)


*`docker`设置*

- 设置加速器

[阿里云](https://cr.console.aliyun.com/#/accelerator)

[docker 中国](http://docker-cn.com/registry-mirror)

```shell
# vim /etc/docker/daemon.json
{
  "registry-mirrors": ["https://registry.docker-cn.com"]
}
```

- 不用`sudo`执行`docker`命令

```shell
$ sudo usermod -aG docker your_username   //your_username 普通用户用户名
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

- 运行镜像并映射端口

`docker run -it -p 81:4000 ruby bash`

- 运行镜像并映射本地目录

`docker run -it -v /opt/docker/:/opt/ ruby bash`

- 后台运行容器

`docker run -d ubuntu`

- 进入后台运行的容器

`docker attach 33d032089db6` 连接终止，自动退出后台

`docker exec -it 33d032089db6 /bin/sh`  不会退出后台

- 修改镜像并提交到本地

`docker run -it debian bash`  使用shell作修改

`docker commit -m '提交说明' 797ff13c1047 debian:9.0.01`


