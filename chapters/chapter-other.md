# 其他

**1.[Sublime text]()**

    * [package control](https://packagecontrol.io/)
        
        sublime text 3 
        
        ```
        import urllib.request,os,hashlib; h = 'df21e130d211cfc94d9b0905775a7c0f' + '1e3d39e33b79698005270310898eea76'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)
        ```
    * 插件
        
        ChineseLocalizations  汉化
        ConvertToUTF8  中文乱码
        OmniMarkupPreviewer  Markdown实时预览
           
    * [linux 中文输入](http://www.jianshu.com/p/bf05fb3a4709)


**2.[NetEase-MusicBox](https://github.com/darknessomi/musicbox) 命令行版[网易云音乐](http://music.163.com/#/my/)**

    * ubuntu 16.04 LTS 安装
    ```bash
    $ sudo apt-get install python-pip
    $ sudo pip install NetEase-MusicBox
    $ sudo apt-get install mpg123  //Linux/Unix 上的 mp3 播放器(必须安装)
    $ sudo apt-get install aria2  //用于缓存歌曲(有可以用来替换 wget )
    ```

    * 使用
    ```bash
    $ musicbox  //vi 快捷键操作 'Space' == 暂停和播放
    ```

**3.PyCharm**

   * ubuntu 16.04 LTS 安装
   
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