# 在线文档托管

*[GitBook](https://www.gitbook.com/)*

nodejs 环境搭建

```bash
$ sudo apt-get install nodejs
$ sudo ln -s /usr/bin/nodejs /usr/bin/node
$ node -v
$ wget http://npmjs.org/install.sh
$ chmod +x install.sh
$ sudo ./install.sh
$ npm -v 
```

gitbook 安装

```bash
$ sudo npm install gitbook-cli -g
$ gitbook --version
```

gitbook 使用

```bash
$ gitbook init  //gitbook 初始化
$ gitbook serve  //启动 gitbook 服务器预览
```

gitbook build 生产静态网页(在文档所在目录的上一级目录执行gitbook build, 输出到 build 中)

```bash
$ gitbook build ../mynotes ~/build
```

*[gitbook-editor 编辑器](https://www.gitbook.com/editor/) __注意选择对应版本,不要点 Download 直接下载__ *

```bash
$ wget http://downloads.editor.gitbook.com/download/linux-64-bit
$ sudo gdebi gitbook-editor-6.6.2-linux-x64.deb
```

*SoftCover*
