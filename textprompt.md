[toc]

# 工具名称

textprompt (linux)

textprompt.exe (windows)

提示：为了确保稳定性，请在64位的系统上使用,linux 上使用请先赋予权限 chmod +x textprompt 

# 帮助命令 

帮助命令 使用 -h、--h、-help、--help参数

```shell
./textprompt --help
Usage of ./textprompt:
  -data string
        enter some text to prompt
  -file string
        enter file name to prompt
  -h    print tool help text
  -help
        print tool help text
  -jkey string
        enter json keys to get value
  -json
        whether user json prompt text (default true)
  -num int
        enter how many get match regexp (default 1)
  -re
        whether user regexp prompt text (default true)
  -rkey string
        enter regexp keys to get text
  -v    print tool version
  -version
        print tool version
```

# 版本信息

查看版本信息 请使用-v 、--v、-version、--version参数

```shell
./textprompt --version
text prompt tool v1.0.1/linux @jx
```

# json 数据值提取

## -data 方式传入json数据

```shell
# 获取json vaule值
 ./textprompt  -data '{"name":{"first":"Janet","last":"Prichard"},"age":47}' -jkey "age"
47

#多个key 使用，隔开
./textprompt  -data '{"name":{"first":"Janet","last":"Prichard"},"age":47}' -jkey "name,name.first,age"
{"first":"Janet","last":"Prichard"}
Janet
47


```

## -file方式传入json

```shell
# test.json 测试数据如下
{
  "name": {"first": "Tom", "last": "Anderson"},
  "age":37,
  "children": ["Sara","Alex","Jack"],
  "fav.movie": "Deer Hunter",
  "friends": [
    {"first": "Dale", "last": "Murphy", "age": 44, "nets": ["ig", "fb", "tw"]},
    {"first": "Roger", "last": "Craig", "age": 68, "nets": ["fb", "tw"]},
    {"first": "Jane", "last": "Murphy", "age": 47, "nets": ["ig", "tw"]}
  ]
}
./textprompt -file test.json -jkey "name,name.first,age,children.#,children.1"
{"first": "Tom", "last": "Anderson"}
Tom
37
3
Alex

```

## json提取说明

```shell
# 必须传入正确json 否在会解析失败。普通文本提取可以采用 -re的方式
./textprompt  -data '{"name":{"first":"Janet","last":"Prichard"},"age":47' -jkey "age"
2021/08/08 16:51:57 -data text is not json

# -jkey为空时会输出整段json
./textprompt -file test.json
{
  "name": {"first": "Tom", "last": "Anderson"},
  "age":37,
  "children": ["Sara","Alex","Jack"],
  "fav.movie": "Deer Hunter",
  "friends": [
    {"first": "Dale", "last": "Murphy", "age": 44, "nets": ["ig", "fb", "tw"]},
    {"first": "Roger", "last": "Craig", "age": 68, "nets": ["fb", "tw"]},
    {"first": "Jane", "last": "Murphy", "age": 47, "nets": ["ig", "tw"]}
  ]
}

# json提取示例
"name.last"          >> "Anderson"
"age"                >> 37
"children"           >> ["Sara","Alex","Jack"]
"children.#"         >> 3
"children.1"         >> "Alex"
"child*.2"           >> "Jack"
"c?ildren.0"         >> "Sara"
"fav\.movie"         >> "Deer Hunter"
"friends.#.first"    >> ["Dale","Roger","Jane"]
"friends.1.last"     >> "Craig"
"friends.#(last=="Murphy").first"    >> "Dale"
"friends.#(last=="Murphy")#.first"   >> ["Dale","Jane"]
"friends.#(age>45)#.last"            >> ["Craig","Murphy"]
"friends.#(first%"D*").last"         >> "Murphy"
"friends.#(first!%"D*").last"        >> "Craig"
"friends.#(nets.#(=="fb"))#.first"   >> ["Dale","Roger"]
```

# regexp 提取数据

## -data 方式 regexp 提取数据

