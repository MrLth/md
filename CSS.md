# 1. 单位

## 1.1 设备相关尺寸

1. **设备像素**
   设备的物理像素
2. **CSS像素**
   指逻辑像素，其值为 “设备像素 / **DPR**”，实际页面中显示的像素，基本上所有的 CSS 属性和 JavaScript 属性都是使用的 CSS像素，screen.width 和 screen.height 除外
3. **DPR**（ Device Pxiel Ratio）
   直译设备像素比，其值为 “设备像素/CSS像素”，常见的移动端高分屏一般为 2，由浏览器决定
4. **PPI** ( Pxiels Per Inch )
   每英寸像素数
5. **DPI** ( Dots Per Inch )
   每英寸点数，常用于打印设备（表墨点），在计算机上一般 1ppi 等于 1dpi

## 1.2 viewport 

### 1.2.1 Layout viewport
​	布局视口，如果把移动设备上浏览器的可视区域设为 viewport 的话，某些网页就会因为 viewport 太窄也显示错乱，所以移动端浏览器就默认把 viewport 设为一个较宽的值 ，如 980px
`document.documentElement.clientWidth`

### 1.2.2 Visual viewport
​	可视视口，指移动设备上我们可见的区域的 viewport 的大小，一般为屏幕分辨率的大小
`window.innerWidth`（在部分浏览器中无法正确获取）

### 1.2.3 Ideal viewport
​	理想视口，由于布局视口一般比可视视口大，所以要看到整个页面需要缩放和滑动窗口才行。所以提出了理想视口，让用户不用缩放和滑动窗口就可以看到整个页面，并且页面在不同分辨率下显示的内容的大小相同

​	**实际上就 CSS像素，CSS像素和 DPR 就是 idealviewport 的具体实现**

### 1.2.4 使用  meta  控制 viewport 

```html
<meta name='viewport' content='width=device-width,initial-scale=1.0,maximum-scale=1.0,user-scalable=0'>
```

​	meta viewport 最先是在苹果的移动端 safrai 引入的，安卓端浏览器后续也相继引入，目的是解决移动设备的 viewport 的问题，事实证明这的确是有用的



# 2. 盒子模型

## 2.1 组成

1. 内容 **content**
2. 填充 **padding**
3. 边框 **border**
4. 边界 **margin**

## 2.2 分类

1. **IE盒子模型** **border-box**，width，height 包含 content、border、padding

  2. **W3C**标准盒子模型 **content-box** ，width，height 只包含 content

## 2.3 box-sizing

​	在 IE8+ 的浏览器可以通过 box-sizing 来控制使用的盒子的模型，默认值为 **content-box**

# 3. 盒子

## 3.1 Inline box

### 3.1.1 Inline-level boxes

​	由非替换的 inline-level elements 生成，参与生成 IFC

### 3.1.2  Atomic inline-level boxes

​	不参与生成 IFC 的 inline-level boxes 称为原子行内级盒

### 3.1.3 Atomic inline boxes

​	正常情况下，当一个 inline box 的宽度超过 line box 时，inline box 会被拆分为多个 inline box，并分布到多个 line box 中，但原子行内盒不能被拆分，如单个文字

### 3.1.4 Anonymous inline boxes

​	block box 直接包含文字时，会生成一个匿名行内盒子将文字包裹起来

### 3.1.5 Line box

​	行框盒子是相当重要的一个概念

1. line box 的宽度受包含块和 float 元素的影响，正常情况 line box 会占满包含块的宽度，如果有 float box，就只占据到 float 连续
2. line box 的高度受 line-height 和其下最高的 inline box 影响
3. line box 内盒子的垂直对齐方式受 vertical-align 控制，默认为 base line，空盒子的 base line 在元素底
4. 当一个 inline box 的宽度超过 line box 时，inline box 会被拆分为多个 inline box，并分布到多个 line box 中，对应到 Atomic inline boxes 的概念

#### 3.1.5.1 幽灵空白节点

​	一种特殊的行内盒子，只在 HTML5 中出现，实际上也是一个盒子，不过是个假想盒，名叫“strut”，中文直译为“支柱”，是一个存在于每个“行框盒子”前面，同时**具有**该元素的字体和行高属性的 0 宽度的内联盒。

> Each line box starts with a zero-width inline box with the element’s font and line
> height properties. We call that imaginary box a “strut”.

## 3.2  block box

### 3.2.1 Block-level boxes

​	由块级元素生成，参与块级格式化上下文(BFC)。**描述元素跟它的父元素与兄弟元素之间的表现。**

### 3.2.2 Block container box

​	包含块，生成一个 BFC 或者 IFC，BFC 只包含 block-level box，IFC 只包含 inline-level box

​	有些块级盒，比如表格，不是块容器盒
​	一些块容器盒，比如非替换行内块及非替换表格单元格，不是块级盒

​	**描述元素跟它的后代之间的影响。**

### 3.2.3 Block boxes

​	同时是块容器盒的块级盒

