![img](https://gitee.com/guoluyan53/image-bed/raw/master/img/1618650369902-a402f0bc-d213-4330-93ea-6cb1a9bd3976.png)

# 一、基本概念

## 1、流

**”流“又叫文档流，是css的一种基本定位和布局机制**。

流是HTML的一种抽象概念，暗喻这种排列布局方式好像水流一样自然流动。“流体布局”是html默认的布局机制，如果写的HTML不用css，默认自上而下（块级元素），从左到右（内联元素）堆砌的布局方式。

## 2、块级元素和内联（行内）元素

**块级元素**是指单独撑满一行的元素，如 `div、ul、li、table、p、h1`等。这些元素的默认值是 `block、table、list-item`等。

**行内元素**是指只占它对应标签的边框所包含的空间的元素，这些元素如果父元素宽度足够则排列在一行显示，如 `span、a、em、i、img、td`等。这些元素的默认值是 `inline、inline-block、inline-table、table-cell`等。

在实际开发中，我们常常把 display计算值为 inline、inline-block、inline-table、table-cell的元素叫做内联元素，而把display计算值为block的元素称为块级元素。

## 3、width：auto和height：auto

> `width`和 `height`的默认值都是 `auto`

**对于块级元素**，流体布局之下 `width：auto`自适应撑满父元素的宽度。这里的撑满并不同于 `width:100%`的固定宽度，而是像水一样能够根据 `margin`不同而自适应父元素的宽度。

**对于内联元素**， `width:auto`则呈现出包裹性，即由子元素的宽度决定。

**无论是内联元素还是块级元素**，`height:auto`则呈现出包裹性，即高度由子级元素撑开。

- css可真有趣，正常流下，如果块级元素的 `width`是个固定值，`margin是auto`，则 `margin`会撑满剩下的空间，这也就是水平居中的效果；
- `margin`是固定值，`width`是auto，则width会撑满剩下的空间。这就是流体布局的根本所在。

> 注意：如果父元素 `height:auto`会导致子元素 `height:100%`百分比失效。如果想要子元素的 `height:100%`生效，那么父元素需要设定显示高度；或者使用绝对定位。（绝对定位元素百分比根据padding box计算），非绝对定位元素百分比根据content box计算）

## 4、盒模型

> css有两种盒模型：标准盒模型和IE盒子模型

![img](https://gitee.com/guoluyan53/image-bed/raw/master/img/1603600820746-e10daafa-451a-454e-9705-f8c358769d5b.png)

![img](https://gitee.com/guoluyan53/image-bed/raw/master/img/1603600820555-dc6ed390-d47e-412b-942a-857bbe5f280d.png)

<u>盒模型都是由四个部分组成的，分别是margin、border、padding、content</u>

标准盒模型和IE盒模型的区别在于设置width和height时，所对应的范围不同：

- 标准盒子模型：`height=width=content`
- IE盒子模型：`height=width=content+padding+border`

可以通过修改元素的`box-sizing`属性来改变盒模型：

- `box-sizing:content-box`表示标准盒模型（默认值）
- `box-sizing:border-box`表示IE盒模型（怪异盒模型）

## 5、外在盒子和内在盒子

> 在《css世界》中提出每个元素都有两个盒子（这是对元素结构和内容相关的一种形象的说法）：外在盒子和内在盒子。

**外在盒子**是决定元素排列方式的盒子，即决定盒子具有块级特性还是内联特性的盒子。外在盒子负责结构布局。

**内在盒子**是决定元素内部一些属性否生效的盒子。内在盒子负责内容显示。

如 `display:inline-table`外在盒子就是inline，内在盒子就是table。外在盒子决定了元素要像内联元素一样并排在一排显示，内在盒子决定了元素可以设置宽高、垂直方向的margin等属性。

## 6、css中可继承和不可继承属性

### 不可继承属性

1. `display`：规定元素生成框的类型
2. **文本属性**
   - `vertical-align`：垂直文本对齐
   - `text-decoration`：规定添加到文本的装饰
   - `text-shadow`：文本阴影效果
   - `white-space`：空白符处理
   - `Unicode-bidi`：设置文本的方向
3. **盒子模型的属性**：`width`、`height`、`margin`、`border`、`padding`
4. **背景属性**：`background-...`
5. **定位属性**：`float、clear、position、top、right、bottom、left、min-width、overflow、clip、z-index`
6. **生成内容属性**：`content、counter-reset、counter-increment`
7. **轮廓样式属性**：`outline-style、outline-width、outline`
8. **页面样式属性**：`size、page-break-before`
9. **声音样式属性**：`pause-before、cue-after`

### 可继承属性

1. **字体系列属性**：`font-family、font-weight、font-size、font-style`
2. **文本系列属性**：`text-indent、text-align、line-height、word-spacing、letter-spacing、text-transform、color`
3. **元素可见性**：visibility
4. **列表布局属性**：list-style、
5. **光标属性**：cursor

## 7、替换元素

> 根据“外在盒子”是内联还是块级，我们把元素分为内联元素和块级元素，而根据内容是否可替换，我们把元素分为可替换元素和非替换元素。

**替换元素**，是指内容可替换的元素。浏览器会根据元素的标签和属性，来决定元素的具体显示内容。其内容不受css视觉格式化模型控制，css渲染模型并不考虑对此内容的渲染，且元素本身一般拥有固定尺寸（宽高，宽高比）

**例子**：

1. **img**。浏览器会根据img元素的src属性加载图片信息并显示，如果进查看HTML代码，只能看到引用地址，而看不到图片的实际内容。
2. 另外，`textarea、select、iframe、video`都是替换元素。这些元素往往没有实际的内容，即是一个空元素，有些元素仅在特点情况下被作为可替换元素处理，如：`option、audio、canvas、object`等

**除了内容可替换外，还有以下特性**：

- **内容的外观不受页面上的css的影响**。用专业的话说就是在样式表 现在css作用域之外。如何更改替换元素本身外观需要类似appearance属性，或者浏览器自身暴露的一些样式接口。
- **有自己的尺寸**。在web中，很多替换元素在没有明确尺寸设定情况下，其默认的尺寸（不包括边框）是300*150像素
- **在很多css属性上有自己的默认样式**。比如 `vertical-align`属性，默认值是 `baseline`，是基线的意思，但替换元素这个属性的默认值是 `bottom`，定义为元素的下边缘。
- **所有替换元素都是内联水平元素**。也就是替换元素和替换元素、替换元素和替换文字都是可以在一行显示的。默认`display`的值是 `inline`或 `inline-block`。

**替换元素的尺寸由内而外分为三类**：

- **固有尺寸**：指的是替换内容原本的尺寸。例如，图片、视频作为一个独立文件存在的时候，都是有着自己的宽度和高度。
- **HTML尺寸**：只能通过HTML原生属性改变，这些HTML原生属性包括width和height属性、size属性。、
- **css尺寸**：特指可以通过css的width和和height或者max-width/max-width和 max-height/min-height设置尺寸，对应盒尺寸中的content box。

**这三层结构的计算规则**：

固有尺寸》HTML尺寸》css尺寸》300*150像素

## 8、对css sprites的理解

css sprites（精灵图），将一个页面涉及到的所有图片都包含到一张大图中去，然后利用css 的 `background-image`、 `background-repeat`、`background-position`属性的组合进行背景定位。也被称为 “雪碧图”

适合用于导航的背景图、按钮小图标等。

**优点**：通过整合图片，减少对服务器的请求数量，减少图片的体积从而减轻服务器的负担，提高网页的加载速度。







# 二、css基础

## 1、CSS选择器和权重

css选择器权重表：

| 权重  | 选择器                                                       |
| ----- | ------------------------------------------------------------ |
| 10000 | !important（他并不是选择器，但是权重却是最高的）             |
| 1000  | 内联样式`style=''`                                           |
| 100   | id选择器 `#idname{...}`                                      |
| 10    | 类选择器 `.classname{...}`<br />伪类选择器 `:hover{...}`<br />属性选择器 `[type="text"]={...}` |
| 1     | 标签选择器 `div{...}`<br />伪元素选择器 `:after{...}`        |
| 0     | 通用选择器（*）<br />子选择器（>）<br />相邻选择器（+）<br />兄弟选择器（~） |

在css中，`!important`的权重相当高，但是由于宽高会被 `max-width/min-width`覆盖，所以有时 `!important`会失效。

```css
width:100px !important;
min-width:200px;
```

上面代码计算之后会被引擎解析成：

```css
width:200px;
```

##  2、隐藏元素的方法有哪些？

1. `display:none`：渲染树不会包含该渲染对象，因此该元素不会在页面中占据位置，也不会响应绑定的对象。
2. `visibility:hidden`：元素在页面中仍占据空间，但是不会响应绑定的监听事件。
3. `opacity:0`：将元素的透明度设置为0，以此来实现元素的隐藏。元素在页面中仍然占据空间，并能够响应元素绑定的监听事件。
4. `position:absolute`：通过使用绝对定位将元素移除可视区域内，以此来实现元素的隐藏。
5. `z-index:负值`：来使其他元素遮盖该元素，以此来实现隐藏。
6. `clip/clip-path`：使用元素裁剪的方法来实现元素的隐藏。元素仍占据页面中的位置，但是不会影响绑定的监听事件。
7. `transform:scale(0,0)`：将元素缩放为0，来实现元素的隐藏。元素仍在页面中占据位置，但是不会响应绑定的监听事件。

## 3、display:none 和 visibility:hidden的区别

两个属性都是让元素隐藏，不可见。**两者区别如下**：

**（1）在渲染树中是否渲染**

- `display:none`会让元素完全从dom树中消失，渲染时不会占据任何空间。
- `visibility:hidden`不会让元素 从dom树中消失，渲染的元素还会占据相应的空间，只是内容不可见。

**（2）是否是继承属性**

- `display:none`是非继承属性，子孙节点会随着父节点从渲染树消失，通过修改子孙节点的属性也无法显示。
- `visibility:hidden`：是继承属性，子孙节点消失是由于继承了 `hidden`。设置 `visibility:visible`可以让子孙节点显示。

**（3）修改常规文档流中元素的 `display`通常会造成文档的<u>重排（回流）</u>，但是修改 `visibility`属性只会造成本元素的<u>重绘</u>**

**（4）如果使用读屏器，设置为 `display:none`的内容不会被读取，设置为 `visibility:hidden`的内容会被读取。**

**（5）`display:none`会影响css3的 `transition`过渡效果，`visibility:hidden`不会**

## 4、伪元素和伪类的区别和作用

- **伪元素**：在内容元素的前后插入额外的元素或样式，但是这些元素实际上并不在文档中生成。他们只在外部显示可见，但不会在文档的源代码中找到它们，因此称为“伪”元素。

```css
p::before{content:"第一个";}
p::after{content:"hot!";}
p::first-line{background:red;}
p::first-letter{font-size:90px;} /*第一个字的大小*/
```

- **伪类**：将特殊的效果添加到特定选择器上。它是已有元素上添加类别的，不会产生新的元素。

```css
a:hover{color:green;}
p:first-child{color:red;}
```

> 总结：伪类是通过在元素选择器上加入伪类改变元素状态，而伪元素通过对元素的操作进行对元素的改变。

## 5、display的block、inline、inline-block的区别

**（1）block**：会独占一行，多个元素会另起一行，可以设置width、height、margin和padding属性。

**（2）inline**：元素不会独占一行，设置width、height属性无效。但可以设置水平方向的margin和padding属性，不能设置垂直方向的margin和padding。

**（3）inline-block**：将对象设置为inline，但对象的内容作为block对象呈现，之后的内联对象会被排列在同一行内。

## 6、link和@import的区别

**两者都是外部引用css的方式**，区别如下：

1. `link`是XHTML标签，除了加载css外， 还可以定义RSS等其他事务；`@import`属于css范畴，只能加载css。
2. `link`引用css时，在页面载入时同时加载；`@import`需要页面内容完全载入以后加载。
3. `link`是XHTML标签，无兼容问题；`@import`是在css2.1提出的低版本的浏览器不支持。
4. `link`支持使用JavaScript控制dom去改变样式；而 `@import`不支持。

## 7、transition和animation的区别

- **transition是过渡属性**，强调过度，它的实现需要触发一个事件（比如鼠标移动上去，焦点，点击等）才执行动画。它类似于flash的补间动画，设置一个开始关键帧，一个结束关键帧。
- **animation是动画属性**，它的实现不需要触发事件，设定好时间之后可以自己执行，且可以循环一个动画。它也类似于flash的补间动画，但是它可以设置多个关键帧完成动画。

## 8、li和li之间看不见的空白间隔是什么原因引起的？如何解决？

我们通常会将导航栏一行放置，从而会设置li为 `inline-block`。而且为了代码美观，我们通常会一个li标签放一行。但是这导致 `li标签`产生换行字符，它就变成了一个空格，占用了一个字符的宽度。

```html
<style>
    ul{
        width: 100px;
    }
    li{
        background-color: aquamarine;
        display: inline-block;
    }
</style>
<body>
    <ul>
        <li>ds</li>
        <li>d</li>
        <li>sd</li>
        <li>fs</li>
    </ul>
    <ul>
        <li>ds</li><li>d</li><li>sd</li><li>fs</li>
    </ul>
</body>
```

![image-20220125172917586](https://gitee.com/guoluyan53/image-bed/raw/master/img/image-20220125172917586.png)

**解决方法**：

1. 为 `<li>`设置 `float:left`。不足：有些容器是不能设置浮动，如左右切换的焦点图等。
2. 将所有 `<li>`写在同一行。不足：代码不美观。
3. 将 `<ul>`内的字符尺寸直接设为0，即 `font-size:0`。不足： `<ul>`中的其他字符尺寸也被设置为0，需要额外重新设定其他字符尺寸，且在Safari浏览器依然会出现空白间隔。
4. 消除 `<ul>`的字符间隔`letter-spacing:-8px`。不足：这也设置了 `<li>`内的字符间隔。因此需要将 `<li>`内的字符间隔设为默认 `letter-spacing:normal`

## 9、为什么有时候用translate来改变位置而不是定位？

translate是transform属性的一个值。改变transform或opacity不会触发浏览器重新布局（回流reflow）或重绘（repaint），只会触发复合（compositions）。而改变**绝对定位会触发回流，进而触发重绘和复合**。

transform使浏览器为元素创建一个GPU图层，但改变绝对定位会使用到CPU。因此translate更高效，可以缩短平滑动画的绘制时间。而**translate改变位置时，元素依然会占据其原始空间**，绝对定位不会发生这种情况。

## 10、margin和padding的使用场景

- 需要在border外侧添加空白，且空白处不需要背景（色）时，使用margin。（上下相连的两个盒子之间的空白，需要相互抵消时）
- 需要在border内侧添加空白，且空白处需要背景（色）时，使用padding。（上下相连的两个盒子之间的空白，希望等于两者之和时）。

## 11、对line-height的理解







