[toc]

# 1、hurl 简介

everything hurl 是一个命令行工具，有windows （hurl.exe） 和 linux （hurl） 两个平台版本。目前支持的协议有file，ftp, sftp

, http, https（http2、http3），所以hurl是综合传输工具（来自jx的定义）

# 2、帮助命令

./hurl -h, --h, -help, --help

```shell
./hurl -h
Usage of ./hurl:
  -I    whether HEAD request http request
  -L    whether to open the redirect request
  -byte int
        byte array max length default 10M (default 10485760)
  -crt string
        enter the crt file
  -data string
        http request body data
  -download string
        download ftp to local path
  -file string
        input file name use to multipart/form
  -h    print hurl tool help text
  -headers string
        set http request header,must json type
  -help
        print hurl tool help text
  -http2
        whether use http 2 protocol
  -http3
        whether use http 3 protocol
  -i    whether print http request headers
  -method string
        enter http protocol request method (default "GET")
  -multipart
        whether send a multipart/form request
  -o string
        enter file name to save http body
  -password string
        enter the user password
  -re string
        enter the regexp rule to match
  -type string
        walk the path type (default "all")
  -u int
        enter the number of concurrent (default 5)
  -upload string
        upload local path to ftp
  -url string
        enter a net/url text or local path
  -user string
        enter the user name
  -v    print hurl tool version
  -version
        print hurl tool version
  -walk
        whether walk to the path
```

# 3、版本信息

./hurl -v, --v , -version , --version

```shell
./hurl -v
everything is hurl v1.0.0/linux @jx

Protocols:file ftp sftp http https;
EMail:1245863260@qq.com g1245863260@gmail.com;
Github:https://github.com/stonebirdjx;
Gitee:https://gitee.com/stonebirdjx;
```

# 4、file协议使用说明

file协议 url 为 file://path 或者 path 即可

## 4.1、查看单个文件

```shell
# 文件-相对路径
./hurl -url "hurl.go"
# 文件绝对路径
./hurl -url "/home/ubuntu/hjx/program/hurl/hurl.go"
# file协议访问文件 只支持绝对路径
./hurl -url "file:///home/ubuntu/hjx/program/hurl/hurl.go"
# 正则表达式过滤行内容 -re "compile"
./hurl -url "hurl.go" -re "(u)$"
```

## 4.2、查看文件夹 - 列出文件夹里面内容 

```shell
# 列出文件夹
ubuntu@VM-16-9-ubuntu:~/hjx/program/hurl$ ./hurl -url "/home/ubuntu"
-rw------- ubuntu ubuntu   180 2021-08-21 13:06:55 .Xauthority
-rw------- ubuntu ubuntu 55871 2021-08-21 13:36:55 .bash_history
drwx------ ubuntu ubuntu  4096 2021-03-23 23:43:30 .cache
drwxrwxr-x ubuntu ubuntu  4096 2021-04-10 15:50:05 .config
-rw-r--r--   root   root    52 2021-04-10 11:14:00 .gitconfig
drwx------ ubuntu ubuntu  4096 2021-03-22 23:50:20 .gnupg
drwxr-x--- ubuntu ubuntu  4096 2021-03-23 23:46:40 .groovy
drwxr-x--- ubuntu ubuntu  4096 2021-03-23 23:35:56 .java
drwxr-x--- ubuntu ubuntu  4096 2021-08-16 21:37:48 .jenkins
-rw------- ubuntu ubuntu   225 2021-03-23 11:24:39 .mysql_history
drwxr-xr-x   root   root  4096 2021-03-22 23:50:17 .pip
-rw-r--r--   root   root    73 2021-03-22 23:50:17 .pydistutils.cfg
-rw-rw-r-- ubuntu ubuntu    75 2021-03-23 23:55:57 .selected_editor
drwx------ ubuntu ubuntu  4096 2021-04-10 11:18:14 .ssh
-rw-r--r-- ubuntu ubuntu     0 2021-03-22 23:50:42 .sudo_as_admin_successful
-rw-------   root   root  7588 2021-03-23 11:46:18 .viminfo
drwxrwxr-x ubuntu ubuntu  4096 2021-03-23 11:25:56 go
drwxrwxr-x ubuntu ubuntu  4096 2021-08-17 21:42:45 hjx

# 列出文件夹 过滤对应file -re "compile" re对文件名正则匹配
ubuntu@VM-16-9-ubuntu:~/hjx/program/hurl$ ./hurl -url "/home/ubuntu" -re "go|hjx"
drwxrwxr-x ubuntu ubuntu 4096 2021-03-23 11:25:56 go
drwxrwxr-x ubuntu ubuntu 4096 2021-08-17 21:42:45 hjx
```

