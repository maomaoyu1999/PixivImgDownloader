﻿# PixivImgDownloader

## pixiv图片下载器

<br>
作为一个喜欢收藏好看插画的人，每次用手点保存图片实在是效率低下，
就花了点时间用python写了个能批量下载的程序，顺便加深一下理解。
  
主要功能为：下载排行榜的图片和画师的作品。用selenium登录一次拿到cookie之后就可以用了。
至于cookie能用多久没测试过，要是下载的图数量不对可以删掉cookie重新获取一次。
<br/>

支持排行榜url所有参数，动图会自动合成为gif，gif合成器方面的代码用的多进程，有空闲时间会改成多线程。
下载动图排行榜的时候如果用多进程稍微有点占内存，可以改下代码。

用法：

```python
from PixivImageDownloader import PixivScheduler, DownloadQueue

# 要使用多进程要指定multy_process=True，multy_process默认为False
# 还支持图片大小有 ['mini', 'thumb', 'small', 'regular', 'original']，image_size默认为original
# 动图支持的size有 ['src', 'originalSrc']，ugoira_size默认为originalSrc
# 第一次使用请输入用户名，保存了cookie文件之后就不需要登录了
PS = PixivScheduler(username="Your username",
                    password="Your password",
                    image_size='regular',
                    multy_process=True)
Q = DownloadQueue()

# 排行榜模式
params_li = PS.rank_mode()  # 排行榜模式，默认下载当日综合排行榜，参数根据排行榜url输入就行了
Q.add_task(params_li)  # 添加参数到下载队列
# https://www.pixiv.net/ranking.php?mode=daily&content=illust
params_li = PS.rank_mode(mode='daily', content='illust')
Q.add_task(params_li)  # 添加参数到下载队列

# 画师模式
# https://www.pixiv.net/users/画师ID
# 画师ID输入字符串或者int都可以
params_li = PS.artist_mode('画师ID', content='illust')  # 画师模式
Q.add_task(params_li)  # 添加参数到下载队列
Q.run()
```

基本功能已经写好了，想自定义自己的下载器可以看各个类的注释。


