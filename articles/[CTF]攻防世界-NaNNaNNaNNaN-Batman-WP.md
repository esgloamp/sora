*2021/7/29*

# [CTF]攻防世界-NaNNaNNaNNaN-Batman-WP

文件后缀名加上html，用浏览器打开
F12查看源码

![此图片的alt属性为空；文件名为image.png]([CTF]攻防世界-NaNNaNNaNNaN-Batman-WP.assets/image.png)

将script里的内容复制到控制台，前面加上let赋值，然后打印出这个变量

![此图片的alt属性为空；文件名为image-1.png]([CTF]攻防世界-NaNNaNNaNNaN-Batman-WP.assets/image-1.png)

代码审计一波

![此图片的alt属性为空；文件名为image-2.png]([CTF]攻防世界-NaNNaNNaNNaN-Batman-WP.assets/image-2.png)

发现前面一堆正则完全不用理会，直接把第8行到第16行的代码执行一遍即可获得flag

![此图片的alt属性为空；文件名为image-3.png]([CTF]攻防世界-NaNNaNNaNNaN-Batman-WP.assets/image-3.png)

