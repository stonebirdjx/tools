# hstream 使用说明

hstream 工具主要用户http服务检测

可用于检测 m3u8 内容的完成性，直播是否断流，直播默认时长10分钟

```shell
#1、新建一个url.txt 内容格式如下,每行一个url，多个写多行
http://xxx/xxx/xxx//xxx
http://xxx/xxx/xxx//xxx/index.m3u8

#2、执行hstream、
chmod +x hstream  #linux 下记得赋予执行权限
nohup ./hstream & #默认url文件为url.txt

nohup ./hstream -urlfile filename & #指定url file
nohup ./hstream -urlfile filename -u 40 &  #-u 指定并发送 默认为5
nohup ./hstream -urlfile filename -u 40 --chunk=false -live 1200 & # -live 设置直播检测时长单位s 默认是10分钟
nohup ./hstream -urlfile filename -u 40 --chunk=false & #不检查body 数据 默认检测
nohup ./hstream -urlfile filename -u 40 --chunk=false  -byte 1048576 & #-byte 可以加快速度，但相对会消耗内存,默认10M

#日志：
hstream.log
#输出
url_check_failure.txt
url_check_success.txt
```

