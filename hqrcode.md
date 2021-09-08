# hqrcode说明

hqrcode 是一款给予go语音开发的可以创建和解析二维码的命令行工具

# 创建二维码

```shell
# 创建二维码  -content 指定内容
./hqrcode -content "https://www.hjxstbserver.xyz" 

# 控制设置大小 -size 设置二维码大小 单位像素
./hqrcode -content "https://www.hjxstbserver.xyz" -size 400  # 默认大小 256 *256

# 设置背景颜色 -bgcolor（RGBA） 和 二维码颜色 -qrcolor （RGBA）
./hqrcode -content "https://www.hjxstbserver.xyz" -size 400 -bgcolor "255,255,255,255" -qrcolor "0,255,0,255"

# 设置输出名称 -o filename
./hqrcode -content "https://www.hjxstbserver.xyz" -size 400 -bgcolor "255,255,255,255" -qrcolor "0,255,0,255" -o hjx.png

#创建有logo的二维码 -logo （jpg|jpeg|png）
./hqrcode -content "https://www.hjxstbserver.xyz" -logo 'logo.jpg' -o hjx.png

#设置logo 大小
./hqrcode -content "https://www.hjxstbserver.xyz" -logo 'logo.jpg' -logosize 40 -o hjx.png
```



# 解析二维码

```shell
#解析二维码 -decode filename
./hqrcode -decode hjx.png  #目前不能解析带logo的二维码
```



