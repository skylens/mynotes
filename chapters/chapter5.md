# LaTeX 、Emacs 、Org-mode 相关

## Emacs 配置

[`.emscs`](https://raw.githubusercontent.com/skylens/mydotfile/master/dot.emacs)

`.emacs.d/lisp` 下的文件

[color-theme.el](https://raw.githubusercontent.com/skylens/mydotfile/master/dot.emacs.d/lisp/color-theme.el)

[markdown-mode.el](https://raw.githubusercontent.com/skylens/mydotfile/master/dot.emacs.d/lisp/markdown-mode.el)

[mycedet.el](https://raw.githubusercontent.com/skylens/mydotfile/master/dot.emacs.d/lisp/mycedet.el)

[php-mode.el](https://raw.githubusercontent.com/skylens/mydotfile/master/dot.emacs.d/lisp/php-mode.el)

[rect-mark.el](https://raw.githubusercontent.com/skylens/mydotfile/master/dot.emacs.d/lisp/rect-mark.el)

[window-numbering.el](https://raw.githubusercontent.com/skylens/mydotfile/master/dot.emacs.d/lisp/window-numbering.el)

## 中文字体输出

+[参考](http://www.bagualu.net/wordpress/archives/5396#latex环境)

```bash
$ fc-list -f "%{family}\n"  :lang=zh
```

## LaTeX 

### 安装

*Ubuntu/Debian 下安装 LaTeX*

```bash
$ cd Downloads/
$ wget https://raw.githubusercontent.com/skylens/LinuxNote/master/LaTeX/pkg
```

###  字体设置

```bash
$ /usr/share/fonts/truetype/  //如果系统默认没有字体，把字体文件拷贝到这个目录下，刷新字体缓存就有这个字体了
$ fc-list //列出系统安装的字体
$ fc-list :lang=zh  //列出系统安装的中文字体
$ fc-list :lang=en  //列出系统安装的英文字体
$ fc-cache -fsv  //刷新字体缓存
```
### xelatex 编译

+ xelatex 出错时，**`shift + x`** + **`Enter`** 退出

### LaTex 中文文档

**xelatex 方式编译生成 PDF 文档**

```bash
$ nano latex-xeletex-zh.tex
  
\documentclass{article}
 
\usepackage{xeCJK}
 
\setCJKmainfont{SimSun}
\setCJKsansfont{simhei}
\setCJKmonofont{simfang}

\begin{document}
  
\title{LaTex 中文模板（xeletex 编译）}
   
\section{前言}
这是前言的全部内容。
 
\section{第一章}
这是第二章的内容。
 
\vspace{0.5cm}
 
\end{document}

$ xelatex latex-xeletx-zh.tex
```

**pdflatex 方式编译生成 PDF 文档**

```bash
$ pdflatex latex-pdf-zh.tex
```

### Emacs 下使用 LaTeX


## [pandoc](http://pandoc.org/) 格式转换工具 

**Markdown 格式装换为 LaTex、PDF、docx、html 格式**

**[try pandoc](https://pandoc.org/try/)**

**[在线文档装换](https://convertio.co/zh/)**

### Ubuntu 16.04 LTS 安装

```bash
$ wget https://github.com/jgm/pandoc/releases/download/1.19.2.1/pandoc-1.19.2.1-1-amd64.deb
$ sudo gdebi pandoc-1.19.2.1-1-amd64.deb
```

### 转换命令

```bash
$ pandoc filename.md -f markdown -t html -s -o filename.html  //Markdown 转 html
$ pandoc filename.tex -t latex -o filename.pdf --latex-engine=xelatex 
//先转成 LaTex 再转 PDF
$ pandoc filename.md -f markdown -t latex -s -o filename.tex  //Markdown 转 LaTex
$ 
```

###中文排版

*导出为 `PDF` 格式

```bash
$ pandoc filename.md -f markdown -t html -s -o filename.html  //先把Markdown转为 html
$ pandoc -s filename.html --latex-engine=xelatex -V mainfont=文泉驿等宽正黑 -o filename.pdf  //在通过指定主要字体来导出为中文PDF
$ pandoc --template=template.tex --latex-engine=xelatex filename.md -o filename.pdf  //Markdown直接转换为PDF格式
```


*导出为 `docx` 格式

### 问题

* 问题一

```
pandoc: Error producing PDF from TeX source.
! LaTeX Error: File `lmodern.sty' not found.

Type X to quit or <RETURN> to proceed,
or enter new name. (Default extension: sty)

Enter file name: 
! Emergency stop.
<read *> 

l.4 \usepackage

Error: pandoc document conversion failed with error 43
Execution halted
```

解决方法
```bash
$ sudo apt-get install lmodern -y
```

* 问题二

  中文问题

  [解决方法](https://github.com/tzengyuxio/pages/tree/gh-pages/pandoc)
  [https://www.afoo.me/posts/2013-07-10-how-to-transform-chinese-pdf-with-pandoc.html](https://www.afoo.me/posts/2013-07-10-how-to-transform-chinese-pdf-with-pandoc.html)

*__相关链接__*

[Emacs + LaTeX 快速上手](http://cs2.swfc.edu.cn/~wx672/lecture_notes/linux/latex/latex_tutorial.html)

[How To Write Your Lab Report With Emacs Org-mode](http://cs2.swfc.edu.cn/~wx672/lecture_notes/linux/tutorials/org/howto.html)

[西南林业大学本科毕业论文LaTeX模版使用说明](http://cs2.swfu.edu.cn/~wx672/texmf/doc/latex/swfu/swfcthesis/readme.html)

[毕业论文模板](http://cs2.swfu.edu.cn/~wx672/swfcthesis/)