![](https://mdn.mozillademos.org/files/3559/venn_blocks.png)

### 3.2.4 Anonymous block boxes

​	没有名字，不能被 CSS 选择符选中。块容器盒要么只包含行内级盒，要么只包含块级盒，但通常文档会同时包含两者，在这种情况下，将创建匿名块盒来包含毗邻的行内级盒。

```html
<div>
   I am Block container box
   <p>I'm Inline-level boxes</p>
   I am Block container box
</div>
<!--
	 Block box --- div 同时是 block container box 和 block-level boxes
		Anonymous block box
			Anonymous inline box
		Anonymous block box
			inline-level boxes --- p
		Anonymous block box
			Anonymous inline box
-->
```

## 3.3 参考文档

1. [CSS中各种布局的背后(*FC)_软硬皆施 - SegmentFault 思否](https://segmentfault.com/a/1190000013372963)



# 4. 包含块

​	当元素被赋予了百分比值时，它的计算值就是由这个元素的包含块计算而来的，主要包括以下属性：

1. **`height、top、bottom`**的百分比，是通过包含块的 **`height`** 来计算的

> 如果包含块没有指定 height，使包含块的 height 根据它的内容变化 ，而且包含块的 position 为 relative 或者 static ，那么这些值的计算结果为 auto

2. **`width、left、right、padding、margin`**的百分比，是通过包含块的 **`width`** 来计算的

## 4.1 确定包含块

确定一个元素的包含块的过程完全依赖于这个元素的 [`position`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/position) 属性：

1. 根元素（很多情况下可以看作是 <html>）被称为**初始包含块**，其尺寸等于浏览器视口的大小
2. 如果 position 属性为 `static` 、 `relative` 或 `sticky`，包含块可能由它的最近的祖先**块元素**（比如说inline-block, block 或 list-item元素）的内容区的边缘组成，也可能会建立格式化上下文(比如说 a table container, flex container, grid container, 或者是 the block container 自身)。
3. 如果 position 属性为 `absolute` ，包含块就是由它的最近的 position 的值不是 `static` （也就是值为`fixed`, `absolute`, `relative` 或 `sticky`）的祖先元素的内边距区的边缘组成。
4. 如果 position 属性是 **`fixed`**，在连续媒体的情况下(continuous media)包含块是 [viewport](https://developer.mozilla.org/zh-CN/docs/Glossary/Viewport) ,在分页媒体(paged media)下的情况下包含块是分页区域(page area)。
5. 如果 position 属性是 `absolute` 或 `fixed`，包含块也可能是由满足以下条件的最近父级元素的内边距区的边缘组成的：
   1.  [`transform`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform) 或 [`perspective`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/perspective) 的值不是 `none`
   2.  [`will-change`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/will-change) 的值是 `transform` 或 `perspective`
   3.  [`filter`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/filter) 的值不是 `none` 或 `will-change` 的值是 `filter`(只在 Firefox 下生效).
   4.  [`contain`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/contain) 的值是 `paint` (例如: `contain: paint;`)

## 4.2 position 为 absolute 与正常流元素的包含块的区别

1. 内联元素也可以作为包含块
2. 一般包含块所在并非父元素，而是祖辈元素中第一个 position 不为 static 的元素
3. 边界是 paddingBox 而不是 content









# 5. 元素

## 5.1 行级元素

**创建**

​	display: inline | inline-block | line-table

**特性**

1. 水平排列
2. 生成 Inline-level box，参与 IFC
3. 无法设置 height，width
4. 垂直方向的 margin 无效
5. 垂直方向的 padding 和 boder 有效，但不会影响外部元素的布局，但会产生遮挡
6. 高度受 line-height 影响

## 5.2 块级元素

**创建**

​	display:block | list-item | table

**特性**

1. 视觉上呈现为块，竖直排列
2. 生成 Principal block-level box 来包含其后代盒和生成的内容，参与 BFC
3. 某些块级元素还会在主要盒之外产生额外的盒： list-item 元素。这些额外的盒会相对于主要盒来摆放

## 5.3 替换元素

​	不同于行内元素和块元素的概念，修改元素的某个属性值，它的呈现内容就可以被替换的元素就称为“替换元素”，例如：`<img>、<object>、<vedio>、<input>、<iframe>、<select>`

**其它特性**

1. 所有替换元素都是内联元素，虽然有的是 `inline`，有的是 `inline-block`
2. 内容的外观不受 CSS 影响，除非其自身暴露的样式接口
3. 替换元素有自己的固有尺寸，如 `<vedio>、<iframe>、<canvas>`为 `300x150`
4. 在很多 CSS 样式上有自己的一套表现规则 ，如 `vertical-algin` 的 `baseline` 的含义就与非替换元素不同
   1. 非替换元素：文字基线（字符的下边缘）
   2. 替换元素：元素的下边缘

**替换元素的尺寸计算**

1. 优先使用 CSS 设置的尺寸
2. 其次使用 HTML 设置的尺寸
3. 如果元素含有固定的宽高比，同时仅设置了宽或者高，则元素依然按照固有的宽高比显示
4. 没有使用 CSS 或者 HTML 指定尺寸，则使用元素的固有尺寸

### 5.3.1 匿名替换元素 content

​	使用 content 属性可以使元素的内容可替换，也就是将非替换元素转为替换元素，类似 `h1::after{content: url('....')}`

**特性**

1. 使用 content 生成的文本无法被选中，无法被屏幕阅读器设备读取，无法被搜索引擎抓取
2. content 生成的内容无法左右 :empty 伪类
3. content 生成的内容无法被 JS 获取

> 部分浏览器可以在其它元素中使用 content 属性，大部分浏览器只允许在伪元素中使用 



# 6. 上下文

## 6.1 层叠上下文

​	stacking context ，我们假定用户正面向（浏览器）视窗或网页，而 HTML 元素沿着其相对于用户的一条虚构的 z 轴排开，**层叠上下文**就是对这些 HTML 元素的一个三维构想。众 HTML 元素基于其元素属性按照优先级顺序占据这个空间。

### 6.1.1 创建

1. 文档根元素 `<html>`
2. position: absolute | relative & z-index != auto
3. postion: fixed | sticky
4. flex 容器的子元素，且 z-index != auto
5. gird 容器的子元素，且 z-index != auto
6. opacity < 1 
7. transiform | filter | perspective | clip-path | mask | mask-image | mask-border 不为 none
8. -webkit-overflow-scrolling != touch 
   控制在移动端设备上是否有滚动回弹机制
9. will-change 不为空

### 6.1.2 层叠水平

​	stacking level，决定了同一层叠上下文中元素在 z 轴上的显示顺序。**如下图，**

- **DIV#1 z-index 5 在 DIV#4 z-index 6 之上，这是因为 DIV#4 归属于 z-index 3 的 DIV#3**
- **DIV#2 z-index 2 在 DIV#5 z-index 1 之下，这是因为 DIV#5 归属于 z-index 3 的 DIV#3**

![](https://developer.mozilla.org/@api/deki/files/913/=Understanding_zindex_04.png)

### 6.1.3 层叠顺序 

 	stacking order，表示元素发生层叠时有关特定的垂直显示顺序。
	内联盒子要比块级盒子和 float 浮动盒子层叠水平更高是因为文字和图片是重要内容，更需要被显示

![](https://segmentfault.com/img/bVbCN3c)

### 6.1.4 层叠准则

1. 谁大谁在上，当具有明显的层叠水平标识时，如生效的 z-index 属性值，在同一层叠上下文内，层叠水平大的那一个在上
2. 后来者居上，当元素间的层叠水平一致，层叠顺序相同时，在 ＤＯＭ 流中处于后面的元素会覆盖当前元素



## 6.2 格式化上下文

​	formatting contexts 指页面中一个渲染区域，拥有一套渲染规则，它决定了其子元素如何定位，以及与其他元素的相互关系和作用。

### 6.2.1 BFC

​	Block formatting context 块级格式化上下文，是一个独立的布局环境，可以理解为一个容器，在这个容器中布局的元素，不会影响到容器外的元素，外部的元素的布局也不会影响到容器内部的元素
​	**注意，BFC 属于普通流**

**如何创建**

1. 根元素或者包含根元素的元素
2. position = absolute | fixed
3. float != none
4. display = inline-block | flex | inline-flex | table-cell | table-caption
5. overflow != visible 

**其它特性**

1. 同一个 BFC 下会发生 margin 重叠
2. BFC 内部可以包含浮动元素，也就是清除浮动

### 6.2.2 IFC

​	Inline formatting context 行内格式化上下文，它有以下布局规则：

1. IFC 内部的盒子会在水平方向，一个接一个地放置，当一行不够时，会自动切换到下一行
2. 这些盒之间的水平 margin，border 和 padding 都有效
3. 盒可能以不同的方式竖直对齐：以它们的底部或者顶部对齐，或者以它们里面的文本的基线对齐
4. 行级上下文的高度由内部最高的内联盒子的高度决定
5. IFC 中不能有块级元素，如果插入块级元素（如 p > div），就会产生两个匿名块与 div 分隔开，即产生两个 IFC，每个 IFC 对外表现为块元素，与 div 垂直排列

### 6.2.3 FFC

​	Flex formatting context，自适应格式化上下文

**如何创建**

​	display = flex | inline-flex

**特性**

1. 渲染为一个块级元素或者行内元素
2. 由伸缩容器和伸缩项目组成，通过设置元素的 display 为 flex 来得到一个伸缩容器
3. 伸缩容器中的每一个子元素都是一个伸缩项目，不受数量限制
4. 通过属性定义伸缩项目应该如何布局

### 6.2.4 GFC

​	GrideLayout formatting context ，风格布局格式化上下文。当对一个元素设置 display 为 grid 时，此元素会获得一个独立的渲染区域

# 7. 布局

## 7.1 Normal flow

​	所有元素默认都是普通流定位，元素按在 HTML 文档中的书写顺序自上而下布局，在这个过程中，内联元素水平排列，直到当行被占满。块级元素则会被渲染为一个完整的新行。

## 7.2 Flexbox

​	弹性盒布局模型，任何一个容器都可以指定为 flex 布局，行内元素也可以使用 flex 布局，设置为 flex 布局后，子元素的 float、clear 和 veritcal-algin 都将失效。

### 7.2.1 容器属性

1. **`flex-direction`** 决定主轴的方向，常用属性 `column`
2. **`justify-content`** 决定项目在主轴上的对齐方式，常用属性 `center`
3. **`align-items`** 决定项目在交叉轴上的对齐方式，常用属性 `center`
4. **`flex-wrap`** 决定项目在一条轴线上排不下时的换行方式，默认 `nowrap`
5. **`flex-flow: <flex-direction> <flex-wrap>`**  简写方式
6. **`align-count`** 定义多根轴线的对齐方式，只有一根轴线不起作用

<img src="http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071010.png" style="zoom: 51.5%;" /> <img src="http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071011.png" style="zoom: 50%;" /><img src="http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071012.png" style="zoom: 50%;" />

> 左1 justify-content，左2 align-items，右1 align-content

### 7.2.2 项目属性

1. **`order`** 定义项目的排列顺序，数值越小，排列越靠前，默认为 0 
2. **`flex:<flex-grow> <flex-shrink> <flex-basic>`** 默认值 `0 1 auto`
3. **`align-self`** 允许单个项目有与其他项目不一样的对齐方式

<img src="http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071016.png" style="zoom: 50%;" />

# 

# 8. CSS 选择器

## 8.1 优先级

​	优先级分为 4 类，加上绝对优先的 **!important**

1. **A**，是否存在内联样式，取值1/0
2. **B**，ID选择器出现的次数
3. **C**，类选择器、属性选择器、伪类选择器出现的总次数
4. **D**，标签选择器、伪元素选择器出现的总次数

```css
a{color: yellow;} /*0,0,0,1*/
div a{color: green;} /*0,0,0,2*/
.demo a{color: black;} /*0,0,1,1*/
.demo input[type="text"]{color: blue;} /*0,0,2,1*/
.demo *[type="text"]{color: grey;} /*0,0,2,0*/
#demo a{color: orange;} /*0,1,0,1*/
div#demo a{color: red;} /*0,1,0,2*/
```

## 8.2 分类

1. ID 选择器
2. 类选择器
3. 属性选择器
4. 伪类选择器
5. 标签选择器
6. 伪元素选择器
7. 组合选择器
8. 通配选择器

| 选择器                                                       | 例子                  | 例子描述                                            | CSS  |
| :----------------------------------------------------------- | :-------------------- | :-------------------------------------------------- | :--- |
| [.*class*](https://www.w3school.com.cn/cssref/selector_class.asp) | .intro                | 选择 class="intro" 的所有元素。                     | 1    |
| [#*id*](https://www.w3school.com.cn/cssref/selector_id.asp)  | #firstname            | 选择 id="firstname" 的所有元素。                    | 1    |
| [*](https://www.w3school.com.cn/cssref/selector_all.asp)     | *                     | 选择所有元素。                                      | 2    |
| [*element*](https://www.w3school.com.cn/cssref/selector_element.asp) | p                     | 选择所有 <p> 元素。                                 | 1    |
| [*element*,*element*](https://www.w3school.com.cn/cssref/selector_element_comma.asp) | div,p                 | 选择所有 <div> 元素和所有 <p> 元素。                | 1    |
| [*element* *element*](https://www.w3school.com.cn/cssref/selector_element_element.asp) | div p                 | 选择 <div> 元素内部的所有 <p> 元素。                | 1    |
| [*element*>*element*](https://www.w3school.com.cn/cssref/selector_element_gt.asp) | div>p                 | 选择父元素为 <div> 元素的所有 <p> 元素。            | 2    |
| [*element*+*element*](https://www.w3school.com.cn/cssref/selector_element_plus.asp) | div+p                 | 选择紧接在 <div> 元素之后的所有 <p> 元素。          | 2    |
| [[*attribute*\]](https://www.w3school.com.cn/cssref/selector_attribute.asp) | [target]              | 选择带有 target 属性所有元素。                      | 2    |
| [[*attribute*=*value*\]](https://www.w3school.com.cn/cssref/selector_attribute_value.asp) | [target=_blank]       | 选择 target="_blank" 的所有元素。                   | 2    |
| [[*attribute*~=*value*\]](https://www.w3school.com.cn/cssref/selector_attribute_value_contain.asp) | [title~=flower]       | 选择 title 属性包含单词 "flower" 的所有元素。       | 2    |
| [[*attribute*\|=*value*\]](https://www.w3school.com.cn/cssref/selector_attribute_value_start.asp) | [lang\|=en]           | 选择 lang 属性值以 "en" 开头的所有元素。            | 2    |
| [:link](https://www.w3school.com.cn/cssref/selector_link.asp) | a:link                | 选择所有未被访问的链接。                            | 1    |
| [:visited](https://www.w3school.com.cn/cssref/selector_visited.asp) | a:visited             | 选择所有已被访问的链接。                            | 1    |
| [:active](https://www.w3school.com.cn/cssref/selector_active.asp) | a:active              | 选择活动链接。                                      | 1    |
| [:hover](https://www.w3school.com.cn/cssref/selector_hover.asp) | a:hover               | 选择鼠标指针位于其上的链接。                        | 1    |
| [:focus](https://www.w3school.com.cn/cssref/selector_focus.asp) | input:focus           | 选择获得焦点的 input 元素。                         | 2    |
| [:first-letter](https://www.w3school.com.cn/cssref/selector_first-letter.asp) | p:first-letter        | 选择每个 <p> 元素的首字母。                         | 1    |
| [:first-line](https://www.w3school.com.cn/cssref/selector_first-line.asp) | p:first-line          | 选择每个 <p> 元素的首行。                           | 1    |
| [:first-child](https://www.w3school.com.cn/cssref/selector_first-child.asp) | p:first-child         | 选择属于父元素的第一个子元素的每个 <p> 元素。       | 2    |
| [:before](https://www.w3school.com.cn/cssref/selector_before.asp) | p:before              | 在每个 <p> 元素的内容之前插入内容。                 | 2    |
| [:after](https://www.w3school.com.cn/cssref/selector_after.asp) | p:after               | 在每个 <p> 元素的内容之后插入内容。                 | 2    |
| [:lang(*language*)](https://www.w3school.com.cn/cssref/selector_lang.asp) | p:lang(it)            | 选择带有以 "it" 开头的 lang 属性值的每个 <p> 元素。 | 2    |
| [*element1*~*element2*](https://www.w3school.com.cn/cssref/selector_gen_sibling.asp) | p~ul                  | 选择前面有 <p> 元素的每个 <ul> 元素。               | 3    |
| [[*attribute*^=*value*\]](https://www.w3school.com.cn/cssref/selector_attr_begin.asp) | a[src^="https"]       | 选择其 src 属性值以 "https" 开头的每个 <a> 元素。   | 3    |
| [[*attribute*$=*value*\]](https://www.w3school.com.cn/cssref/selector_attr_end.asp) | a[src$=".pdf"]        | 选择其 src 属性以 ".pdf" 结尾的所有 <a> 元素。      | 3    |
| [[*attribute**=*value*\]](https://www.w3school.com.cn/cssref/selector_attr_contain.asp) | a[src*="abc"]         | 选择其 src 属性中包含 "abc" 子串的每个 <a> 元素。   | 3    |
| [:first-of-type](https://www.w3school.com.cn/cssref/selector_first-of-type.asp) | p:first-of-type       | 选择属于其父元素的首个 <p> 元素的每个 <p> 元素。    | 3    |
| [:last-of-type](https://www.w3school.com.cn/cssref/selector_last-of-type.asp) | p:last-of-type        | 选择属于其父元素的最后 <p> 元素的每个 <p> 元素。    | 3    |
| [:only-of-type](https://www.w3school.com.cn/cssref/selector_only-of-type.asp) | p:only-of-type        | 选择属于其父元素唯一的 <p> 元素的每个 <p> 元素。    | 3    |
| [:only-child](https://www.w3school.com.cn/cssref/selector_only-child.asp) | p:only-child          | 选择属于其父元素的唯一子元素的每个 <p> 元素。       | 3    |
| [:nth-child(*n*)](https://www.w3school.com.cn/cssref/selector_nth-child.asp) | p:nth-child(2)        | 选择属于其父元素的第二个子元素的每个 <p> 元素。     | 3    |
| [:nth-last-child(*n*)](https://www.w3school.com.cn/cssref/selector_nth-last-child.asp) | p:nth-last-child(2)   | 同上，从最后一个子元素开始计数。                    | 3    |
| [:nth-of-type(*n*)](https://www.w3school.com.cn/cssref/selector_nth-of-type.asp) | p:nth-of-type(2)      | 选择属于其父元素第二个 <p> 元素的每个 <p> 元素。    | 3    |
| [:nth-last-of-type(*n*)](https://www.w3school.com.cn/cssref/selector_nth-last-of-type.asp) | p:nth-last-of-type(2) | 同上，但是从最后一个子元素开始计数。                | 3    |
| [:last-child](https://www.w3school.com.cn/cssref/selector_last-child.asp) | p:last-child          | 选择属于其父元素最后一个子元素每个 <p> 元素。       | 3    |
| [:root](https://www.w3school.com.cn/cssref/selector_root.asp) | :root                 | 选择文档的根元素。                                  | 3    |
| [:empty](https://www.w3school.com.cn/cssref/selector_empty.asp) | p:empty               | 选择没有子元素的每个 <p> 元素（包括文本节点）。     | 3    |
| [:target](https://www.w3school.com.cn/cssref/selector_target.asp) | #news:target          | 选择当前活动的 #news 元素。                         | 3    |
| [:enabled](https://www.w3school.com.cn/cssref/selector_enabled.asp) | input:enabled         | 选择每个启用的 <input> 元素。                       | 3    |
| [:disabled](https://www.w3school.com.cn/cssref/selector_disabled.asp) | input:disabled        | 选择每个禁用的 <input> 元素                         | 3    |
| [:checked](https://www.w3school.com.cn/cssref/selector_checked.asp) | input:checked         | 选择每个被选中的 <input> 元素。                     | 3    |
| [:not(*selector*)](https://www.w3school.com.cn/cssref/selector_not.asp) | :not(p)               | 选择非 <p> 元素的每个元素。                         | 3    |
| [::selection](https://www.w3school.com.cn/cssref/selector_selection.asp) | ::selection           | 选择被用户选取的元素部分。                          | 3    |

# 9. 伪类和伪元素

1. **伪类**

   单冒号（:）用于 CSS3 伪类，伪类一般匹配元素的一些特殊状态，如 `hover`、`actived`

2. **伪元素**
   双冒号（::）用于 CSS3 伪元素，双冒号是当前规范中引入的，用于区分伪类和伪元素。不过部分浏览器也支持使用单冒号表示伪元素
   伪元素一般匹配的特殊位置并创建这些不在文档树中的元素，还可以为其设置样式，比如 `after`、`before`

> a 标签有 4 个伪类状态，
>
> 1. link 链接访问前
> 2. visited 链接访问后
> 3. hover 鼠标滑过
> 4. active 激活
>
> 因为 hover 和 active 以及 link/visited 可能会同时存放，为了避免样式覆盖，就以 LVHA 的顺序书写了

# 10. 属性继承

​	**当一个属性不是继承属性时，可以使用 inherit 关键值指定一个属性从父元素继承**，iherit 用于显示指定继承性，可用于任何继承性/非继承性属性，

> 非继承属性没有指定值时，则取属性的初始值 intialvalue，非继承属性有时又称为 resetproperty

1. 字体相关属性

   ```css
   font、font-family、font-weight、font-size、font-variant、font-stretch、font-size-adjust
   ```

2. 文本系列属性

   ```css
   text-align、line-height、letter-spacing、color、text-shadow、direction、text-transform、word-spacing、text-indent
   ```

3. 表格布局属性

   ```css
   caption-side、border-collapse、empty-cells
   ```

4. 列表属性

   ```css
   list-style
   ```

5. 光标属性

   ```css
   cursor
   ```

6. 元素可见性

   ```css
   visibility
   ```

7. 其它不常见的

   ```css
   speak、page
   ```

# 11. CSS3 新增特性

## 11.1 新增伪类

```css
elem:nth-child(n) 匹配选中父元素的第 n 个子元素，并且此子元素为 elem
elem:nth-of-type(n) 匹配选中父元素的第 n 个指定 elem 元素

elem:last-child
elem:last-of-type
elem:first-of-type

:not(elem) 
input:target 选择当前活动的 input 元素

elem:only-child
elem:only-of-type
elem:empty

:enabled 可用的表单控件
:disable 不可用的表单控件
:checked 选中的选择框元素
```

> `first-child` 是 css2 **就有的**

## 11.2 新增属性

1. 圆角 `border-radius`
2. 多列布局 `column-count`
3. 阴影 `box-shadow text-shadow`
4. 反射 `box-reflect`
5. 动画 `animate`
6. 变换 `transform`
7. 过渡 `transition`
8. 全部属性 `all`

### 11.2.1 all

​	 表示所有属性（除开 unicode-bidi 和 derection），目前有 3 个值：

1. **initial** 忽略浏览器样式和用户样式，使用初始值
2. **inherit** 强制使用父元素的所有样式
3. **unset** 忽略浏览器样式和用户样式，可以继承的属性使用继承值，不可继承的属性使用初始值

# 12. 性能优化

## 12.1 选择器的书写

​	样式系统从关键选择器，也就是最右边的选择器，然后依次左移匹配。这样做的好处就是减少节点的匹配，因为左边的选择器能匹配的节点总是比右边多的，从右边开始匹配能有效减少二次匹配的次数。

- 减少选择器嵌套层数
- 最右边的选择器的匹配范围应该尽量小

## 12.2 字体大小尽量选用偶数

1. 偶数字号更容易和其它元素构成比例关系，比如常用的 line-height 常用的就是 1.5 倍字号大小
2. 低版本的 IE 浏览器会强制把字号渲染成偶数，比如 13px 会渲染为 14px
3. 早期的 windows 系统是没有 13px 大小的宋体点阵

# 13. 工程实现

## 13.1 元素居中

### 13.1.1 宽高固定

1. `margin:auto` 实现水平居中
2. 绝对定位，`top:50%;left:50%` 加上 `负margin`
3. 绝对定位，四个方向的值都为`0`， 设置 `magin:auto`

### 13.1.2 宽高不定

4. `flex`布局，`justify-content:center;align-items:center`
5. 绝对定位，`top:50%;left:50%` 配合 `transform:translate(-50%, -50%)`

## 13.2 CSS 实现 三角形

1. border

​	元素宽高均设为 0 ，只设置 border，然后隐藏掉另外三边，剩下的就是一个三角形

2. transform:rotage(45deg)

## 13.3  CSS 实现多列等高

1. flex 布局， align-content:strech

2. display:table-cell，表格的单元格高度相等

3. position:absolute; top:0; bottom:0

4. ```scss
   .box {
       overflow:hidden // 创建 BFC ,清除浮动
   }
   .box > div{
       padding-bottom: 10000px; // 顶开背景色的高度
       margin-bottom:-10000px; // 收缩高度
       float:left; // 水平分布
   }
   ```

## 13.4 CSS + JS 实现全屏滚动

```scss
.parent{
    height:100vh;
    overflow:hidden;
    positon:relative;
}
.parent>.wrapper{
    postion:absolute;
    transition: all .1s ease
}
.wrapper>.child{
    height:100vh
}
```

## 13.5 视差滚动

​	多层背景以不同速度移动，形成立体的运动效果，带来非常出色的视觉效果。

**思路1**

1. 多层背景分别设置高度，如近 100vh ，中 75 vh ，远 50 vh。
2. 监听 mousewheel 事件，每次触发时获取 scrollTop，按比例移动三个图层，如近移动 2px，远只需要移动 1px

**思路2**

1. 使用队列保存动画函数，如 `{triggerTop:100, animateFn:()=>{}}`
2. 监听 mousewheel 事件，每次触发时获取 scrollTop，满足条件的动画函数执行并从队列中移除
3. 所有动画都执行完后停止监听

## 13.6 常见隐藏元素的方式

1. display:none 渲染树不会包含该渲染对象和其下的子元素，在页面中不占据位置，不响应绑定的监听事件
2. visibility:hidden 元素不可视，在页面中仍然占据位置，但不响应绑定的监听事件
3. opacity:0 元素不可视，在页面中仍然占据位置，响应绑定的监听事件

## 13.7 css reset 与 normalize.css 的区别

​	两者都是为了使各个浏览器渲染页面效果一致

**css reset**

​	将所有浏览器标签的自带样式重置，这样更易于保持各浏览器渲染的一致性

**normalize.css**

​	尽量保留浏览器的默认样式，不进行太多的重置，尽力让这些样式保持一致并尽可能与现代标准相符合

```scss
// css reset 部分
html, body, div, span, applet, object, iframe, h1, h2, h3, h4, h5, h6, p, blockquote, pre, a, abbr, acronym, address, big, cite, code, del, dfn, em, img, ins, kbd, q, s, samp, small, strike, strong, sub, sup, tt, var, b, u, i, center, dl, dt, dd, ol, ul, li, fieldset, form, label, legend, table, caption, tbody, tfoot, thead, tr, th, td, article, aside, canvas, details, embed,  figure, figcaption, footer, header, hgroup,  menu, nav, output, ruby, section, summary, time, mark, audio, video {  
   margin: 0;  
   padding: 0;  
   border: 0;  
   font-size: 100%;  
   font: inherit;  
   vertical-align: baseline; 
}

// normalize.css 部分
/**
 * 1. Addresses appearance set to searchfield in S5, Chrome
 * 2. Addresses box-sizing set to border-box in S5, Chrome (include -moz to future-proof)
 */
input[type="search"] {
  -webkit-appearance: textfield; /* 1 */
  -moz-box-sizing: content-box;
  -webkit-box-sizing: content-box; /* 2 */
  box-sizing: content-box;
}

/**
 * Removes inner padding and search cancel button in S5, Chrome on OS X
 */
input[type="search"]::-webkit-search-decoration,
input[type="search"]::-webkit-search-cancel-button {
  -webkit-appearance: none;
}
```

#     

# 14. 特殊属性

## 14.1  clear 清除浮动的原理

```scss
clear: both | left | right | none
```

​	从字面上看，`clear:left` 是清除左浮动，`clear:right` 是清除右浮动，实际上，这种解释并不对，因为浮动一直还在，并没有清除，官方对 clear 的解释是 **元素盒子的边不能和前面的浮动元素相邻**，换言之，clear 是为了清除浮动元素对该元素的影响，而不是清除掉浮动。

```scss
.clear::after{
	content: '';
    display: block; // clear 只对块级元素有效
    clear:both; // 父元素最后一个块不受浮动元素的影响，那么他的定位就是正常的，也就把父元素的高度撑开了
}
```

> ​	由于 float 同一时刻只能为 left 或者 right，并且 clear 对后面的浮动元素是不管的，因此，clear:right 有效的时候，clear:left 必定无效，等同于设置 clear:both；clear:left 有效的时候，clear:right 必定无效，也等同于设置 clear:both。所以在任何时候，clear:both 完全可以代替 clear:right 和 clear: left

## 14.2 zoom: 1 清除浮动的原理

​	zoom 是 IE 浏览器专有属性，它可以设置或检索对象的绽放比例。解决 IE 下比较奇葩的 BUG，如 margin 重叠、清除浮动、触发 hasLayout等。

​	当设置了 zoom 的值后，所元素的元素就会放大或者缩小，浏览器就会重新计算元素的高度和宽度，从而触发重排，通过这个原理也就解决了 IE 下子元素浮动的时候父元素不随着自动扩大的问题

> zoom 已经出现在 CSS 3.0 的草案中

## 14.3 margin:auto 的填充规则

1. 一侧 margin 定值，一侧为 auto时，auto 吃完剩余的空间

2. 两侧为 auto 时，两侧平分剩余空间

3. 要求元素具有流动性，比如普通文档流中的块元素默认情况在在水平方向是流动的，所以使用 margin: 0 auto 会填充空隙

4. 而对于绝对定位的元素，默认情况下它是不流动的，没有多余的空间让它填充， margin:auto 无效。但在设置 height + top + bottom 或者 width + left + right 后，它就处于流动状态了。如以下垂直水平居中的实现(宽高固定)

   ```scss
   .center-middle{
   	position: absolute;
       top:0;
   	bottom:0;
       right:0;
       left:0;
       height:100px;
   	width:100px;
   }
   ```

## 14.4 margin 无效的情况

1. 内联非替换元素的垂直 margin 是无效的，因为元素并非流动，所以水平方向的 auto 也不起作用
2. 表格的 `<tr>、<td>`的 margin 是无效的（display 为 table-cell 或者 table-row 的元素同理）
3. 绝对定位元素的 margin:auto 默认是无效的，同理 1 没有多余空间让 auto 起作用 

## 14.5 margin 重叠

​	块级元素的 margin-top 和另外一个元素的 margin-bottom 有时会合并，其大小为两者中最大值

**合并条件**

1. 两个元素处于常规文档流的块级盒子中，并且处于同一 BFC 中
2. 没有线盒、没有空隙、没有 padding 和 border 将它们隔开 （指父子元素）

**出现场景**

1. 父元素的 margin-top 与其第一个子元素的 margin-top
2. 元素的 margin-bottom 与其下一个兄弟元素的 margin-top
3. height 为 auto 的父元素的 margin-bottom 与其最后一个子元素的 margin-bottom
4. 高度和最小高度都为 0 的空元素的 margin-top 和 bottom

**解决办法**

  1. 相邻兄弟节点

     1. 新建一个 BFC，让两者不再处于同一 BFC 内
     2. 父元素与 第一/最后一 个子元素
     3. 父元素独立为一个新的 BFC
     4. 设置 boder-top/bottom 或者 padding-top/bottom 
     5. 使用 行内元素 作为父元素的第一个子元素
     6. 父元素与最后一个子元素

        1. 设置父元素的 height、min-height 或者 max-height
     7. 空元素

        1. 设置 border-top/bottom 或者 padding-top/bottom
        2. 添加行内元素（空格无效）
        3. 设置 height 或者 min-height

## 14.6 border 的特殊性

1. border-width 不支持百分比
2. border-style 默认值为 none，所以必需指定，否则不显示
3. border-color 的默认值为 color 的值，所以可以不指定

## 14.7 vertical-align

​	verical-align 属性影响内联盒子元素在行盒子中的垂直方向的对齐方式（还有表格别忘了），一个行盒子中的多个内联盒子的 vertical-align 值可以不同，默认都为 baseline

- middle ， baseline 到 middleline 的中心线

### 14.7.1  行内块元素的 vertical-align 表现略有不同

1. 行内块元素为空时，或者 overflow 为 visivle，可以将整个元素当作一个文字，基线在块元素下边缘，如下图第一组
2. 行内块元素内有内联元素时，或者 overflow 不为 visivle，此元素的基线就是元素里面的最后一行，如下图第一组和图二

<img src="https://pic2.zhimg.com/80/v2-4a972c9d5df5751cb21bf87f8f0a0079_720w.png" style="zoom: 80%;" />

![](https://ftp.bmp.ovh/imgs/2020/11/484b805401e3e0d3.png)

### 14.7.2 vertical-align 的取值

1. 可以为百分比，相对于 line-height 计算
2. 可以为数值，相对于基线向上或者向下偏移

### 14.7.3 table 的 vertical-align 

![](https://ftp.bmp.ovh/imgs/2020/12/52e9f04efbe27ef7.png)

## 14.8 line-height 

### 14.8.1 行距 = line-height - font-size

​	内联元素的高度由固定高度（font-size）和不固定高度（行距）来确定，line-height 就是通过改变行距来实现的，行距分为两个部分等分在文字的上下方。

### 14.8.2 line-height 的取值

1. 为数字时，为其值和 font-size 相乘的值，如 `font-size:1.5` 就是 1.5 倍字体大小
2. 为百分比值时，为其值和 font-size 相乘的值，如 `font-size:150%` 就是 1.5 倍字体大小
3. 为固定值时，保持原意不变

> 无论 line-height 取何值，该内联元素所在的行盒子的高度由行盒子的最高内联元素的高度决定

## 14.9 display 

1. `block`，块元素，宽度默认为父元素宽度，可设置宽高，独占一行
2. `none`，不显示，并从文档流中移除
3. `inline`，行内元素，默认宽度为元素宽度，不可设置宽高，同行显示，`margin`和 `padding` 只显示左右，`margin`的上下无效，`padding`的上下有效但不直接显示
4. `inline-block`，行内块元素，宽度默认为元素宽度，可设置宽高，同行显示 
5. `list-item`，块元素，并添加列表的默认样式
6. `table`，作为块级表格显示
7. `inherit`，使用父元素的值

## 14.10 position 定位原点

1. absolute

   生成绝对定位元素，相对值不为 `static` 的祖先元素的 **`paddingbox`** 定位
   注意！！！祖先元素的 `margin` 会影响定位

2. relative

   生成相对定位元素，相对于元素本身的正常位置进行定位

3. fixed

   生成绝对定位元素，相对于浏览器窗口进行定位（老 IE 不支持）

4. static

   默认。没有定位，元素正常出现流中（忽略 `top、bottom、left、right、z-index` 的声明）

## 14.11 display、postition、float 的相互关系

1. disply:none 直接从文档流中删除元素，position 和 float 完全无效
2. position 为 absolute 或者 fixed 的元素，float 属性失效，并且此时 display 的值为 table 或者 block
3. position 为 relative 的元素，且 float 不为 none，元素相对于浮动后的位置定位
4. position 为 static，float 为 none 的元素，display 才会使用设置时的值

> 简单理解就是： position 绝对定位 > float > position 相对定位 > 默认

## 14.12 width:100% 和 width:auto 的区别

1. width:100% 会让元素的宽度等于包含容器的宽度，如果此时设置了 padding、border 或者 margin ，那么元素的整体宽度就会溢出了
2. width:auto 会使元素撑满包含窗口的宽度，margin、border、padding、content 会自动进行分配

## 14.13 visibility: collapse 的作用

1. 对于一般的元素，它的表现跟 visibility:hidden 一样，元素不可见，不可点击，但仍然占用页面空间
2. 但对于 table 相关的元素，例如 table行、tablegroup、table列、tablecolumngroup，他的表现和 display:none 一样，也就是它们占用的空间也会释放

### 14.13.1 在不同浏览器的区别

1. chrome 里面，使用 collapse 和使用 hidden 没有区别
2. 在 FireFox、Opera 和 IE11 里，使用 collapse 的 table 的行会消息，它下面的一行会补充它的位置

## 14.14 clip 裁剪

```scss
position: absolute
clip: rect(top, right, bottom, left) // 4个值都为坐标值，而非距离值 
// 如一个 100*100 的图片
```

## 14.15  white-space, word-break, word-wrap 

1.  white-space, 控制空白字符的显示

| 是否能发挥作用 | 换行符 | 空格      | 自动换行 | </br>、nbsp; |
| -------------- | ------ | --------- | -------- | ------------ |
| normal         | ×      | ×（合并） | √        | √            |
| nowrap         | ×      | ×（合并） | ×        | √            |
| pre            | √      | √         | ×        | √            |
| pre-wrap       | √      | √         | √        | √            |
| pre-line       | √      | ×（合并） | √        | √            |

2. word-break: normal | break-all | keep-all, 控制单词如何被拆分换行的
3. word-wrap（overflow-wrap）, 控制长度超过一行的单词是否被拆分换行，是`word-break`的补充

**参考**

​	[彻底搞懂word-break、word-wrap、white-space - Dream_It_Possible - 博客园 (cnblogs.com)](https://www.cnblogs.com/dfyg-xiaoxiao/p/9640422.html)

## 14.16 input:-webkit-autofill

​	chrome 自动填充框伪类

```scss
input:-webkit-autofill{
	box-shadow:99999px solid #fff inset;
    // background-color 和 color 是无法修改的，只能使用其它方法替代
}
```

## 14.17 font-soomthing

​	 仅 Mac OS 下有效，并且需要加前缀（ **-webkit-** ），非标准属性，没有用的必要

## 14.18 font-style 的 italic 和 oblique 的区别

​	**italic** 会优先使用当前字体的斜体版本，**oblique** 是通过计算让文字倾斜。**litalic** 如果没有当前字体的斜体版本会解析为 **oblique**

## 14.19 transition 和 animation 的区别

​	 可以皅 transition 当作 animation 的简化版，tansition 主要关注一个属性或者多个属性从一个值到别一个值的过渡，而 animation 关注的是一个元素的变化,   可以使用关键帧控制更小粒度的变化

## 14.20 height:100% 为什么会失效

​	对于普通文档流中的元素, 如果包含块没有显式指定高度值, 它的 height 值始终为 auto, 也就是由它来撑起包含块的高度

## 14.21 min-width/max-width  之间的覆盖规则

1. max-width/min-width 会覆盖 width，即使 width 是行内样式或者 !important
2. min-width 会覆盖 max-width，此规则发生在两者冲突时