## 4.3、Walk文件夹 - 打印文件路径

walk 时 -re 对路径进行正则匹配

 ```shell
 # 遍历所有
 ./hurl -url "/home/ubuntu/hjx/sftptest/" -walk
 # 遍历文件夹
 ./hurl -url "/home/ubuntu/hjx/sftptest/" -walk -type dir
 # 遍历文件夹 过滤文件夹 -re "compile"
 ./hurl -url "/home/ubuntu/hjx/sftptest/" -walk -type dir -re 'go$'
 # 遍历文件
 ./hurl -url "/home/ubuntu/hjx/sftptest/" -walk -type file
 # 遍历文件 过滤文件 -re "compile"  re对路径正则匹配
 ./hurl -url "/home/ubuntu/hjx/sftptest/" -walk -type file -re 'pdf$'
 ```

# 5、ftp协议使用说明

ftp协议格式为 ftp://[userinfo@]host/path

绝对路径  //

相对路径 /

如：ftp://ubuntu:ubuntu@192.168.163.128:21//home/ubuntu/test.txt"    文件 绝对路径 

如：ftp://ubuntu:ubuntu@192.168.163.128:21/test.txt"     文件 相对路径 

<font color ="red"> 文件夹 必须以 / 结尾</font>

如：ftp://ubuntu:ubuntu@192.168.163.128:21//home/ubuntu/    文件夹 绝对路径 

如：ftp://ubuntu:ubuntu@192.168.163.128:21/"     文件夹相对路径 

ftp url 其他 方式 写法（-user 代替用户  -password 代替密码）url里的userinfo 优先级高：

​	1、ftp://192.168.163.128:21//home/ubuntu/   -user ubuntu -password ubuntu

​	2、ftp://ubuntu@192.168.163.128:21//home/ubuntu/     -password  ubuntu 

##  5.1、查看单个文件

```shell
# 查看文件
./hurl -url "ftp://ubuntu:ubuntu@192.168.163.128:21//home/ubuntu/test.txt"  #绝对路径
./hurl -url "ftp://ubuntu:ubuntu@192.168.163.128:21/test.txt"  #相对路径

# 过滤文件行内容  -re "compile"
./hurl -url "ftp://ubuntu:ubuntu@192.168.163.128:21/test.txt" -re "boy%"

```

## 5.2、查看文件夹

```shell
./hurl -url "ftp://ubuntu:ubuntu@192.168.163.128:21//home/ubuntu/" [-walk] [-type [dir|file|all]] [-re compile]
./hurl -url "ftp://ubuntu:ubuntu@192.168.163.128:21/"

# -type （file 、dir、all）来过滤对应 信息
./hurl -url "ftp://ubuntu:ubuntu@192.168.163.128:21//home/ubuntu/" -type dir
./hurl -url "ftp://ubuntu:ubuntu@192.168.163.128:21//home/ubuntu/" -type file

# 可以使用 -re 过滤 ，对文件名正则
./hurl -url "ftp://ubuntu:ubuntu@192.168.163.128:21//home/ubuntu/" -re  "test.*go$"
```

## 5.3、Walk文件夹

```shell
./hurl -url "ftp://ubuntu:ubuntu@192.168.163.128:21//home/ubuntu/" -walk 
#同理可以使用 -type  -re （re对路径匹配）
./hurl -url "ftp://ubuntu:ubuntu@192.168.163.128:21//home/ubuntu/" -walk -type dir -re "\.go$"
```

## 5.4、文件上传

文件上传 ftp path 必须以 / 结尾，本地路径如果为文件 就上传单个文件，本地如果是文件夹 会上传此文件夹下所有内容

```shell
./hurl -url "ftp://ubuntu:ubuntu@192.168.163.128:21//home/ubuntu/" -upload "./test.txt"  #上传文件到ftp
./hurl -url "ftp://ubuntu:ubuntu@192.168.163.128:21//home/ubuntu/" -upload "./"   #上传文件夹（含子文件夹）到ftp
```

## 5.5、文件下载

文件下载 本地必须是目录

```shell
./hurl -url "ftp://ubuntu:ubuntu@192.168.163.128:21//home/ubuntu/test.txt" -download "./"  #ftp 下载单个文件
./hurl -url "ftp://ubuntu:ubuntu@192.168.163.128:21//home/ubuntu/" -download "./"   #ftp 下载目录
```

# 6、sftp协议说明

 用法 和 ftp 一模一样 。此处不过多详述

# 7、http、https协议使用说明

## 7.1、普通http、https请求

