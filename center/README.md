# 各种居中方法总结
## 水平居中
- 行内元素
为父元素设置`text-align:center`来实现
```
<body>
  <div><span>123</span></div>
</body>
</html>
<style>
  div {
    text-align: center
  }
</style>
```
- 块级元素
1. 宽度确定
- `margin: 0 auto`
```
 .content {
    width: 50%;
    margin: 0 auto;
  }
```
- 绝对定位`absolute`
```
<body>
  <div class="container"><div class="content">12wrwe3234234242342</div>
</body>
</html>
<style>
  .container {
    width: 100%;
  }
  .content {
    width: 200px;
    position: absolute;
    left: 50%;
    margin-left: -100px;
  }
</style>
```
2. 宽度不确定
- 将块级元素改为`inline-block`，父元素设置`text-align: center`
```
.container {
    text-align: center
  }
  .content {
    display: inline-block;
    border: 1px solid #000000
  }
```
- 将父容器设为浮动布局，就可以包裹子元素，再使它从原来的位置移动`50%`,由于父容器本身具有宽度，移动是从父容器左边距为起始位置，所以会有父容器宽度的差值，也就是内容宽度的差值，再将其内容元素往回移动50%即可

父元素未使用浮动
```
<body>
  <div class="container"><div class="content"><div>12wrwe3234234242342</div></div>
</body>
</html>
<style>
  .container {
    width: 100%;
  }
  .content {
    border: 1px solid #000000
  }
</style>
```
![center1](https://github.com/MyDAIDAI/css-study-guide/blob/center/center/center1.png)

使用浮动后
```
.content {
    border: 1px solid #000000;
    float: left;
  }
```
![center2](https://github.com/MyDAIDAI/css-study-guide/blob/center/center/center2.png)

使用浮动后，父元素包裹子元素内容，然后移动位置即可，属性`position:relative`是相对于文档流原本的位置进行定位
```
.content {
    border: 1px solid #000000;
    float: left;
    position: relative;
    left: 50%;
  }
```
![center3](https://github.com/MyDAIDAI/css-study-guide/blob/center/center/center3.png)

当移动之后，由于是按照容器左边原地进行定位，所以会有容器宽度的偏差，我们再将其中的内容往回移动即可：
![center4](https://github.com/MyDAIDAI/css-study-guide/blob/center/center/center4.png)

上面的内容元素已经相对于`container`居中，只需要隐藏其边框即可
- 绝对定位实现，一般使用绝对定位居中的时候需要知道其宽度值，比如：
```
 .content {
    width: 200px;
    position: absolute;
    left: 50%;
    margin-left: -100px;
  }
```
但是怎么在不知道元素宽度的时候使用呢，我们可以使用上面的办法做一些改变：
```
<body>
  <div class="container"><div class="content"><div>12wrwe3234234242342</div></div>
</body>
</html>
<style>
  .container {
    width: 100%;
  }
  .content {
    position: absolute;
    left: 50%;
    border: 1px solid #000
  }
  .content div {
    position: relative;
    right: 50%;
  }
</style>
```
- 使用`flex`
```
<body>
  <div class="container"><div class="content"><div>12wrwe3234234242342</div></div>
</body>
</html>
<style>
  .container {
    width: 100%;
  }
  .content {
    display: flex;
    justify-content: center;
  }
  .content div {
    border: 1px solid #000000
  }
</style>
```
`content`使用`flex`布局之后，会使内部的`div`元素包裹其内容，然后指定其对齐方式即可
- 使用`fit-content`
`fit-content`是`css`给`width`属性新加的一个属性值，配合`margin`使用
```
<body>
  <div class="container"><div class="content">2wrwe3234234242342</div>
</body>
</html>
<style>
  .container {
    width: 100%;
  }
  .content {
    width: fit-content;
    margin: 0 auto;
  }
</style>
```

## 垂直居中
通用办法：
父元素设置相同的`padding-top`与`padding-bottom`值
- 行内元素
1. 单行的行内元素垂直居中,由于只有一行，而`line-height`表示的就是一行文本所占的高度，所以将`line-height`与`height`相同即可
```
<body>
  <div class="container"><div class="content">2wrwe3234234242342</div>
</body>
</html>
<style>
  .container {
    width: 100%;
  }
  .content {
    width: 200px;
    height: 200px;
    border: 1px solid #000000;
    line-height: 200px;
  }
</style>
```
2. 多行行内元素居中
- 使用`vertical-align`
```
<body>
  <div class="container"><div class="content">2wrwe312312333333333332312312312234234242342</div>
</body>
</html>
<style>
  .container {
    width: 100%;
  }
  .content {
    width: 200px;
    height: 200px;
    border: 1px solid #000000;
    word-break: break-all;
    display: table-cell;
    vertical-align: middle
  }
</style>
```
- 使用`flex`，需要父元素指定高度值
```
<body>
  <div class="container"><div class="content">2wrwe312312333333333332312312312234234242342</div>
</body>
</html>
<style>
  .container {
    width: 100%;
  }
  .content {
    width: 200px;
    height: 300px;
    border: 1px solid #000000;
    word-break: break-all;
    display: flex;
    align-items: center
  }
</style>
```
- 块状元素
1. 高度确定
- 使用`absolute`和`margin`
```
.content {
    width: 200px;
    height: 200px;
    position: absolute;
    top: 50%;
    margin-top: -100px;
    border: 1px solid #000000;
    word-break: break-all;
  }
```
- 父元素`posotion:relative`，子元素使用`position:absolute`, `top`和`margin`
```
<body>
  <div class="container"><div class="content"><div>2wrwe312312333333333332312312312234234242342</div></div>
</body>
</html>
<style>
  .container {
    width: 100%;
  }
  .content {
    width: 200px;
    height: 300px;
    border: 1px solid #000000;
    position: relative;
  }
  .content div {
    position: absolute;
    top: 0;
    bottom: 0;
    margin: auto;
    height: 100px;
    word-break: break-all;
    border: 1px solid #000000;
  }
</style>
```
这种实现方式的两个核心是：把要垂直居中的元素相对于父元素绝对定位，top和bottom设为相等的值，我这里设成了0，当然你也可以设为99999px或者-99999px无论什么，只要两者相等就行，这一步做完之后再将要居中元素的margin设为auto，这样便可以实现垂直居中了。被居中元素的宽高也可以不设置，但不设置的话就必须是图片这种自身就包含尺寸的元素，否则无法实现。

2. 高度不确定
- 使用`absolute`和`transform: translateY(-50%)`
```
.content {
    width: 200px;
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
    border: 1px solid #000000;
    word-break: break-all;
  }
```
- 使用`flex`
```
<body>
  <div class="container"><div class="content"><div>2wrwe312312333333333332312312312234234242342</div></div>
</body>
</html>
<style>
  .container {
    width: 100%;
  }
  .content {
    width: 200px;
    /* 设置高度，没有高度则会包裹内容 */
    height: 300px;
    border: 1px solid #000000;
    display: flex;
    flex-direction: column;
    justify-content: center;
  }
  .content div {
    word-break: break-all;
    border: 1px solid #000000
  }
</style>
```

- `positon: relative;top:0;left:0;bottom:0;right:0;margin:auto auto`
- `table table-cell`

参考： 
- [六种实现元素水平居中](https://www.w3cplus.com/css/elements-horizontally-center-with-css.html)
- [CSS 居中大全](https://jinlong.github.io/2013/08/13/centering-all-the-directions/)
- [用CSS/CSS3 实现 水平居中和垂直居中的完整攻略](http://blog.csdn.net/summer_lover_/article/details/66479576)
- [CSS垂直居中的11种实现方式](https://www.cnblogs.com/panda-ling/p/6323535.html)