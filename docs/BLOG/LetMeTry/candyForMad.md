# 边缘检测加工视频

> B站刷到[用python播放神女劈观](https://www.bilibili.com/video/BV1nq4y1C7pt/) ，恰好我最近在看 **opencv**，不禁见猎心喜，故欲实践一番



## 环境

**IDE**

- Vscode

**工具**

- opencv
- ffmpeg



## 下载素材

B站上这个  [“小波奇~你在玩一种很新的东西！⚡️”](https://www.bilibili.com/video/BV1ry4y1R7fA/) ，我感觉挺有意思的。嗯~ 就决定是你啦 ！

> 从B站下载视频时，注意将原视频的**视频**和**音频**都下载下来



## 代码实现

**参数配置**

```python
import cv2 as cv
import os

# 拼接 素材-视频 路径
video_name = "BV1ry4y1R7fA"
vc = cv.VideoCapture('./data/' + video_name + '.mp4')

# 获取 素材-视频 尺寸
size = ( int(vc.get(3)),int(vc.get(4)))

# 获取 素材-视频 帧率
cap_fps = vc.get(5)

# 根据 素材-视频类型 设置视频编码器 
fourcc = cv.VideoWriter_fourcc(*'XVID')

# 配置 加工后视频的导出路径 
video = cv.VideoWriter('./frame/out.mp4v', fourcc, cap_fps, size, isColor=False)
```

> 注意点：
>
> - `isColor=False` 很重要，如此才是灰度图
>
> - `fourcc` 一定要与原视频**类型匹配**

**检查素材**

```python
if vc.isOpened():
    open, frame = vc.read()
else:
    open = False
    print('文件发生错误，请检查文件')
```

**加工处理**

```python
# 遍历每一帧
while open:
    ret, frame = vc.read()
    if frame is None:
        break
    if ret == True:
        # 转为灰度图
        img_gray = cv.cvtColor(frame, cv.COLOR_RGB2GRAY)
        # 边缘检测（最后两个参数分别是低阈值、高阈值，视处理效果调整）
        img_canny = cv.Canny(img_gray, 100, 200) 
        # 保存到 导出文件中
        video.write(img_canny)
        # 窗口预览
        cv.imshow('pv', img_canny)
        # 控制窗口（ESC结束）
        if cv.waitKey(1) & 0xff == 27:  # 【27】 代表 'Exc' 键 ；
            break
# 释放资源
vc.release()
video.release()
cv.destroyWindow(winname='pv')
```

**合并音视频**（已配置过环境变量）

```python
# 视频的相对路径
video= "./frame/out.mp4v"
# 拼接 音频的相对路径
music = "./data/"+ video_name +".m4a"
# 合并后视频的绝对路径
out = "E:/Desktop/out.mp4"
# 执行命令
os.system("ffmpeg -i "+ video +" -i "+ music +" -c:v copy -c:a aac -strict experimental "+ out)
```

**deng~ deng~ dengdeng~**

```python
# 播放
os.system("start " + out)
```





## ffmpeg 简单指令

**提取音频**

* 进入 `ffmpeg` 的 `bin` 目录内，地址栏 输入 `cmd` 进入命令行（若配置过环境变量，可跳过此步）
* `ffmpeg -i [某视频的绝对路径] -f wav -ar 16000 -vn 生成音频.wav`

**音视频合并**

`ffmpeg -i [视频的绝对路径] -i [音频的绝对路径] -c:v copy -c:a aac -strict experimental [合并后视频的绝对路径]`



## 对比

`opencv + winsound` 存在 **音画不不同步** 问题，究其原因在于 **逐帧边缘检测处理的速度** 跟不上 **音频播放速度**

- 一边`opencv`处理画面并播放，另一边 `winsound` 播放音频，画面处理速度受硬件条件很大，尤其在面对复杂、高清晰度的视频时差异极为明显
- 处理并实时播放，观看体验不大好

而我采用的 `opencv + ffmpeg`  在思路上保持一致，即`opencv`处理（边缘检测）视频图像，但针对 **音画同步问题**作出了优化， 使用`ffmpeg`将音频合并到新视频中，实现 **音画同步**

- 采取 先处理后播放，处理视频获得新视频，然后将新视频与音频合并，观看最后得到的视频体验要好

- 相对地 这样内存占用要高，终归有中间文件生成

> `opencv` 有且仅对图像进行处理，所以在不依赖其他工具支持的条件下，是无法对音频处理的。
>
> 因为直接调用 `Canny()`对图像进行边缘检测，所以处理效果未来还有很大的提升空间。
>



## 思考

> 如何在实时处理视频图像时兼顾音频 ？

 - 设置定时

   设计定时器，规定每一帧处理的时间上限。无论处理成功与否，到时就处理下一帧

 - 跳帧

   设计跳帧规则，比如规定一秒12帧的视频，随机跳过其中 5%-15%，即1帧（0.6~1.8）

 - 视频内容分析

   简单画面和复杂画面的处理速度肯定是由差异的，所以可以两者对应帧的分布，根据结果设计不同的处理规则。

   ……



## Reference

-  [用python播放神女劈观](https://www.bilibili.com/video/BV1nq4y1C7pt/) 

- [“小波奇~你在玩一种很新的东西！⚡️”](https://www.bilibili.com/video/BV1ry4y1R7fA/) 