```shell
./hurl -url "http://www.hjxstbserver.xyz/"
./hurl -url "https://www.hjxstbserver.xyz/"

# 自定义crt 文件
./hurl -url "https://www.hjxstbserver.xyz/"  -crt "sever.crt"
```

## 7.2、HEAD请求

使用 -I 或者 -method HEAD 发送 head 请求 

```shell
./hurl -url "https://www.hjxstbserver.xyz/" -I
HTTP/1.1 301 Moved Permanently
Cache-Control: [no-cache, no-store, max-age=0, must-revalidate, value]
Location: [/pc/index]
Makefriends-Email: [1245863260@qq.com]
Server-Name: [hjx stb.gin server]
Server: [nginx/1.19.3]
Date: [Sat, 21 Aug 2021 11:54:03 GMT]
Content-Type: [text/html; charset=utf-8]
Connection: [keep-alive]

./hurl -url "https://www.hjxstbserver.xyz/" -method HEAD
HTTP/1.1 301 Moved Permanently
Date: [Sat, 21 Aug 2021 11:54:35 GMT]
Content-Type: [text/html; charset=utf-8]
Connection: [keep-alive]
Cache-Control: [no-cache, no-store, max-age=0, must-revalidate, value]
Location: [/pc/index]
Makefriends-Email: [1245863260@qq.com]
Server-Name: [hjx stb.gin server]
Server: [nginx/1.19.3]
```

## 7.3、重定向请求

-L  参数支持重定向

```shell
./hurl -url "https://www.hjxstbserver.xyz/" -I -L
```

## 7.4、显示内容与头部

-i 参数 支持 打印body 时 打印头部

```shell
./hurl -url "https://www.hjxstbserver.xyz/" -i -L
```

## 7.5、自定义header

-headers 'json text' 用来设置自定义header

```shell
./hurl -url "https://www.hjxstbserver.xyz/" -i -L -headers '{"name":"hjx","len":18}'
```

## 7.6、保存文件

-o filename 将body 数据保存到文件

```shell
./hurl -url "https://www.hjxstbserver.xyz/" -i -L -headers '{"name":"hjx","len":18}' -o index.html
```

## 7.7、其他方式的请求

用 -method 改变请求方式即可，-data 设置body 发送数据

```shell
# 发送POST请求
./hurl -url "https://www.hjxstbserver.xyz/system/getCkey" -i -L -headers '{"name":"hjx","len":18}' -method POST

# 发送json 数据
./hurl -url "https://www.hjxstbserver.xyz/system/getJson" -i -L -headers '{"Content-Type":"application/json"}' -data '{"name":"hjx","len":18}' -method POST 

# 发送普通form 表单
./hurl -url "https://www.hjxstbserver.xyz/system/getForm" -i -L -headers '{"Content-Type":"application/x-www-form-urlencoded"}' -data 'name=hjx&age=18&len=18' -method POST 

# -multipart 发送multipart/form  -file 参数可以携带文件，多个文件用','隔开 -multipart 参数下 -data 必须为json 数据
./hurl -url "https://www.hjxstbserver.xyz/system/getMultipart" -i -L -data '{"name":"hjx","len":18}' -method POST  -multipart -file "hurl.go,conf.go"
```

## 7.2、http2 、http3 请求

可以使用 -http2  来实现http2 请求，-http3 实现http3请求

```shell
./hurl -url "https://127.0.0.1:8888/" -http2 -i
HTTP/2.0 200 OK
Date: [Sun, 22 Aug 2021 01:52:58 GMT]
Content-Type: [text/plain; charset=utf-8]
Content-Length: [5]

Hello

./hurl -url "https://127.0.0.1:9888/" -http3 -i
HTTP/3 200 OK
Server: [hjx-http3]

it is http3 server
```



​	

# 附录

## 其他一些参数说明

```shell
#ftp、sftp 上传和下载时控制并发数 -u
./hurl -url "ftp://ubuntu:ubuntu@192.168.163.128:21//home/ubuntu/" -upload "./" -u 8

# 控制下载、上传数量 可以用-byte 来控制速度，不设置时最大默认值是10M
./hurl -url "ftp://ubuntu:ubuntu@192.168.163.128:21//home/ubuntu/" -upload "./" -u 8 -byte 1024
```



## 正则表达式说明

单字符：

```
        .              任意字符（标志s==true时还包括换行符）
        [xyz]          字符族
        [^xyz]         反向字符族
        \d             Perl预定义字符族
        \D             反向Perl预定义字符族
        [:alpha:]      ASCII字符族
        [:^alpha:]     反向ASCII字符族
        \pN            Unicode字符族（单字符名），参见unicode包
        \PN            反向Unicode字符族（单字符名）
        \p{Greek}      Unicode字符族（完整字符名）
        \P{Greek}      反向Unicode字符族（完整字符名）
```

