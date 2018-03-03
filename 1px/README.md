# 苹果手机端渲染1px问题
苹果手机由于分辨率较高，其retina的`devicePixelRatio=2`，所以在其下设置边框为1px会显示2px，可能更高
## 解决办法
1. ios8以上的版本可以支持0.5，所以在`devicePixelRatio=2`时，可以将其`border-width`设置为0.5px,这样显示出来就是1px的效果，但是版本在ios8以下的会将0.5显示为0，所以需要进行兼容
```
@media (-webkit-min-device-pixel-ratio: 2) {
    div {
      border-width: 0.5px
    }
  }
```
2. 使用`viewport` + `rem`
在`devicePixelRatio=2`时，输出`viewport`
```
  <meta name="viewport" content="initial-scale=0.5, maximum-scale=0.5, minimum-scale=0.5, user-scable=no">
```
在`devicePixelRatio=3`时，输出`viewport`
```
  <meta name="viewport" content="initial-scale=0.33333333, maximum-scale=0.33333333, minimum-scale=0.33333333, user-scable=no">
```
同时设置对应`viewport`的`rem`的基准值，这样就可以像以前一样使用1px设置边框
3. 伪类 + transform: scale(0.5)
将原元素的`border`去掉，利用`:before`或者`:after`重做`border`,并使用`transform`的`scale`将其缩小一半，最后把原元素相对定位，新`border`绝对定位,
- 单个`border`
```
 div {
    border: none;
    width: 500px;
    height: 500px;
    position: relative;
  }
  div:after {
    display: inline-block;
    content: '';
    position: absolute;
    left: 0;
    width: 100%;
    height: 1px;
    background: #000000;
    transform: scaleY(0.5);
    -webkit-transform: scaleY(0.5);
  }
```
- 四条`border`
设置整体区域与border值，将整体缩小一半
```
div {
    border: none;
    width: 500px;
    height: 500px;
    position: relative;
  }
  div:after {
    display: inline-block;
    content: '';
    position: absolute;
    left: 0;
    top: 0;
    border: 1px solid #000000;
    width: 200%;
    height: 200%;
    box-sizing: border-box;
    -webkit-box-sizing: border-box;
    transform: scale(0.5);
    -webkit-transform: scale(0.5);
  }
```

4. box-shadow
使用盒阴影模拟边框
```
div {
    border: none;
    width: 500px;
    height: 500px;
    box-shadow: 0 1px 1px -1px rgba(0, 0, 0, 0.5);
  }
```
缺点：会有阴影出现
5. 背景渐变
```
.border {
    background:
    linear-gradient(180deg, black, black 50%, transparent 50%) top    left  / 100% 1px no-repeat,
    linear-gradient(90deg,  black, black 50%, transparent 50%) top    right / 1px 100% no-repeat,
    linear-gradient(0,      black, black 50%, transparent 50%) bottom right / 100% 1px no-repeat,
    linear-gradient(-90deg, black, black 50%, transparent 50%) bottom left  / 1px 100% no-repeat;
  }
```
缺点：不能实现圆角
6. 图片
```
.border{
    border-width: 1px;
    border-image: url(border.gif) 2 repeat;
}
```
缺点：修改图片颜色则要修改图片，很麻烦
参考链接：
- [Retina屏的移动设备如何实现真正1px的线？](http://jinlong.github.io/2015/05/24/css-retina-hairlines/#comments)
- [再谈mobile web retina 下 1px 边框解决方案](http://www.ghugo.com/css-retina-hairline/)
- [移动端1px细线解决方案总结](https://www.jianshu.com/p/d62d22b44ce4)