```shell
# regexp 用于正则表达式提取数据 需要加上 -re 参数
./textprompt  -data '{"name":{"first":"Janet","last":"Prichard"},"age":47}' -re  -rkey "l.*t"
last

/textprompt  -data '{"name":{"first":"Janet","last":"Prichard"},"age":47}' -re  -rkey "[A-Z].*d"
Janet","last":"Prichard

# -num 设置匹配个数，=0则不匹配， < 0则匹配全部
./textprompt -data "/home/ubuntu/hjx/program/download/tools/Hjx" -re -rkey "(?i)hjx" -num -1
hjx
Hjx

```

## -file方式regexp 提取数据

```shell
# file的方式 程序是逐行匹配，不能跨行匹配
ubuntu@VM-16-9-ubuntu:~/hjx/program/download/tools$ ./textprompt -file test.json -re -rkey "\s*\".*,"
  "name": {"first": "Tom", "last": "Anderson"},
  "age":37,
  "children": ["Sara","Alex","Jack"],
  "fav.movie": "Deer Hunter",
"first": "Dale", "last": "Murphy", "age": 44, "nets": ["ig", "fb", "tw"]},
"first": "Roger", "last": "Craig", "age": 68, "nets": ["fb", "tw"]},
"first": "Jane", "last": "Murphy", "age": 47, "nets": ["ig",
```

## regexp 提取说明

```shell
#当rkey 为空时会打印全部
./textprompt -file test.json -re -rkey ""
{
  "name": {"first": "Tom", "last": "Anderson"},
  "age":37,
  "children": ["Sara","Alex","Jack"],
  "fav.movie": "Deer Hunter",
  "friends": [
    {"first": "Dale", "last": "Murphy", "age": 44, "nets": ["ig", "fb", "tw"]},
    {"first": "Roger", "last": "Craig", "age": 68, "nets": ["fb", "tw"]},
    {"first": "Jane", "last": "Murphy", "age": 47, "nets": ["ig", "tw"]}
  ]
}

#正则匹配说明
单字符：
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
结合：

        xy             匹配x后接着匹配y
        x|y            匹配x或y（优先匹配x）
重复：

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
实现的限制：计数格式x{n}等（不包括x*等格式）中n最大值1000。负数或者显式出现的过大的值会导致解析错误，返回ErrInvalidRepeatSize。

分组：

        (re)           编号的捕获分组
        (?P<name>re)   命名并编号的捕获分组
        (?:re)         不捕获的分组
        (?flags)       设置当前所在分组的标志，不捕获也不匹配
        (?flags:re)    设置re段的标志，不捕获的分组
标志的语法为xyz（设置）、-xyz（清楚）、xy-z（设置xy，清楚z），标志如下：

        i              大小写不敏感（默认关闭）
        m              ^和$在匹配文本开始和结尾之外，还可以匹配行首和行尾（默认开启）
        s              让.可以匹配\n（默认关闭）
        U              非贪婪的：交换x*和x*?、x+和x+?……的含义（默认关闭）
边界匹配：
        ^              匹配文本开始，标志m为真时，还匹配行首
        $              匹配文本结尾，标志m为真时，还匹配行尾
        \A             匹配文本开始
        \b             单词边界（一边字符属于\w，另一边为文首、文尾、行首、行尾或属于\W）
        \B             非单词边界
        \z             匹配文本结尾
转义序列：

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
字符族（预定义字符族之外，方括号内部）的语法：

        x              单个字符
        A-Z            字符范围（方括号内部才可以用）
        \d             Perl字符族
        [:foo:]        ASCII字符族
        \pF            单字符名的Unicode字符族
        \p{Foo}        完整字符名的Unicode字符族
预定义字符族作为字符族的元素：

        [\d]           == \d
        [^\d]          == \D
        [\D]           == \D
        [^\D]          == \d
        [[:name:]]     == [:name:]
        [^[:name:]]    == [:^name:]
        [\p{Name}]     == \p{Name}
        [^\p{Name}]    == \P{Name}
Perl字符族：

        \d             == [0-9]
        \D             == [^0-9]
        \s             == [\t\n\f\r ]
        \S             == [^\t\n\f\r ]
        \w             == [0-9A-Za-z_]
        \W             == [^0-9A-Za-z_]
ASCII字符族：
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

