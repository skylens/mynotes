# git 相关

1.git 安装

+ 通过软件仓库安装(但是版本比较低)

```bash
$ sudo apt-get install git  //Debian 系
$ sudo yum install git  // CentOS 系
```

+ 编译安装(获得较新的版本)

```bash
// CentOS 系
sudo yum groupinstall "Development Tools"
sudo yum install gettext-devel openssl-devel perl-CPAN perl-devel zlib-devel wget
// Debian 系
sudo apt-get install libssl-dev libcurl4-gnutls-dev wget
wget --no-check-certificate https://github.com/git/git/archive/v2.16.2.tar.gz
tar -xvf v2.16.2.tar.gz
cd git-*
make configure
./configure --prefix=/usr/local
sudo make install
git --version
```

2.git 使用

+ 常规操作

```bash
$ git init ./  //初始化
$ git add --all  //添加修改
$ git status  //查看 git 的状态
$ git commit -m '修改说明'  //提交修改
$ git push  //推送到当前的分支(一般是主分支) 
```

+ 分支操作

```bash
$ git branch testing  //创建 testing 分支
$ git checkout testing  //切换到 testing 分支
$ git branch -a  //查看本地分支及远程分支
$ git branch  //查看本地分支
$ git branch -d testing  //删除本地 testing 分支
$ git push origin --delete testing  //删除远程 testing 分支
$ git push origin master  //把本地主分支推送到远程主分支
$ git push origin testing   //把 testing 本地分支推送到远程 testing 分支
```

+ 回滚操作

```shell
$ git log  //查看提交历史
$ git reset –hard 8ff24a6803173208f3e606e32dfcf82db9ac84d8  //回滚到某个 commit ( commit 的 hash 值)
```

3.GitHub,Coding,OSChina

+ 使用 `ssh` 克隆 
  
  参考： [coding 帮助](https://coding.net/help/doc/git/ssh-key.html)

 先创建 ssh 公钥和私钥，打开公钥，在 `GitHub,Coding,OSChina` SSH公钥管理页面添加公钥，然后进行测试
  
 `GitHub:  ssh -T git@github.com` 

 `Coding： ssh -T git@git.coding.net`

 `OSChina： ssh -T git@git.oschina.net`
 
 不报错则证明添加公钥成功！最后选择 SSH 方式克隆项目
 
 + gitconfig 配置(设置用户名、用户邮箱等等)
  
  ```
  $ git config --global user.email "skylens116@outlook.com"
  $ git config --global user.name "skylens"
  $ git config --global core.editor vim
  $ git config --global merge.tool vimdiff
  ```
  查看配置
  
  ```
  $ git config --list
  ```

  + gitignore 配置(忽略 git 的文件)
  
  ```bash
  $ vim .gitignore  // .gitignore 文件位于整个目录的第一级目录之下
    *~
    auto
    *.o
    *.svn
    *.aux
    *.log
    *.snm
    *.toc
    *.blg
    *.bbl
    *.vrb
    *.nav
    *.out
    *.lof
    *.lol
    *.lot
    *.bak
    *.tdo
    *.pyg
    *.synctex.gz
    _*
    *.fdb_latexmk
    *.fls
    *.ent
    *.bcf
    *.run.xml
  ```
  
Other

[不要再为代码管理而头疼，GitHub是你最佳的选择！](http://cs.swfu.edu.cn/itf/?p=299)

[学会用 git ](http://cs2.swfc.edu.cn/~wx672/lecture_notes/linux/tutorials/git.html)
