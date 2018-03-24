# Shell 脚本

### 使用 `wget` `curl` 来执行 shell 脚本

```shell
$ wget -qO - http://skylens.github.io/echo.sh | sh
$ curl -sSL http://skylens.github.io/echo.sh | sh
```

### 使用短的 url 

- [git.io](https://git.io/)
- [ow.ly](http://ow.ly/url/shorten-url)
- [bit.ly](https://bitly.com/)

```shell
$ curl -sSL https://git.io/vddVc | sh
```

wc 

## sed 流处理编辑器、行处理

+ 原则

a、sed一次只处理一行内容 

b、sed不改变文件内容( 除非重定向 )

+ 

## grep 字符匹配

```bash
$ grep '1' test   //筛选出 test 中含 1 的内容
$ grep '[0-9]' test   //筛选出 test 中含从 0 到 9 的内容
$ grep '[^0-9]' test   //筛选出 test 中 0 到 9 之外的内容
```

egrep




