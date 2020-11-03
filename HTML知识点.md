# 1. HTML

## 1.1 DOCTYPE 的作用

​	处于HTML文档中第一行，告诉浏览器的解析器应该使用怎样的文档标准解析。

​	DOCTYPE不存在或者格式不对时将会导致文档以兼容模式呈现

​	在html5之后不再需要指定DTD文档，因为html5以前的文档都是基于SGML的，所以需要指定DTD来定义文档中允许的属性以及一些规则。

> ​	IE5.5引入了文档模式的概念，而这个概念就是通过文档类型（DOCTYPE）的切换来实现的

## 1.2 标准模式与兼容模式的区别

​	标准模式的渲染方式和JS引擎的解析方式都是以浏览器支持的最高标准运行。

​	兼容模式下，浏览器为了使站点正常工作，会以宽松的向后兼容的方式显示，模拟老式浏览器的行为。

## 1.3 SGML  |  HTML  |   XML  |   XHTML的区别

- **SGML**, `Standard Gerneralized Markup Language` 标准通用标记语言，是一种定义电子文档结构和描述其内容的国际标准语言，是所有电子文档标记语言的起源，第一版发布于1978，但由于它太复杂，因而难以普及。XML的产生就是为了简化它，以便用于更加通用的目的，比如[语义Web](https://zh.wikipedia.org/wiki/语义Web)。XML可以被认为是它的一个子集，而HTML是它的一个应用。
- **HTML**, `Hyper Text Markup Language`超文本标记语言，主要用于规定怎么显示网页
- **XML**, `Extensible Markup Language` 可扩展标记语言，第一版于1998年发布，也是未来网页语言的发展方向，和HTML最大的区别就是XML的标签是可以自己创建的，数量无限多，而HTML的标签都是固定的而且数量有限
- **XHTML**, `Extensible Hyper Markup Language` 可扩展超文本标记语言，第一版发布于2000年，相比HTML更加严格，比如标签名必须小写，所有标签必须闭合，XHTML是基于XML的，而HTML是基于SGML

## 1.4 DTD介绍

​	**`Document Type Definition`** 文档类型定义， 是一级机器可读的规则，定义XML或者HTML的特定版本中所有允许元素及它们的属性和层次关系的定义，浏览器将使用这些规则检查页面的有效性并且采取相应的措施。

DTD是对HTML文档的声明，还会影响浏览器的渲染模式（工作模式）

# 2. 元素

##  2.1 行内元素与块级元素的区别

​	**`HTML4`**中，元素被分为两大类：`inline` 及 `block`

- 格式上： 默认情况下，块级元素以新行开始，行内元素不会
- 内容上： 默认情况下，块级元素可以包含行内元素和其它块级元素，而行内元素只能包含行内元素
- 盒子模型属性上：行内元素无法设置 **`height,width`**, 设置 **`margin, padding`**不会影响其它元素，**`line-height`**有效

## 2.2 HTML5 中元素的分类

	1. Metadata
 	2. Flow
 	3. Sectioning
 	4. Heading
 	5. Embedded
 	6. Interactive

> 空元素：标签内没有内容的HTML标签称为空元素，空元素的闭合是在开始标签中的，常见的空元素有：br hr img input link meta

## 2.3 link 标签的定义

​	link标签定义文档与外部资源的关系

​	link标签的 **`rel`**属性定义了当前文档与被链接文档之前的关系，比如 `stylesheet`

## 2.4 页面导入样式时，link和@import的区别

- 从属关系， @import 是CSS提供的语法规则 ，只可导入样式表，而link是HTML的标签，不仅可以加载css文件，而且可以定义RSS、rel等属性，引入 网站图标等
- 加载时机不同，加载页面时，多个link标签引入的多个CSS文件会同时加载，而@import引入的CSS文件将在页面加载完毕后加载
- 兼容性，@import 是CSS2.1的语法，不支持IE5-， 而link是支持的
- DOM的可控性，可能动态创建link改变样式，而@import不行

# 3. 浏览器

## 3.1 浏览器内核的概念

​	主要分为两部分：渲染引擎和JS引擎

​	渲染引擎的职责就是渲染浏览器请求的内容，默认情况下，渲染引擎可以显示html、xml文档和图片。可以借助扩展或者插件显示其它类型的数据，比如PDF

​	JS引擎用于解析和执行Javsscript 脚本，最开始渲染引擎和JS引擎并没有明确的区分，后来JS引擎越来越独立，内核就倾向于只指渲染引擎

## 3.2 浏览器内核的比较

### Trident

​	IE浏览器使用的内核，早期IE占据大量市场，所以较为流行。但此内核对网页标准支持不好，并且微软已经放弃IE，大量BUG未被修复，导致此内核已经与W3C标准脱节，已经半截入土了。

### Gecko

​	FireFox和Flock使用的内核，早期内存大户，但是功能强大且丰富，可以支持很多复杂的网页效果和浏览器扩展接口

### Presto

​	Opera曾经使用的内核，当时公认的最快的内核，在处理JS脚本时，比其它内核快3倍，但为了这一目的丢掉了一部分网页的兼容性

### Webkit

​	Safari采用的内核，前身是KDE小组的KHTML引擎，可以说是KHTML的一个开源分支，速度虽不及Presto，但也比Gecko和Trident快，但是速度快的同时，兼容性也有牺牲

### Blink

​	Chrome 使用的内核，是苹果的开源浏览器核心Webkit的一个分支，目前由Google和Opera Software共同研发

## 3.3 常见浏览器内核

1. IE浏览器内核，Trident内核，俗称IE内核
2. Chrome浏览器内核，统称为Chromium内核或者Chrome内核，以前是Webkit，现在是Blink
3. FireFox浏览器内核，Gecko内核，俗称FireFox内核
4. Safari浏览器内核，Webkit内核
5. Opera浏览器内核，最初是自己的Presto内核，后来加入谷歌大军，从Webkit又到了Blink内核
6. 360浏览器、猎豹浏览器、2345浏览器内核，IE+Chrome双内核
7. 搜狗、遨游、QQ浏览器内核，Trident（兼容模式）+ Webkit（高速模式）
8. 百度浏览器、世界之窗内核，IE内核
9. UC浏览器内核，众口不一，UC说是自己研发的U3内核，但好像还是基于Webkit与Trident，还有说是基于火狐内核

## 3.4 浏览器渲染流程

1. 解析HTML文件，构建DOM Tree
2. 解析CSS文件，构建CSSOM Tree
3. 执行JavaScript 脚本
4. 根据DOM Tree与CSSOM Tree，构建Render Tree 
   1. 布局
   2. 分层

> Render Tree的节点被称为渲染对象，渲染对象是一个包含有颜色和大小的矩形，与DOM元素相对应，但不是所有DOM节点都会有对应的渲染对象，不可见的DOM元素不会加入到渲染树中

5. 准备绘制列表
6. 将绘制列表提交给合成线程进行合成，根据绘制列表生成相应图层的位置
7. 光栅化
8. 显示

## 3.5 script标签的defer与async的区别

![Bs3G9J.png](https://s1.ax1x.com/2020/11/03/Bs3G9J.png)

**相同点：**

1. 两者都是异步加载脚本，加载时不阻塞文档的解析
2. 仅对外部脚本文件有效果，对于内嵌脚本无效

**不同点：**

1. **执行时机**

   **async**

   - 在 **load**下事件 之前执行
   - 一旦加载完成，立即执行，执行时可能会阻塞文档的解析

   - 标记为**async**的多个脚本并不保证按照指定他们的先后顺序执行

   **defer**

   - 在 **DOMContentLoaded**事件 之前执行
   - 在文档解析完后再执行，提前加载完成需等待DOM Tree的构建
   - 标记为**defer**的多个脚本按顺序执行

> DOMContentLoaded 事件触发得越早，后续的操作就越快被执行，所以 async 相比 defer 会更快
>
> 异步加载样式文件，可以给<link>标签加上 media="print" 属性

## 3.6 DOMContentLoaded 和 load 事件的区别

**DOMContentLoaded ** 仅当DOM Tree准备就绪，不包含异步加载脚本、样式表和图片资源。

**load** 页面上所有的DOM，图片，样式表，脚本都已准备就绪。

## 3.7 文档的预解析

​	Webkit与FireFox都做了这个优化，当执行JavaScript脚本时，另一个线程继续解析剩下的文档，并加载后面需要通过网络加载的资源。这种方式使得资源并行加载从而提升系统整体速度。

​	**预解析只解析外部资源的引用，比如外部脚本、样式表以及图片，预解析不改变DOM树，它将这个工作留给主解析线程**

# 4. HTML5

​	HTML5 已不再是 SGML 的子集

## 4.1 新增

1. 绘画 **`canvas`**
2. 多媒体 **`video`**、**`audio`**
3. 本地存储 **`localStorage`**、**`sessionStorage`**
4. 语义化标签 **`articl`**、**`footer`**、**`header`**、**`nav`**、**`section`**
5. 表单控件 **`calendar`**、**`time`**、**`email`**、**`date`**、**`url`**、**`search`**
6. 多线程 **`webWorker`**
7. 通信 **`webSocket`**
8. 新的文档属性 **`document.visiblityState`**
9. 移除了一些元素
   - 纯表现的元素 **`basefont`**、**`big`**、**`center`**、**`font`**、**`s`**、**`strike`**、**`tt`**、**`u`**
   - 对可用性产生负面影响的元素 **`frame`**、**`frameset`**、**`noframes`**

## 4.2 html5 兼容处理

​	1. **IE8、IE7、IE6** 支持通过 **`document.createElement`** 创建标签，通过这一特性让浏览器支持 **`HTML5`** 标签，并加上标签默认的样式

 2. 使用成熟的框架，**`html5shim`** 

    ```html
    <!--[if lt IE 9]>
    <script src="http://html5shim.googlecide.com/svn/trunk/html5.js"></script>
    <![endif]-->
    ```

> `<!--[if lt IE 9]><![endif]>` 用于判断IE的版本，限定只有IE9以下的浏览器版本才执行

## 4.3 HTML语义化的好处

1. 用正确的标签做正确的事
2. 让页面内容结构化，结构更清晰，便于开发者维护
3. 拥有容易阅读的默认样式
4. 部分标签对残障人士更友好，比如屏幕阅读器对 **`<strong>`**标签会加强语气，而 **`<b>`** 标签不会
5. 利于SEO，搜索引擎的抓虫根据HTML标签来确定上下文和各个关键字的权重

### 4.3.1 b 与 strong 的区别，i 与 em 、cite 的区别

- b , 仅表示加粗
- strong, 语义化标签，表示强调，屏幕阅读器会加重语气
- i,  倾斜，独立于正文的内容，比如外来词，词语的定义
- em , 语义化标签，内容强调
- cite, 语义化标签，书名，电影名

# 5. SEO优化

1. 合理的 **`title`**、**`description`**、**`keywords`**， 搜索引擎对这三项的权重逐个减小
   - title, 一般为网页标签加上站点名，如：`css-loader | webpack`，`seo优化 - google搜索`
   - description, 把页面内容高度概括，长度合适，不同页面应有不同
   - keywords, 列举出重要关键字，逗号分隔
2. 语义化的 **`HTML`** 代码，符合 **`W3C`** 规范，让搜索引擎更容易理解网页
3. 重要内容放在最前，部分搜索引擎对抓取有长度限制
4. 重要内容不用JS输出，爬虫不会执行JS获取内容
5. 少用 **`iframe`**，爬虫不会抓取 **`iframe`** 中的内容
6. 非装饰性图片必须加上 `alt` 标签
7. 提高网站速度，网站速度是搜索引擎排序的一个重要指标

# 6. iframe 的缺点

​	iframe 元素会创建包含另外一个文档的内联框架（即行内框架）

1. iframe 会阻塞主页面的 **`onload`** 事件触发，window 的 onload 事件需要所有 iframe 加载完毕后都会触发
2. 搜索引擎爬虫不会检索 iframe 页面的内容，不利于 SEO
3. 浏览器的前进后退按钮失效
4. 不方便布局，移动端不方便显示



