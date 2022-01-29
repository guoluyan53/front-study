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

## 9、对媒体查询的理解

**媒体查询**是由一个可选的媒体类型和零个或多个使用媒体功能的限制了样式表范围的表达式组成，例如宽度、高度和颜色。媒体查询，添加自css3，允许内容呈现针对一个特定范围的输出设备而进行裁剪，而不必改变内容本身，适合web网页应对不同型号的设备做出对应的响应适配。

媒体查询包含一个可选的媒体类型和满足css3规范的条件下，包含零个或多个表达式，这些表达式描述了媒体特征，最终会被解析为true或false。如果媒体查询中指定的媒体类型匹配展示文档所使用的设备类型，并且所有的表达式的值都是true，那么该媒体查询的结果为true，那么该媒体查询结果为true。那么媒体查询内的样式将会生效。

```html
<!--link元素中的css媒体查询-->
<link rel="stylesheet" media="(max-width:800px)" href="aaa.css"/>
<!--样式表中的css媒体查询-->
<style>
    @media (max-width:600px){
        .aaa{
            display:none;
        }
    }
</style>
```

简单来说，使用@media查询，可以针对不同的媒体类型定义不同的样式。@media可以针对不同的屏幕尺寸设置不同的样式，特别是需要设计响应式的页面，@media是非常有效的。当重置浏览器大小的过程中，页面也会根据浏览器的宽度和高度重新渲染页面。





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

**（1）line-height的概念**

- line-height指一行文本的高度，包括了字间距，实际上是下一行基线到上一行基线距离
- 如果一个标签没有定义height属性，那么最终表现的高度由line-height决定
- 一个容器没有设置高度，那么撑开容器高度的是line-height，而不是容器的文本内容
- 把line-height值设置为height一样大小的值可以实现单行文字的垂直居中
- line-height和height都能撑开一个高度

**（2）line-height的赋值方式**

- 带单位：px是固定值，而em会参考父元素font-size值计算自身的行高
- 纯数字：会把比例传递给后代。例如，父级行高为1.5，子元素字体为18px，则子元素行高为1.5*18=27px
- 百分比：将计算后的值传递给后代

## 12、单行、多行文本溢出隐藏

- **单行文本溢出**：

```css
{
    overflow:hidden;   //溢出隐藏
    text-overflow:ellipsis;  //溢出用省略号表示
    white-space:nowrap;   //规定段落中的文本不进行换行
}
```

- **多行文本溢出**：

```css
{
    overflow:hidden;    //溢出隐藏
    text-overflow: ellipsis;   //溢出用省略号显示
    display: -webkit-box;   //作为弹性伸缩盒子模型显示
    -webkit-box-orient:vertical;   //设置伸缩盒子的子元素排列方式：从上到下垂直排列
    -webkit-line-clamp:3;    //显示的行数
}
```

> 注：由于上面的三个属性都是css3的属性，没有浏览器可以兼容，所以要在前面加一个 `-webkit-`来兼容一部分浏览器。

## 13、z-index在什么情况下会失效

通常 z-index 的使用是在有两个重叠的标签，在一定的情况下控制其中一个在另一个的上方或者下方出现。z-index值越大就越是在上层。z-index元素的position属性需要是relative，absolute或是fixed。

**z-index属性在下列情况下会失效**：

- 父元素position为relative时，子元素的z-index失效。解决：父元素position改为absolute或static；
- 元素没有设置position属性为非static属性。解决：设置该元素的position属性为relative，absolute或是fixed中的一种；

- 元素在设置z-index的同时还设置了float浮动。解决：float去除，改为display：inline-block；

# 三、css进阶

## 1、css优化和提高性能的方法有哪些？

**加载性能**：

1. **css压缩**：将写好的css进行压缩，可以减小文件体积
2. **css单一样式**：当需要下边距和左边距的时候，很多时候会选择使用`margin:top 0 bottom 0;`但`margin-bottom:bottom；margin-left:left；`执行效率会更高
3. **减少使用@import，建议使用link**。因为后者在页面加载的时候一起加载，前者是等待页面加载完成之后再进行加载。

**选择器性能**：

