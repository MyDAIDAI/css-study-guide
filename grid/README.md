# grid布局
`grid`栅格布局，具有强大的内容尺寸和定位能力，是一种基于二维网格的布局系统，旨在改变我们设计基于网格的用户界面的方式，弥补网页开发在二维布局能力上的缺陷

与flex分为伸缩容器与伸缩项目类似，grid也分为网格容器和网格项目
## 网格容器与其属性
将`display`属性设置为`grid`或`inline-grid`创建一个网格容器，网格容器中的所有子元素就变为网格项目
```
<body>
  <div>
    <div class="grid-container">
      <div class="child child1">item1</div>
      <div class="child child2">item2</div>
      <div class="child child3">item3</div>
    </div>
    <span>123</span>
  </div>
</body>
</html>
<style>
  .grid-container {
    display: grid;
    border: 1px solid #000000
  }
  .child {
    border: 1px solid #000000
  }
</style>
```
当`display:grid`时，类为`grid-container `的grid网格容器占据父元素宽度，网格项目均占据相同宽度依次向下排列
![grid1](https://github.com/MyDAIDAI/css-study-guide/blob/grid/grid/grid1.png)

当`display:inline-grid`时，网格容器与网格项目包裹内容，后续节点向上排列
![grid2](https://github.com/MyDAIDAI/css-study-guide/blob/grid/grid/grid2.png)

### 设置网格的列和行
使用`grid-template-columns`和`grid-template-rows`属性设置一个网格列的宽度与一个网格行的高度，默认值为`none`
```
.grid-container {
    display: grid;
    border: 1px solid #000000;
    grid-template-rows: 60px 40px;
  }
```
上面的代码中设置了两行的行高，由于只定义了两个高度值，其余的高度是根据内容的高度来定义

通过指定`grid-template-columns`指定值来创建每列的列宽，指定多少列宽，就创建多少列，网格项中的内容依次填充进入网格
```
<body>
  <div>
    <div class="grid-container">
      <div class="child child1">item1</div>
      <div class="child child2">item2</div>
      <div class="child child3">item3</div>
      <div class="child child4">item4</div>
    </div>
    <span>123</span>
  </div>
</body>
</html>
<style>
  .grid-container {
    display: grid;
    border: 1px solid #000000;
    grid-template-columns: 80px 40px;
  }
  .child {
    border: 1px solid #000000
  }
</style>
```
![grid3](https://github.com/MyDAIDAI/css-study-guide/blob/grid/grid/grid3.png)
也就是说在`grid`布局中使用`grid-template-columns`和`grid-template-rows`来画网格的列宽和行高，也就是相应的轨道值，当没有指定值得时候，默认为一行一列，其值为相应的内容值，当指定了列宽后，会将网格项按照列依次填充

![grid4](https://github.com/MyDAIDAI/css-study-guide/blob/grid/grid/grid4.png)

上图中指定了两列的宽度值，则其中的网格项依次填充完两列，不指定行高值时行高为内容高度值，指定行高，则相应位置的行高进行变化

![grid5](https://github.com/MyDAIDAI/css-study-guide/blob/grid/grid/grid5.png)

### repeat()
可以使用`repeat()`创建重复的轨道，适用于创建相当尺寸的网格项目，接受两个参数，第一个为重复次数，第二个为定义每个轨道的尺寸
```
    grid-template-columns: repeat(2, 40px);
```
上述代码相当于:`grid-template-columns: 40px 40px`
### `fr`单位
`fr`单位允许将轨道大小设置为网格容器自由空间的一部分，下面代码会使每个网格项目列宽为容器的三分之一
```
  grid-template-columns: 1fr 1fr 1fr
```
![grid6](https://github.com/MyDAIDAI/css-study-guide/blob/grid/grid/grid6.png)

注意：自由空间是排除又有不可伸缩的值之后得到的，比如，下面的代码中的`fr`的自由空间总量不包括`40px`
```
  grid-template-columns: 1fr 40px 1fr
```
![grid7](https://github.com/MyDAIDAI/css-study-guide/blob/grid/grid/grid7.png)

### `grid-template-areas`
通过引用`grid-area`属性指定的网格区域的名称来定义网格模板，重复网格区域的名称导致内容扩展到这个单元格，点号表示一个空的单元格
```
<body>
  <div>
    <div class="grid-container">
      <div class="child child1">header</div>
      <div class="child child2">main</div>
      <div class="child child3">sidebar</div>
      <div class="child child4">footer</div>
    </div>
    <span>123</span>
  </div>
</body>
</html>
<style>
  .grid-container {
    display: grid;
    border: 1px solid #000000;
    grid-template-columns: 50px 50px 50px 50px;
    grid-template-areas: 
      "header header header header"
      "main main . sidebar"
      "footer footer footer footer"
  }
  .child {
    border: 1px solid #000000
  }
  .child1 {
    grid-area: header;
  }
  .child2 {
    grid-area: main;
  }
  .child3 {
    grid-area: sidebar;
  }
  .child4 {
    grid-area: footer
  }
</style>
```
![grid8](https://github.com/MyDAIDAI/css-study-guide/blob/grid/grid/grid8.png)
上面的代码中首先将网格项命名，然后在网格区域模板中使用这些命名网格项即可，点号表示空白网格

### `grid-template`
可以使用`grid-template`同时声明`grid-template-rows、grid-template-columns、grid-template-areas `，其值为：
 - none - 将三个属性都设置为其初始值
 - subgrid - 把 grid-template-rows 和 grid-template-columns 设置为 subgrid, 并且 grid-template-areas 设置为初始值
 - `grid-template-rows / grid-template-columns` - 把 grid-template-columns 和 grid-template-rows 设置为指定值, 与此同时, 设置 grid-template-areas 为 none

### `grid-column-gap / grid-row-gap`
指定网格线的大小，可以想象为设置列/行之间的间距的宽度
```
<body>
  <div>
    <div class="grid-container">
      <div class="child child1">item1</div>
      <div class="child child2">item2</div>
      <div class="child child3">item3</div>
      <div class="child child4">item4</div>
    </div>
    <span>123</span>
  </div>
</body>
</html>
<style>
  .grid-container {
    display: grid;
    border: 1px solid #000000;
    grid-gap: 10px 15px;
    grid-template-columns: 40px 50px;
  }
  .child {
    border: 1px solid #000000
  }
</style>
```
![grid9](https://github.com/MyDAIDAI/css-study-guide/blob/grid/grid/grid9.png)
### `grid-gap`
`grid-gap`即为`grid-row-gap`和`grid-column-gap`的缩写
上面的代码与下面等同：
` grid-gap: 10px 15px;`

当只设置`grid-gap: 10px;`时，其`grid-row-gap`与`grid-column-gap`值相同

### `justify-items`与`align-items`
沿着行轴对齐网格内的内容（与之对应的是 align-items, 即沿着列轴对齐），该值适用于容器内的所有的 grid items。其值为：
- start: 内容与网格区域的左端对齐
- end: 内容与网格区域的右端对齐
- center: 内容位于网格区域的中间位置
- stretch: 内容宽度占据整个网格区域空间(这是默认值)
也可以给单个`grid item`设置`justify-self`以及`align-items`属性来达到上述效果

### `justify-content`与`align-content`
当网格的总大小小于其容器大小时，可以设置其`justify-content`与`align-content`来设置整个网格与网格容器的对齐方式，其值为：
- start - 网格与网格容器的左边对齐
- end - 网格与网格容器的右边对齐
- center - 网格与网格容器的中间对齐
- stretch - 调整g rid item 的大小，让宽度填充整个网格容器
- space-around - 在 grid item 之间设置均等宽度的空白间隙，其外边缘间隙大小为中间空白间隙宽度的一半
- space-between - 在 grid item 之间设置均等宽度空白间隙，其外边缘无间隙
- space-evenly - 在每个 grid item 之间设置均等宽度的空白间隙，包括外边缘

### `grid-auto-columns`与`grid-auto-rows`
指定自动生成的网格轨道（又名隐式网格轨道）的大小，隐式网格轨道在显示定位超出指定网格范围的行或列时被创建

```
<body>
  <div>
    <div class="grid-container">
      <div class="child child1">item1</div>
      <div class="child child2">item2</div>
      <div class="child child3">item3</div>
      <div class="child child4">item4</div>
    </div>
    <span>123</span>
  </div>
</body>
</html>
<style>
  .grid-container {
    display: grid;
    border: 1px solid #000000;
    grid-template-columns: 60px 60px;
    grid-template-rows: 90px 90px;
    grid-auto-columns: 60px;
  }
  .child {
    border: 1px solid #000000
  }
  .child1 {
    grid-column: 1 / 2;
    grid-row: 2 / 3;
  }
  .child2 {
    grid-column: 5 / 6;
    grid-row: 2 / 3;
  }
</style>
```
在上面的代码中我们创建了一个2*2的网格，分别为`child1`与`child2`指定其相应的网格位置，但是`child2`指定的网格位置不是我们开始创建的，这个时候就会自动创建`child2`所需要的网格，这个时候就可以使用`grid-auto-columns`和`grid-auto-rows`属性来指定这些隐式轨道的宽度和高度，如下：
![grid10](https://github.com/MyDAIDAI/css-study-guide/blob/grid/grid/grid10.png)

### `grid-auto-flow`
若没有显示的指明放置网格上的`grid item`，则自动放置算法会自动放置这些项目，而该属性会用户控制自动布局算法的工作方式，其值为：
- row, 依次填充每行，根据需要添加新行
- column，依次填充每列，根据需要添加新列
- dense

## 网格项目(grid item)与其属性
下面是网格项目也就是`grid item`所支持的属性
### `grid-column-start / grid-column-end / grid-row-start /grid-row-end`
使用指定的网格线确定`grid item`在网格内的位置，`grid-column-start/grid-row-start `属性表示`grid item`的网格线的起始位置，`grid-column-end/grid-row-end`属性表示网格项在网格线的终点位置，其值为：
- line: 可以是一个数字来指代相应编号的网格线，也可使用名称指代相应命名的网格线
- span <number>: 网格项将跨越指定数量的网格轨道
- span <name>: 网格项将跨越一些轨道，直到碰到指定命名的网格线
- auto: 自动布局， 或者自动跨越， 或者跨越一个默认的轨道

### `grid-column / grid-row`
grid-column-start + grid-column-end, 和 grid-row-start + grid-row-end 的简写形式。其值为：
- `<start-line> / <end-line>`: 每个值的用法都和属性分开写时的用法一样

### `grid-area`
给`grid-item`进行命名以便使用`grid-template-areas`属性创建模板时来进行引用。也可以作为`rid-row-start + grid-column-start + grid-row-end + grid-column-end `的简写

命名：
```
.item-d {
  grid-area: header
}
```
简写：
```
.item-d {
  grid-area: 1 / col4-start / last-line / 6
}
```

### `justify-self / align-self`
沿着行轴对齐grid item 里的内容（与之对应的是 align-self, 即沿列轴对齐）。此属性对单个网格项内的内容生效，其值为：
- start - 将内容对齐到网格区域的左端
- end - 将内容对齐到网格区域的右端
- center - 将内容对齐到网格区域的中间
- stretch - 填充网格区域的宽度 (这是默认值)

参考链接：
- [CSS Grid 系列(上)-Grid布局完整指南](https://segmentfault.com/a/1190000012889793#articleHeader4)
- [grid栅格布局](https://www.cnblogs.com/xiaohuochai/p/7083153.html#anchor1)