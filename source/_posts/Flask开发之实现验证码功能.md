---
title: Flask开发之实现验证码功能
date: 2018-02-20 18:02:27
tags: Python
toc: true
---

> 在进行web开发时，考虑到安全问题，一般都会添加验证码功能，那么，在Flask中如何实现验证码功能呢?

编写验证码的步骤：
1. 获取验证码（字母+数字）；
2. 获取验证码图片；
3. 将验证码图片显示于网页；
4. 点击验证码图片刷新。

<!--more-->

### 获取验证码
Flask是Python的web开发框架之一，这一部分使用Python编写，代码如下：
```
    codenum = 4
    source = list(string.letters)
    for index in range(0, 10):
        source.append(str(index))
    code = ''.join(random.sample(source, 4))
```
### 获取验证码图片
这里使用Python中的PIL库绘制，代码如下：
```
# 设置图片大小
width = 45 * 5
height = 50
image = Image.new('RGB', (width, height), (255, 255, 255))
#选择字体
font = ImageFont.truetype('Arial.ttf', 36)
draw = ImageDraw.Draw(image)

for x in range(width):
    for y in range(height):
        colorRandom1 = (random.randint(255, 255), random.randint(255, 255), random.randint(255, 255))
        draw.point((x, y), fill=colorRandom1)

    for t in range(codenum):
        colorRandom2 = (random.randint(85, 155), random.randint(85, 155), random.randint(85, 155))
        draw.text((45 * t + 25, 10), code[t], font=font, fill=colorRandom2)
image_d = ImageDraw.Draw(image)
for i in range(5):
    colorRandom3 = (random.randint(85, 100), random.randint(85, 100), random.randint(85, 100))
    begin = (random.randint(0, width), random.randint(0, height))
    end = (random.randint(0, width), random.randint(0, height))
    image_d.line([begin, end], fill=colorRandom3)
```
得到了验证码图片之后，就要考虑一个问题，如何保存验证码图片的问题，最简单的方法当然就是将其保存于文件中。
```
image = image.save('/路径' + code +'.jpg')
```
但是，保存到文件中导致占用的空间过大，那么如何解决呢？stringio库可以实现将文件保存于内存中，而不是磁盘中，在本次代码编写中，使用比stringio更快的cstringio来进行实现，将上述代码中保存于文件的代码片段进行替换：
```
import cStringIO
    tmps = cStringIO.StringIO()
    image.save(tmps, "jpeg")
    res = Response()
    res.headers.set("Content-Type", "image/JPEG;charset=UTF-8")
    res.set_data(tmps.getvalue())
    return res
```

### 将验证码显示于网页
接下来，就是最重要的一部分了，需要将得到的验证码显示于网页。因为是img标签，所以肯定是需要src属性，刚开始不熟悉，总觉得需要src后面需要添加图片的路径，而采用图片流的方式返回的不是图片，该怎么处理呢？在这里，将上述得到验证码图片的代码定义为一个路由，在前端的<img>标签中调用。
```
 <img id="verficode" src="./verficode">
```
即可实现调用。

### 点击验证码刷新
点击验证码刷新属于前端的内容了，需要在img标签中添加onclick属性，如下：
```
<img id="verficode" src="./verficode" onclick="window.location.reload()">
```
上述代码，即可实现验证码的显示。