1. **关键选择器（key selector）。**选择器的最后面的部分为关键选择器（即用来匹配目标元素的部分）。css选择器是从左到右进行匹配的。当使用后代选择器时，浏览器会遍历所有子元素来确定是否是指定的元素等。
2. 如果规则拥有**ID选择器**作为其关键选择器，则不要为规则增加**标签选择器**。过滤掉无关的规则（这样样式系统就不会浪费时间去匹配他们了）
3. **避免使用通配规则**。如 `*{}`计算次数惊人，只对需要用到的元素进行选择。
4. 尽量少的去对**标签选择器**进行选择，而是使用**class选择器**
5. 尽量少的去使用后代选择器，降低选择器的权重值。后代选择器的开销是最高的，尽量将选择器的深度降到最低，最高不要超过三层，更多的使用**类**来关联每一个标签元素。
6. 了解哪些属性是可以通过继承而来的，然后避免对这些属性重复指定规则。

**渲染性能**：

1. 慎重使用高性能属性：浮动，定位
2. 尽量减少页面重排、重绘
3. 去除空规则：{}。空规则产生的原因一般来说是为了预留样式。去除这些空规则无疑能减少css文档体积。
4. 属性值为0时，不加单位
5. 属性值为浮动小数0.XX，可以忽略小数点前的0.
6. 标准化各种浏览器前缀：带浏览器前缀的在前。标准属性在后
7. 不使用@import前缀，会影响css的加载速度
8. 选择器优化嵌套，尽量避免层级过深
9. css雪碧图，同一页面相近部分的小图标，方便使用，减少页面的请求次数，但是同时图片本身会变大，优劣考虑清楚，再使用。
10. 正确使用display的属性，由于display的作用，某些样式组合会无效，徒增样式体积的同时也影响解析性能。
11. 不滥用web字体。对于中文网站来说webfonts可能很陌生，国外却很流行。web fonts通常体积庞大，而且一些浏览器在下载web fonts时会阻塞页面渲染损伤性能。

**可维护性、健壮性**：

1. 将具有相同属性的样式抽离出来，整合并通过class在页面中进行使用，提高css的可维护性。
2. 样式与内容分离：将css代码定义到外部css中。

## 2、css预处理器/后处理器是什么？为什么要使用他们？

**预处理器**：如 `less`、`sass`、`stylus`，用来预编译 `sass`或者 `less`，增加了css代码的复用性。层级，mixin，变量，循环，函数等对编写以及开发、UI组件都极为方便。

**后处理器**：如 `postCss`，通常是在完成的样式表中根据 css 规范处理 css，让其更有效，目前最常做的是给css属性添加浏览器私有前缀，实现跨浏览器兼容性的问题。

`css`预处理器为`css`增加一些编程特性，无需考虑浏览器的兼容问题，可以在`CSS`中使用变量，简单的逻辑程序，函数等在编程语言中的一些基本的性能，可以让`css`更加的简洁，增加适应性以及可读性，可维护性等。

其它`css`预处理器语言：`Sass（Scss）`, `Less`, `Stylus`, `Turbine`, `Swithch css`, `CSS Cacheer`, `DT Css`。



使用原因：

- 结构清晰， 便于扩展
- 可以很方便的屏蔽浏览器私有语法的差异

- 可以轻松实现多重继承
- 完美的兼容了`CSS`代码，可以应用到老项目中

## 3、对css工程化的理解

CSS 工程化是为了解决以下问题：

1. **宏观设计**：CSS 代码如何组织、如何拆分、模块结构怎样设计？
2. **编码优化**：怎样写出更好的 CSS？

1. **构建**：如何处理我的 CSS，才能让它的打包结果最优？
2. **可维护性**：代码写完了，如何最小化它后续的变更成本？如何确保任何一个同事都能轻松接手？



以下三个方向都是时下比较流行的、普适性非常好的 CSS 工程化实践：

- 预处理器：Less、 Sass 等；
- 重要的工程化插件： PostCss；

- Webpack loader 等 。



基于这三个方向，可以衍生出一些具有典型意义的子问题，这里我们逐个来看：

**（1）预处理器：为什么要用预处理器？它的出现是为了解决什么问题？**

预处理器，其实就是 CSS 世界的“轮子”。预处理器支持我们写一种类似 CSS、但实际并不是 CSS 的语言，然后把它编译成 CSS 代码：

