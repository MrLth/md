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

# 2. 元素

## 2.1 行级元素

**创建**

​	display: inline | inline-block | line-table

**特性**

1. 水平排列
2. 生成 Inline-level box，参与 IFC
3. 无法设置 height，width, 设置  margin，padding 不会影响其它元素
4. 高度受 line-height 影响

## 2.2 块级元素

**创建**

​	display:block | list-item | table

**特性**

1. 视觉上呈现为块，竖直排列
2. 生成 Principal block-level box 来包含其后代盒和生成的内容，参与 BFC
3. 某些块级元素还会在主要盒之外产生额外的盒： list-item 元素。这些额外的盒会相对于主要盒来摆放

## 2.3 替换元素

​	不同于行内元素和块元素，



# 2. 上下文

## 2.1 层叠上下文

## 2.2 格式化上下文

​	formatting contexts 指页面中一个渲染区域，拥有一套渲染规则，它决定了其子元素如何定位，以及与其他元素的相互关系和作用。

### 2.2.1 BFC

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

### 2.2.2 IFC

​	Inline formatting context 行内格式化上下文，它有以下布局规则：

1. IFC 内部的盒子会在水平方向，一个接一个地放置，当一行不够时，会自动切换到下一行
2. 这些盒之间的水平 margin，border 和 padding 都有效
3. 盒可能以不同的方式竖直对齐：以它们的底部或者顶部对齐，或者以它们里面的文本的基线对齐
4. 行级上下文的高度由内部最高的内联盒子的高度决定
5. IFC 中不能有块级元素，如果插入块级元素（如 p > div），就会产生两个匿名块与 div 分隔开，即产生两个 IFC，每个 IFC 对外表现为块元素，与 div 垂直排列

### 2.2.3 FFC

​	Flex formatting context，自适应格式化上下文

**如何创建**

​	display = flex | inline-flex

**特性**

1. 渲染为一个块级元素或者行内元素
2. 由伸缩容器和伸缩项目组成，通过设置元素的 display 为 flex 来得到一个伸缩容器
3. 伸缩容器中的每一个子元素都是一个伸缩项目，不受数量限制
4. 通过属性定义伸缩项目应该如何布局

### 2.2.4 GFC

​	GrideLayout formatting context ，风格布局格式化上下文。当对一个元素设置 display 为 grid 时，此元素会获得一个独立的渲染区域

# 2. 布局

## 2.1 Normal flow

​	所有元素默认都是普通流定位，元素按在 HTML 文档中的书写顺序自上而下布局，在这个过程中，内联元素水平排列，直到当行被占满。块级元素则会被渲染为一个完整的新行。

## 2.2 Flexbox

​	弹性盒布局模型，任何一个容器都可以指定为 flex 布局，行内元素也可以使用 flex 布局，设置为 flex 布局后，子元素的 float、clear 和 veritcal-algin 都将失效。

### 2.2.1 容器属性

1. **`flex-direction`** 决定主轴的方向，常用属性 `column`
2. **`justify-content`** 决定项目在主轴上的对齐方式，常用属性 `center`
3. **`align-items`** 决定项目在交叉轴上的对齐方式，常用属性 `center`
4. **`flex-wrap`** 决定项目在一条轴线上排不下时的换行方式，默认 `nowrap`
5. **`flex-flow: <flex-direction> <flex-wrap>`**  简写方式
6. **`align-count`** 定义多根轴线的对齐方式，只有一根轴线不起作用

<img src="http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071010.png" style="zoom: 51.5%;" /> <img src="http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071011.png" style="zoom: 50%;" /><img src="http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071012.png" style="zoom: 50%;" />

> 左1 justify-content，左2 align-items，右1 align-content

### 2.2.2 项目属性

1. **`order`** 定义项目的排列顺序，数值越小，排列越靠前，默认为 0 
2. **`flex:<flex-grow> <flex-shrink> <flex-basic>`** 默认值 `0 1 auto`
3. **`align-self`** 允许单个项目有与其他项目不一样的对齐方式

<img src="http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071016.png" style="zoom: 50%;" />

# 

# 1. CSS 选择器

## 1.1 优先级

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

## 1.2 分类

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

# 2. 伪类和伪元素

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

# 3. 属性继承

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

# 5. CSS3 新增特性

## 5.1 新增伪类

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

## 5.2 新增属性

1. 圆角 `border-radius`
2. 多列布局 `column-count`
3. 阴影 `box-shadow text-shadow`
4. 反射 `box-reflect`
5. 动画 `animate`
6. 变换 `transform`
7. 过渡 `transition`
8. 全部属性 `all`

### 5.2.1 all

​	 表示所有属性（除开 unicode-bidi 和 derection），目前有 3 个值：

1. **initial** 忽略浏览器样式和用户样式，使用初始值
2. **inherit** 强制使用父元素的所有样式
3. **unset** 忽略浏览器样式和用户样式，可以继承的属性使用继承值，不可继承的属性使用初始值

# 6. 工程 & 优化

## 6.1 选择器的书写

​	样式系统从关键选择器，也就是最右边的选择器，然后依次左移匹配。这样做的好处就是减少节点的匹配，因为左边的选择器能匹配的节点总是比右边多的，从右边开始匹配能有效减少二次匹配的次数。

- 减少选择器嵌套层数
- 最右边的选择器的匹配范围应该尽量小

## 6.2 字体大小尽量选用偶数

1. 偶数字号更容易和其它元素构成比例关系，比如常用的 line-height 常用的就是 1.5 倍字号大小
2. 低版本的 IE 浏览器会强制把字号渲染成偶数，比如 13px 会渲染为 14px
3. 早期的 windows 系统是没有 13px 大小的宋体点阵