# 其他

**1.[Sublime text]()**

    * [package control](https://packagecontrol.io/)


    * 插件
      ![link](http://www.tuicool.com/articles/E3a2Qni)

        ChineseLocalizations  汉化
        ConvertToUTF8  中文乱码
        Emmet   html代码补全
        CSS3   CSS3代码高亮
        Color Highlighter  css颜色高亮显示
        Doc Blockr  代码块注释
    * [linux 中文输入](http://www.jianshu.com/p/bf05fb3a4709)


**2.[NetEase-MusicBox](https://github.com/darknessomi/musicbox) 命令行版[网易云音乐](http://music.163.com/#/my/)**

    * ubuntu 16.04 LTS 安装
    ​```bash
    $ sudo apt-get install python-pip
    $ sudo pip install NetEase-MusicBox
    $ sudo apt-get install mpg123  //Linux/Unix 上的 mp3 播放器(必须安装)
    $ sudo apt-get install aria2  //用于缓存歌曲(有可以用来替换 wget )
    ​```

    * 使用
    ​```bash
    $ musicbox  //vi 快捷键操作 'Space' == 暂停和播放
    ​```

**3.PyCharm**

*    ubuntu 16.04 LTS 安装

   手动安装

   ```bash
   $ tar -zxvf pycharm-community-2016.3.2.tar.gz
   $ mv pycharm-community-2016.3.2/ pycharm/
   $ sudo mv pycharm/ /opt/
   $ sudo vim /usr/share/applications/Pycharm.desktop
     [Desktop Entry]
     Type=Application
     Name=Pycharm
     GenericName=Pycharm3
     Comment=Pycharm3:The Python IDE
     Exec="/opt/pycharm/bin/pycharm.sh" %f
     Icon=/opt/pycharm/bin/pycharm.png
     Terminal=pycharm
     Categories=Pycharm;
   ```

   添加 PPA 安装

   ```bash
   $ sudo add-apt-repository ppa:mystic-mirage/pycharm
   $ sudo apt update
   $ sudo apt install pycharm-community
   ```