![img](https://cdn.nlark.com/yuque/0/2021/jpeg/1500604/1615998492170-c294084b-84d5-4537-87bb-b32da4bf0cd6.jpeg)

那为什么写 CSS 代码写得好好的，偏偏要转去写“类 CSS”呢？这就和本来用 JS 也可以实现所有功能，但最后却写 React 的 jsx 或者 Vue 的模板语法一样——为了爽！要想知道有了预处理器有多爽，首先要知道的是传统 CSS 有多不爽。随着前端业务复杂度的提高，前端工程中对 CSS 提出了以下的诉求：

1. 宏观设计上：我们希望能优化 CSS 文件的目录结构，对现有的 CSS 文件实现复用；
2. 编码优化上：我们希望能写出结构清晰、简明易懂的 CSS，需要它具有一目了然的嵌套层级关系，而不是无差别的一铺到底写法；我们希望它具有变量特征、计算能力、循环能力等等更强的可编程性，这样我们可以少写一些无用的代码；

1. 可维护性上：更强的可编程性意味着更优质的代码结构，实现复用意味着更简单的目录结构和更强的拓展能力，这两点如果能做到，自然会带来更强的可维护性。



这三点是传统 CSS 所做不到的，也正是预处理器所解决掉的问题。预处理器普遍会具备这样的特性：

- 嵌套代码的能力，通过嵌套来反映不同 css 属性之间的层级关系 ；
- 支持定义 css 变量；

- 提供计算函数；
- 允许对代码片段进行 extend 和 mixin；

- 支持循环语句的使用；
- 支持将 CSS 文件模块化，实现复用。

**（2）PostCss：PostCss 是如何工作的？我们在什么场景下会使用 PostCss？**

PostCss 仍然是一个对 CSS 进行解析和处理的工具，它会对 CSS 做这样的事情：

![img](https://gitee.com/guoluyan53/image-bed/raw/master/img/1615998491947-34e3237c-e54f-4b1a-8aeb-3c38655e1cb0.jpeg)

它和预处理器的不同就在于，预处理器处理的是 类CSS，而 PostCss 处理的就是 CSS 本身。Babel 可以将高版本的 JS 代码转换为低版本的 JS 代码。PostCss 做的是类似的事情：它可以编译尚未被浏览器广泛支持的先进的 CSS 语法，还可以自动为一些需要额外兼容的语法增加前缀。更强的是，由于 PostCss 有着强大的插件机制，支持各种各样的扩展，极大地强化了 CSS 的能力。



PostCss 在业务中的使用场景非常多：

- 提高 CSS 代码的可读性：PostCss 其实可以做类似预处理器能做的工作；
- 当我们的 CSS 代码需要适配低版本浏览器时，PostCss 的 [Autoprefixer](https://github.com/postcss/autoprefixer) 插件可以帮助我们自动增加浏览器前缀；

- 允许我们编写面向未来的 CSS：PostCss 能够帮助我们编译 CSS next 代码；

**（3）Webpack 能处理 CSS 吗？如何实现？**

Webpack 能处理 CSS 吗：

- **Webpack 在裸奔的状态下，是不能处理 CSS 的**，Webpack 本身是一个面向 JavaScript 且只能处理 JavaScript 代码的模块化打包工具；
- Webpack 在 loader 的辅助下，是可以处理 CSS 的。



如何用 Webpack 实现对 CSS 的处理：

- Webpack 中操作 CSS 需要使用的两个关键的 loader：css-loader 和 style-loader
- 注意，答出“用什么”有时候可能还不够，面试官会怀疑你是不是在背答案，所以你还需要了解每个 loader 都做了什么事情：

- - css-loader：导入 CSS 模块，对 CSS 代码进行编译处理；
  - style-loader：创建style标签，把 CSS 内容写入标签。

在实际使用中，**css-loader 的执行顺序一定要安排在 style-loader 的前面**。因为只有完成了编译过程，才可以对 css 代码进行插入；若提前插入了未编译的代码，那么 webpack 是无法理解这坨东西的，它会无情报错。

## 4、如何判断元素是否到达可视区域

以图片显示为例：

- `window.innerHeight` 是浏览器可视区的高度；
- `document.body.scrollTop || document.documentElement.scrollTop` 是浏览器滚动的过的距离；

- `imgs.offsetTop` 是元素顶部距离文档顶部的高度（包括滚动条的距离）；
- 内容达到显示区域的：`img.offsetTop < window.innerHeight + document.body.scrollTop;`

![img](https://gitee.com/guoluyan53/image-bed/raw/master/img/1603966605254-fe880ec0-ebd1-4f94-b662-cdd5e5396c34.png)

# 四、页面布局

## 1、常见的页面布局单位

常见的布局单位包括像素 `px`，百分比`%`，`em`，`rem`，`vh/vw`

**（1）像素px**是页面布局的基础，一个像素表示终端屏幕所能显示的最小的区域，像素分为两种类型：css像素和物理像素。

- css像素：为web开发者提供，在css中使用的一个抽象单位
- 物理像素：只与硬件设备密度有关，任何设备的物理像素都是固定的。

**（2）百分比%**：当浏览器的宽度或者高度发生变化时，通过百分比单位可以使得浏览器中的组件的宽和高随着浏览器的变化而变化，从而实现响应式效果。一般子元素的百分比相对于直接父元素。

**（3）em和rem**：相对于px更灵活，他们都是相对长度单位，他们之间的区别：**em相对于父元素，rem相对于根元素**。

- **em**：文本相对长度单位。相对于当前对象内文本的字体尺寸。如果当前行内文字的字体尺寸未被人为设置，则相对于浏览器的默认字体尺寸（默认16px）。（相对于父元素字体大小的倍数）
- **rem**：rem是css3新增的一个相对单位，相对于根元素（html元素）的font-size的倍数。作用：利用rem可以实现简单的响应式布局，可以利用html元素中字体的大小与屏幕间的比值来设置font-size的值，以此实现当屏幕分辩率变化时让元素也随之变化。

**（4）vw和vh**是与视口有关的单位，vw表示相对于视图窗口的宽度，vh表示相对于视图窗口高度，除了vw和vh外，还有**vmin**和v**max**两个相关的单位。

- vw：相对于视窗的宽度，视窗宽度是100vw
- vh：相对于视窗的高度，视窗的高度时100vh
- vmin：vw和vh中的较小值
- vmax：vw和vh中的较大值

> `vw/vh`和百分比很类似，两者的区别：
>
> - 百分比：大部分相对于祖先元素，也有相对于自身的情况如（border-radius、translate）等
> - vw/vh：相对于视窗的尺寸

## 2、两栏布局的实现

> 一般两栏布局指的是左边一栏宽度固定，右边一栏宽度自适应。

**（1）利用`float+margin-left`**：利用浮动，将左边元素宽度设置为200px，并且设置向左浮动。将右边元素的margin-left设置为200px，宽度为auto（默认为auto，撑满整个父元素）。

```css
.outer {
  height: 100px;
}
.left {
  float: left;
  width: 200px;
  background: tomato;
}
.right {
  margin-left: 200px;
  width: auto;
  background: gold;
}
```

**（2）利用`float+overflow:hidden`**：利用浮动，左侧元素设置固定大小，并左浮动，右侧元素设置overflow:hidden。这样右边就触发了BFC，BFC的区域不会与浮动元素发生重叠，所以两侧就不会发生重叠。

```css
.left{
     width: 100px;
     height: 200px;
     background: red;
     float: left;
 }
 .right{
     height: 300px;
     background: blue;
     overflow: hidden;
 }
```

**（3）利用flex布局**，将左边元素设置固定宽度，右边元素设置为 `flex:1`

```css
.outer {  //父元素
  display: flex;
  height: 100px;
}
.left {
  width: 200px;
  background: tomato;
}
.right {
  flex: 1;
  background: gold;
}
```

**（4）利用绝对定位，将父级元素设置为相对定位。左边元素设置为absolute定位，并且宽度设置固定。将右边元素的margin-left设置为左边元素的宽度**

```css
.outer {
    position: relative;
    height: 100px;
}
left {
    position: absolute;
    width: 200px;
    height: 100px;
    background: tomato;
}
.right {
    margin-left: 200px;
    height: 100px;
    background: gold;
}
```

**（5）利用绝对定位，将父级元素设置为相对定位。左边元素宽度设置固定，右边元素设置为绝对定位，左边定位为左边元素宽度，其余方向定位为0**

```css
.outer {
  position: relative;
  height: 100px;
}
.left {
  width: 200px;
  background: tomato;
}
.right {
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 200px;
  background: gold;
}
```

## 3、三栏布局的实现

> 三栏布局一般指的是页面中一共有3栏， **左右两栏宽度固定，中间自适应布局**

**（1）利用绝对定位，左右两栏设置为绝对定位，中间设置对应方向大小的margin的值**

```css
.outer {
  position: relative;
  height: 100px;
}

.left {
  position: absolute;
  width: 100px;
  height: 100px;
  background: tomato;
}

.right {
  position: absolute;
  top: 0;
  right: 0;
  width: 200px;
  height: 100px;
  background: gold;
}

.center {
  margin-left: 100px;
  margin-right: 200px;
  height: 100px;
  background: lightgreen;
}
```

**（2）利用flex布局，左右两栏设置固定大小，中间一栏设置为flex:1**

```css
.outer {
  display: flex;
  height: 100px;
}

.left {
  width: 100px;
  background: tomato;
}

.right {
  width: 100px;
  background: gold;
}

.center {
  flex: 1;  //中间自适应
  background: lightgreen;
}
```

**（3）利用浮动，左右两栏设置固定大小，并设置对应方向的浮动**。中间一栏设置左右两个方向的margin值，注意这种方式**，中间一栏必须放到最后(在html页面中)：**

```css
.outer {
  height: 100px;
}

.left {
  float: left;
  width: 100px;
  height: 100px;
  background: tomato;
}

.right {
  float: right;
  width: 200px;
  height: 100px;
  background: gold;
}

.center {
  height: 100px;
  margin-left: 100px;
  margin-right: 200px;
  background: lightgreen;
}
```

**（4）圣杯布局**。利用浮动和负边距来实现。父级元素设置左右的 padding，三列均设置向左浮动，中间一列放在最前面，宽度设置为父级元素的宽度，因此后面两列都被挤到了下一行，通过设置 margin 负值将其移动到上一行，再利用相对定位，定位到两边。

```css
.main{
    padding: 0 100px;  /*设置左右内边距为左右部分的宽度*/
}
/*清除浮动,防止高度塌陷*/
.main::after {
    content: '';
    display: block;
    clear: both;  
}
.center,.left,.right{
    height: 229px;
    float: left;
    text-align: center;
    color: aliceblue;
    background-color: cadetblue;
}
.left,.right{
    position: relative;
    width: 100px;
    background-color: rgb(41, 48, 45);
}
.left{
    left: -100px;   /*结合相对定位，偏移移原来的位置到左边，不然会覆盖中间元素的内容*/
    margin-left: -100%; 
}
.center{
   width: 100%;
}
.right{
   right: -100px;
   margin-left: -100px;
}
```

**（5）双飞翼布局**：双飞翼布局给主面板添加了一个父标签来通过margin给子标签腾出空间。

双飞翼布局相对于圣杯布局来说，左右位置的保留是通过中间列的margin值来实现的，而不是通过父元素的padding来实现的。本质上说，也是通过浮动和外边距负值来实现的。

```html
<style>
        .centerfather{
            width: 100%;
        }
        /*清除浮动,防止高度塌陷*/
        .main::after {
            content: '';
            display: block;
            clear: both;  
        }
        .centerfather,.left,.right{
            height: 229px;
            float: left;
            text-align: center;
            color: aliceblue;
            background-color: cadetblue;
        }
        .left,.right{
            width: 100px;
            background-color: rgb(41, 48, 45);
        }
        .left{
            margin-left: -100%; 
        }
        .center{
            margin: 0 100px;
        }
        .right{
            margin-left: -100px;
        }
</style>
</head>
<body>
    <div class="main">
        <div class="centerfather">
            <div class="center"></div>
        </div>
        <div class="left"></div>
        <div class="right"></div>
    </div>
</body>
</html>
```

## 4、实现水平垂直居中

**（1）absolute + translate**。该方法需要考虑浏览器的兼容问题。

先将元素的左上角通过top:50%和 left:50%定位到页面的中心，然后通过translate来调整元素的中心点到页面的中心。

```css
.parent{
    position:relative;
}
.child{
    position:absolute;
    left:50%;
    top:50%;
    transform: translate(-50%,-50%);
}
```

**（2）absolute + margin**。 该方法适用于盒子**有宽高**的情况。

利用绝对定位，设置四个方向的值都为0，并将margin设置为auto，由于宽高固定，因此对应方向实现平分，可以实现水平和垂直方向上的居中。

```css
.parent{
    position:relative;
}
.child{
    width:100px;
    height:100px;
    position:absolute;
    top:0;
    bottom:0;
    left:0;
    right:0;
    margin:auto;
}
```

**（3）absolute + margin-top**。适用于盒子宽高已知的情况。

```css
.parent{
    position:relative;
}
.child{
    width:100px;
    height:100px;
    position:absolute;
    top:50%;
    left:50%;
    margin-top:-50px;  /*自身宽高的一半*/
    margin-left:-50px;
}
```

**（4）flex + margin:auto**

```css
.parent{
    display:flex;
}
.child{
    width:100px;
    height:100px;
    margin:auto;
}
```

**（5）flex**。要考虑兼容问题，该方法在移动端用的较多。

```css
.parent{
    display:flex;
    align-items:center;
    justify-content:center;
}
```



