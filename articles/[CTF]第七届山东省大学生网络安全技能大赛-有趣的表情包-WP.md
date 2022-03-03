*2021/5/3*

# [CTF]第七届山东省大学生网络安全技能大赛-有趣的表情包-WP

蜂蜜浏览器打开图片，按tab键查看exif信息，发现照相机型号，加上flag{}后填写进去，错误；

用strings ada.jpg | grep flag，发现有两个flag.txt文本，意识到图片里可能隐藏了文件；

用binwalk查看图片，发现尾部隐藏了一个zip格式文件，zip文件从0x35695开始到结尾，于是编写python程序：

```python
f = open("./ctf/ada.jpg", "rb").read()
ret = open("./ctf/ret.zip", "wb")
ret.write(f[0x35695:])
```

打开ret.zip，里面有flag.txt文件，看来就是答案了，不过解压要密码，将开头获得的照相机型号输入，错误，可能加过密，最大的可能就是HEX编码，拿去解码，得到密码，解压成功，搞定。