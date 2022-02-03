# 1、CSS流体布局下的宽度分离原则

> 有时候我们希望盒子的宽度不会因为padding或者border的添加而改变，想让content box像水流一样自适应。因此这里可以使用“宽度分离原则”。

**所谓的“宽度分离原则”，就是CSS中的width属性不影响宽度的padding/border（有时候包括margin）属性共存。**

也就是不能出现以下组合：

```css
.box{
    width:100px;
    border:1px solid;
    padding:20px;
}
```

**这时，可以分离，也就是将 width独立占用一层标签，而padding、border、margin利用流动性在内部自适应呈现**

```css
.father{
    width:102px;
}
.son{
    margin:0 20px;
    padding: 20px;
    border:1px solid;
}
```

嵌套一层标签，**父元素定宽，子元素因为width使用的是默认值auto，所以会如流水般自动填满父级容器。**

好处：

- 不用担心尺寸的变化改变布局
- 浏览器会自动计算content box的大小

缺点：需要嵌套额外的标签。

> 会发现这个有些原则和 `box-sizing` 有些类似，但是box-sizing不支持margin box。并不能解决所有的宽度计算问题，但是宽度分离原则可以！但是`box-sizing`被发明出来最大的初衷应该是解决替换元素宽度自适应的问题。

