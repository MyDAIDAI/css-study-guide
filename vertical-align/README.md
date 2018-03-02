
 ## `vertical-align`
在[w3c](https://www.w3.org/TR/CSS21/visudet.html#propdef-vertical-align)中有这样的解释：
```
'vertical-align'
  Value:  	baseline | sub | super | top | text-top | middle | bottom | text-bottom | <percentage> | <length> | inherit
  Initial:  	baseline
  Applies to:  	inline-level and 'table-cell' elements
  Inherited:  	no
  Percentages:  	refer to the 'line-height' of the element itself
  Media:  	visual
  Computed value:  	for <percentage> and <length> the absolute length, otherwise as specified

**baseline**
Align the baseline of the box with the baseline of the parent box. If the box does not have a baseline, align the bottom margin edge with the parent's baseline.
**middle**
Align the vertical midpoint of the box with the baseline of the parent box plus half the x-height of the parent.
**sub**
Lower the baseline of the box to the proper position for subscripts of the parent's box. (This value has no effect on the font size of the element's text.)
**super**
Raise the baseline of the box to the proper position for superscripts of the parent's box. (This value has no effect on the font size of the element's text.)
**text-top**
Align the top of the box with the top of the parent's content area (see 10.6.1).
**text-bottom**
Align the bottom of the box with the bottom of the parent's content area (see 10.6.1).
**<percentage>**
Raise (positive value) or lower (negative value) the box by this distance (a percentage of the 'line-height' value). The value '0%' means the same as 'baseline'.
**<length>**
Raise (positive value) or lower (negative value) the box by this distance. The value '0cm' means the same as 'baseline'.

```
也就是说`vertical-align`适用于`inline-level and 'table-cell'`元素，且默认值为`baseline`，那么`baseline`的值为多少呢
 ### `baseline`的确定规则
 1. `inline-table`元素的`baseline`是它的`table`第一行的`baseline`
 2. 父元素`line box`的`baseline`是最后一个的`line box`的`baseline`
 3. `inline-block`元素的`baseline`确定规则:
    - 如果内部有`line box`,则`inline-block`的`baseline`就是最后一个作为内容存在的元素的`baseline`,而这个元素的`baseline`的就根据自身确定
    - 如果内部没有`line box`或它的`overflow`属性不是`visible`，那么它的`baseline`将是这个`inline-block`元素的底部`margin`边界
  那么什么是`line box`呢
  ### `line box`
  `vertical-align`被用于垂直对齐`inline`元素，`inline`元素被一个挨着一个的摆放在行内，当行内元素太多的时候，一个新的行就会被创建出来，这一行就叫做`line box`，它将行内的所有内容都包裹起来，根据行内内容的不同，所以`line box`的大小也不同。

  现在有如下代码
  ```
  <body>
    <div class="block">
      x
      <div class="child" id="child1"></div>
    </div>
  </body>
  </html>
  <style>
    .block {
      width: 120px;
      height: 120px;
      background-color: gray
    }
    .block .child {
      display: inline-block;
      width: 50px;
      height: 50px;
      background: aliceblue;
    }
  </style>
  ```
  按照上面的说法，也就说类为`child`的子元素的基线会与其父元素的基线对齐，而父元素的基线是字母x的低端，而类为`child`的子元素中没有元素，所有其基线则为底部边界
  ![vertical1](https://github.com/MyDAIDAI/css-study-guide/blob/vertical-align/vertical-align/vertical1.png)
  当我们在子元素中添加元素如下：
  ```
  <div class="block">
      x
      <div class="child" id="child1"></div>
      <div class="child" id="child2">aaa</div>
    </div>
  ```
 ![](https://github.com/MyDAIDAI/css-study-guide/blob/vertical-align/vertical-align/vertical2.png)
  添加子元素后，元素的基线就由最后一个作为内容存在的元素的`baseline`，也就说如果有多行的话，则由最后一行的`baseline`决定,如下：
  ```
   <div class="block">
      x
      <div class="child" id="child1"></div>
      <div class="child" id="child2">aaadfgdsf</div>
    </div>
  ```
 ![](https://github.com/MyDAIDAI/css-study-guide/blob/vertical-align/vertical-align/vertical3.png)
  添加多行之后，id为`child2`的子元素的`baseline`就变为最后一行的`baseline`，所以其基线也随之改变
  ### middle
  官方的解释为:`Align the vertical midpoint of the box with the baseline of the parent box plus half the x-height of the parent.`
  也就说设置为`middle`会将其垂直中点与父元素基线加上`x`一半的高度
  ```
  <body>
    <div class="block">
      X
      <div class="child" id="child1"></div>
      <div class="child" id="child2"></div>
    </div>
  </body>
  </html>
  <style>
    .block {
      width: 200px;
      height: 200px;
      background-color: gray
    }
    .block .child {
      display: inline-block;
      width: 50px;
      height: 50px;
      background: aliceblue;
      word-break:break-all;
    }
    #child1 {
      vertical-align: middle
    }
  </style>
  ```
  ![](https://github.com/MyDAIDAI/css-study-guide/blob/vertical-align/vertical-align/vertical4.png)
  其他值根据相应的英文查看
  ###`vertical-align`基于`line-box`的不同值
  当vertical-align设置为top和bottom时，其就不是按照baseline进行定位了，而是根据line box进行定位。子元素盒子的顶部和底部也就是其上下margin外边界。
  1. `top`
    将子元素盒子的顶部和其所在的line box顶部对齐
  2. `bottom`
    将子元素盒子的底部和其所在的line box底部对齐
  ### `vertical-align:middle`让元素下移而不居中的问题分析
  现在内部有三个`inline-block`块，高度分别为100px, 200px, 300px,如果想让高度为100px的块居中
  ```
  <body>
    <div class="block">
      X
      <div class="child" id="child1"></div>
      <div class="child" id="child2"></div>
      <div class="child" id="child3"></div>
    </div>
  </body>
  </html>
  <style>
    .block {
      width: 200px;
      /*height: 200px;*/
      background-color: gray
    }
    .block .child {
      display: inline-block;
      width: 50px;
      background: aliceblue;
      word-break:break-all;
    }
    #child1 {
      height: 100px;
    }
    #child2 {
      height: 200px;
    }
    #child3 {
      height: 300px;
    }
  </style>
  ```
  ![](https://github.com/MyDAIDAI/css-study-guide/blob/vertical-align/vertical-align/vertical5.png)
  我们修改id为`child1`的垂直对齐方式为`middle`
  ![](https://github.com/MyDAIDAI/css-study-guide/blob/vertical-align/vertical-align/vertical6.png)
  从上面图片我们可以看到由于其父元素的`baseline`没有改变，修改其`vertical-align`只是让其垂直中线与`baseline`和`x`的一半的和的高度对齐了，并不是我们想象的结果，那怎么办呢？
  我们试着把高度最大的元素的`vertical-align`的值设为`middle`，得到如下结果：
  ![](https://github.com/MyDAIDAI/css-study-guide/blob/vertical-align/vertical-align/vertical7.png)
  从上图中我们可以看到，由于其高度最大，整个父元素的高度由该元素撑起来，所以其父元素的`baseline`也随之改变，然后将其想要居中的元素的`vertical-align`也设为`middle`,其他元素按照布局设置为`top`或者`bottom`
  ```
   #child1 {
      height: 100px;
      vertical-align: middle;
    }
    #child2 {
      height: 200px;
      vertical-align: bottom;
    }
    #child3 {
      height: 300px;
      vertical-align: middle
    }
  ```
  ![](https://github.com/MyDAIDAI/css-study-guide/blob/vertical-align/vertical-align/vertical8.png)
  ### 衍生的垂直居中方法
  由上面居中的办法衍生出的一种垂直居中的办法就是为父元素设定一个伪类元素，其高度为父元素高度，将`vertical-align`设置为`middle`即可撑开`line-box`，改变其`baseline`的值为父元素高度的一半的位置，最后将子元素的`vertical-align`设置为`middle`即可

  参考链接：
  1. [深入理解css中vertical-align属性](https://www.cnblogs.com/starof/p/4512284.html?utm_source=tuicool&utm_medium=referral)
  2. [[翻译]关于Vertical-Align你需要知道的事情](https://segmentfault.com/a/1190000002668492)
  3. [w3c](https://www.w3.org/TR/CSS21/visudet.html#propdef-vertical-align)


