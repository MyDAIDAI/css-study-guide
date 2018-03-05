# 常用布局
## 两列&三列布局
两列布局的特征是侧栏固定宽度，主栏自适应宽度。
三列布局的特征是两侧两列的固定宽度，中间列自适应宽度。
### `float+margin`
原理：设置两个侧栏分别向左向右浮动，中间列通过外边距给两个侧栏腾出空间，中间列的宽度根据浏览器窗口实现自适应
```
<body>
  <div class="content">
    <div class="sub">sub</div>
    <div class="extra">extra</div>
    <div class="main">main</div>
  </div>
</body>
</html>
<style>
  .sub {
    width: 100px;
    float: left;
    height: 200px;
    border: 1px solid #000000;
  }
  .extra {
    width: 200px;
    float: right;
    height: 200px;
    border: 1px solid #000000
  }
  .main {
    margin-left: 110px;
    margin-right: 210px;
  }
</style>
```
### `absolute+margin`
将两侧栏使用绝对定位分别布局在页面两端，中间main使用`margin`为两侧栏腾出位置
```
<body>
  <div class="content">
    <div class="sub">sub</div>
    <div class="extra">extra</div>
    <div class="main">main</div>
  </div>
</body>
</html>
<style>
  .sub {
    width: 100px;
    position: absolute;
    left: 0;
    height: 200px;
    border: 1px solid #000000;
  }
  .extra {
    width: 200px;
    position: absolute;
    right: 0;
    height: 200px;
    border: 1px solid #000000
  }
  .main {
    margin-left: 110px;
    margin-right: 210px;
  }
</style>
```
### 圣杯布局（float+负margin）
原理：主面板设置宽度为100%,主面板与两个侧栏都设置浮动，常见为左浮动，这个时候两个侧栏会被挤下去，再通过负边距将浮动的侧栏拉上来，左侧的侧栏的负边距为100%,右侧的侧栏的负边距为其宽度值，为了避免侧栏挡住主体内容，在主面板外层设置其padding值为相应宽度值,最后设置两个侧边栏为相对定位，将其定位到对应位置即可
- 设置宽度，三者都设置向左浮动
```
<body>
  <div class="content">
    <div class="main">main</div>    
    <div class="sub">sub</div>
    <div class="extra">extra</div>
  </div>
</body>
</html>
<style>
  .main {
    width: 100%;
    float: left;
    border: 1px solid #000000;
    box-sizing: border-box
  }
  .sub {
    float: left;
    width: 190px;
    border: 1px solid #000000;
    box-sizing: border-box    
  }
  .extra {
    float: left;
    width: 230px;
    border: 1px solid #000000;
  }
</style>
```
 ![layout1](https://github.com/MyDAIDAI/css-study-guide/blob/layout/layout/layout1.png)

 如上图所示，三个元素根据位置依次排列
- 设置负边距，根据侧栏位置设置负边距
```
.main {
    width: 100%;
    float: left;
    border: 1px solid #000000;
    box-sizing: border-box
  }
  .sub {
    float: left;
    width: 190px;
    border: 1px solid #000000;
    box-sizing: border-box;
    margin-left: -100%; 
  }
  .extra {
    float: left;
    width: 230px;
    border: 1px solid #000000;
    box-sizing: border-box;    
    margin-left: -230px;
  }
```
 ![layout2](https://github.com/MyDAIDAI/css-study-guide/blob/layout/layout/layout2.png)

 设置负边距后两个侧栏都移动到了main的左边与右边
 - main父元素设置padding
  为了避免主界面被侧栏遮挡，需要为main的父元素设置padding值，这样就可以为侧栏腾出位置
```
.content {
    padding: 0 230px 0 190px;
  }
```
 ![layout3](https://github.com/MyDAIDAI/css-study-guide/blob/layout/layout/layout3.png)
- 最后将左右侧边栏移动到父元素腾出来的位置上
```
.sub {
    float: left;
    width: 190px;
    border: 1px solid #000000;
    box-sizing: border-box;
    margin-left: -100%; 
    position: relative;
    left: -190px;
  }
  .extra {
    float: left;
    width: 230px;
    border: 1px solid #000000;
    box-sizing: border-box;    
    margin-left: -230px;
    position: relative;
    right: -230px;
  }
```
![layout4](https://github.com/MyDAIDAI/css-study-guide/blob/layout/layout/layout4.png)

注意：
1. DOM元素的书写顺序不能更改
2. 主面板部分优先渲染
3. 当面板的main内容部分比两边的子面板宽度小的时候，布局就会混乱，可以通过设置main的min-width属性或者使用双飞翼布局来避免这个问题

### 双飞翼布局(float + 负margin)
原理：双飞翼布局与圣杯布局的思想有些相似，都利用了浮动和负边距，但是双飞翼布局对圣杯布局上做了一些改进，在main元素上加了一层div,并且设置了margin,利用margin为两个侧栏腾出位置而非父元素的padding,由于两侧栏的负边距都是相对于`main-wrap`而言，所以`main`的`margin`值不会影响两个侧栏，所以省掉了对两个侧栏设置相对布局的步骤
```
<body>
  <div class="main-warp">
      <div class="main">main</div>        
  </div>
    <div class="sub">sub</div>
    <div class="extra">extra</div>
</body>
</html>
<style>
  .content {
    padding: 0 230px 0 190px;
  }
  .main-warp {
    width: 100%;
    float: left;
    box-sizing: border-box;
  }
  .main {
    margin: 0 230px 0 190px;
  }
  .sub {
    float: left;
    width: 190px;
    border: 1px solid #000000;
    box-sizing: border-box;
    margin-left: -100%; 
  }
  .extra {
    float: left;
    width: 230px;
    border: 1px solid #000000;
    box-sizing: border-box;    
    margin-left: -230px;
  }
</style>
```
圣杯主要使用父元素设置padding值得方式来放置侧边栏，而双飞翼主要使用为main添加父元素，在main上设置margin值来实现。

### flex布局
```
<body>
  <div class="content">
    <div class="sub">sub</div>
    <div class="main">main</div>            
    <div class="extra">extra</div>
  </div>
</body>
</html>
<style>
  .content {
    display: flex;
  }
  .sub {
    width: 190px;
    border: 1px solid #000000;
    box-sizing: border-box
  }
  .extra {
    width: 230px;
    border: 1px solid #000000;
    box-sizing: border-box;
  }
  .main {
    flex: 1
  }
</style>
```
`flex: 1`为自由空间分配
### grid布局
```
<body>
  <div class="content">
    <div class="sub">sub</div>
    <div class="main">main</div>            
    <div class="extra">extra</div>
  </div>
</body>
</html>
<style>
  .content {
    display: grid;
    grid-auto-flow: row;
    grid-template-columns: 190px 1fr 230px;
    grid-template-areas: 
      "sub main extra"
  }
  .sub {
    border: 1px solid #000000;
    box-sizing: border-box;
    grid-area: sub;
  }
  .extra {
    border: 1px solid #000000;
    box-sizing: border-box;
    grid-area: extra;
  }
  .main {
    grid-area: main;
  }
</style>
```
`使用1fr`可以实现自由空间分配