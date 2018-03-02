# 宽度不确定，高度随宽度变化，维持宽高比不变
元素宽度不确定，并且要维持元素宽高比不变，主要是修改元素的`padding`属性，需要注意的是，当一个元素的`padding`属性为百分比时，那么这个百分比是相对父元素的宽度而来，所以我们可以在不同的情况下设置这个属性来达到我们的目的
```
<body>
    <div>
    </div>
</body>
</html>
<style>
  body {
    width: 100%;
  }
  div {
    width: 20%;
    background: green;
  }
  div:before {
    content: "";
    display: inline-block;
    padding-bottom: 200%;
    width: .1px;
  }
</style>
```
上面的代码中设置了`padding-bottom`为`div`元素宽度的2倍，所以宽高比为1:2
也可以不使用伪类，如下：
```
<body>
    <div class="father">
      <div class="son"></div>
    </div>
</body>
</html>
<style>
  .father {
    width: 100%;
  }
  .son {
    width: 20%;
    height: 0;
    padding-top: 40%;
    background: red
  }
</style>
```
上面的代码中类为`son`的元素的宽度为父元素的20%，其高度设置为0,使用`padding-top`为父元素的40%，实现元素宽高比为1:2，注：都是相对于父元素的比值,由于元素的高度全部由`padding`属性撑起，所以其中不能添加内容，在里面添加内容需要使用绝对定位的方式
