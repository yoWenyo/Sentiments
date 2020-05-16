# CSS-canvas画图

## 使用背景

> 微信小程序接单，需要做一个人脸融合的小程序，在多番查找之后，找到了百度AI的解决方案-人脸融合，总体说照片效果决定了处理效果  
> 中间想做一个人像拼合的，但是没有API调用，所以经过搜索之后，发现使用canvas画图可以将图片合成到canvas上面，然后导出来图片即可。

## 使用总结

> 在微信小程序中使用和html中基本一致，需要先获取到canvas对象，然后在将图片一层层的画上去，最后用方法导出来。  
> 在使用过程中，还要注意图片转码问题转换为base64保存方便
> 保存图图片时，要注意输出图片的大小
> 在画上去时，在9个参数的函数上，需要清楚一件事，固定宽度，则要将图片的宽高之比，算出画到canvas固定区域的宽和高，宽固定，则需要用宽宽之比等于高高之比，逻辑一定要绕得出来！！！！

```JS
drawImage(imageResource, dx, dy)
drawImage(imageResource, dx, dy, dWidth, dHeight)
drawImage(imageResource, sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight)

string imageResource
所要绘制的图片资源（网络图片要通过 getImageInfo / downloadFile 先下载）

number sx
需要绘制到画布中的，imageResource的矩形（裁剪）选择框的左上角 x 坐标

number sy
需要绘制到画布中的，imageResource的矩形（裁剪）选择框的左上角 y 坐标

number sWidth
需要绘制到画布中的，imageResource的矩形（裁剪）选择框的宽度

number sHeight
需要绘制到画布中的，imageResource的矩形（裁剪）选择框的高度

number dx
imageResource的左上角在目标 canvas 上 x 轴的位置

number dy
imageResource的左上角在目标 canvas 上 y 轴的位置

number dWidth
在目标画布上绘制imageResource的宽度，允许对绘制的imageResource进行缩放

number dHeight
在目标画布上绘制imageResource的高度，允许对绘制的imageResource进行缩放

const ctx = wx.createCanvasContext('myCanvas')
ctx.save()
// ctx.drawImage(res.path, 0, 0, width, 812 * x);
ctx.drawImage(res.path, 0, 0, self.data.canvasLocation.width, self.data.canvasLocation.height);//
ctx.draw()
ctx.drawImage(res.path, picHumanLocation.left, picHumanLocation.top, picHumanLocation.width, picHumanLocation.height, picFixLocation.left, picFixLocation.top, picFixLocation.width, picFixLocation.height);
ctx.draw(true) //一定要 传入参数true,才能不覆盖之前的
```