结合：

```
        xy             匹配x后接着匹配y
        x|y            匹配x或y（优先匹配x）
```

重复：

```
        x*             重复>=0次匹配x，越多越好（优先重复匹配x）
        x+             重复>=1次匹配x，越多越好（优先重复匹配x）
        x?             0或1次匹配x，优先1次
        x{n,m}         n到m次匹配x，越多越好（优先重复匹配x）
        x{n,}          重复>=n次匹配x，越多越好（优先重复匹配x）
        x{n}           重复n次匹配x
        x*?            重复>=0次匹配x，越少越好（优先跳出重复）
        x+?            重复>=1次匹配x，越少越好（优先跳出重复）
        x??            0或1次匹配x，优先0次
        x{n,m}?        n到m次匹配x，越少越好（优先跳出重复）
        x{n,}?         重复>=n次匹配x，越少越好（优先跳出重复）
        x{n}?          重复n次匹配x
```

实现的限制：计数格式x{n}等（不包括x*等格式）中n最大值1000。负数或者显式出现的过大的值会导致解析错误，返回ErrInvalidRepeatSize。

分组：

```
        (re)           编号的捕获分组
        (?P<name>re)   命名并编号的捕获分组
        (?:re)         不捕获的分组
        (?flags)       设置当前所在分组的标志，不捕获也不匹配
        (?flags:re)    设置re段的标志，不捕获的分组
```

标志的语法为xyz（设置）、-xyz（清楚）、xy-z（设置xy，清楚z），标志如下：

```
        I              大小写敏感（默认关闭）
        m              ^和$在匹配文本开始和结尾之外，还可以匹配行首和行尾（默认开启）
        s              让.可以匹配\n（默认关闭）
        U              非贪婪的：交换x*和x*?、x+和x+?……的含义（默认关闭）
```

边界匹配：

```
        ^              匹配文本开始，标志m为真时，还匹配行首
        $              匹配文本结尾，标志m为真时，还匹配行尾
        \A             匹配文本开始
        \b             单词边界（一边字符属于\w，另一边为文首、文尾、行首、行尾或属于\W）
        \B             非单词边界
        \z             匹配文本结尾
```

转义序列：

```
        \a             响铃符（\007）
        \f             换纸符（\014）
        \t             水平制表符（\011）
        \n             换行符（\012）
        \r             回车符（\015）
        \v             垂直制表符（\013）
        \123           八进制表示的字符码（最多三个数字）
        \x7F           十六进制表示的字符码（必须两个数字）
        \x{10FFFF}     十六进制表示的字符码
        \*             字面值'*'
        \Q...\E        反斜线后面的字符的字面值
```

字符族（预定义字符族之外，方括号内部）的语法：

```
        x              单个字符
        A-Z            字符范围（方括号内部才可以用）
        \d             Perl字符族
        [:foo:]        ASCII字符族
        \pF            单字符名的Unicode字符族
        \p{Foo}        完整字符名的Unicode字符族
```

预定义字符族作为字符族的元素：

```
        [\d]           == \d
        [^\d]          == \D
        [\D]           == \D
        [^\D]          == \d
        [[:name:]]     == [:name:]
        [^[:name:]]    == [:^name:]
        [\p{Name}]     == \p{Name}
        [^\p{Name}]    == \P{Name}
```

Perl字符族：

```
        \d             == [0-9]
        \D             == [^0-9]
        \s             == [\t\n\f\r ]
        \S             == [^\t\n\f\r ]
        \w             == [0-9A-Za-z_]
        \W             == [^0-9A-Za-z_]
```

ASCII字符族：

```
        [:alnum:]      == [0-9A-Za-z]
        [:alpha:]      == [A-Za-z]
        [:ascii:]      == [\x00-\x7F]
        [:blank:]      == [\t ]
        [:cntrl:]      == [\x00-\x1F\x7F]
        [:digit:]      == [0-9]
        [:graph:]      == [!-~] == [A-Za-z0-9!"#$%&'()*+,\-./:;<=>?@[\\\]^_`{|}~]
        [:lower:]      == [a-z]
        [:print:]      == [ -~] == [ [:graph:]]
        [:punct:]      == [!-/:-@[-`{-~]
        [:space:]      == [\t\n\v\f\r ]
        [:upper:]      == [A-Z]
        [:word:]       == [0-9A-Za-z_]
        [:xdigit:]     == [0-9A-Fa-f]
```



