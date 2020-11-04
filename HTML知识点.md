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

## ２.5 常用的 meta 标签

meta标签有下面的作用：

1. 搜索引擎优化（SEO）
2. 定义页面使用语言
3. 自动刷新并指向新的页面
4. 实现网页转换时的动态效果
5. 控制页面缓冲
6. 网页定级评价
7. 控制网页显示的窗口

```html
<meta charset='utf-8'> <!--声明文档使用的字符编码-->
<meta http-equiv='X-UA-Compatiale' content='IE-edge, chrome=1'> <!--优先使用 IE 最新版本和 Chrome-->
<meta name=”description” content=”不超过 150 个字符”/> <!--页面描述-->
<meta name=”keywords” content=””/> <!--页面关键词-->
<meta name=”author” content=”name, email@gmail.com”/> <!--网页作者-->
<meta name=”robots” content=”index,follow”/> <!--robots用来告诉搜索机器人哪些页面需要索引，哪些页面不需要索引-->
<meta name=”viewport” content=”initial-scale=1, maximum-scale=3, minimum-scale=1, user-scalable=no”> <!--为移动设备添加 viewport-->
<meta name=”apple-mobile-web-app-title” content=”标题”> <!--iOS 设备 begin-->
<meta name=”apple-mobile-web-app-capable” content=”yes”/> <!--添加到主屏后的标题（iOS 6新增）-->
<!--是否启用 WebApp 全屏模式，删除苹果默认的工具栏和菜单栏-->
<meta name=”apple-itunes-app” content=”app-id=myAppStoreID, affiliate-data=myAffiliateData, app-argument=myURL”>
<!--添加智能 App 广告条 Smart App Banner（iOS 6+ Safari）-->
<meta name=”apple-mobile-web-app-status-bar-style” content=”black”/>
<meta name=”format-detection” content=”telphone=no, email=no”/> <!--设置苹果工具栏颜色-->
<meta name=”renderer” content=”webkit”> <!--启用 360 浏览器的极速模式(webkit)-->
<meta http-equiv=”X-UA-Compatible” content=”IE=edge”> <!--避免 IE 使用兼容模式-->
<meta http-equiv=”Cache-Control” content=”no-siteapp” /> <!--不让百度转码-->
<meta name=”HandheldFriendly” content=”true”> <!--针对手持设备优化，主要是针对一些老的不识别 viewport 的浏览器，比如黑莓-->
<meta name=”MobileOptimized” content=”320″> <!--微软的老式浏览器-->
<meta name=”screen-orientation” content=”portrait”> <!--uc 强制竖屏-->
<meta name=”x5-orientation” content=”portrait”> <!--QQ 强制竖屏-->
<meta name=”full-screen” content=”yes”> <!--UC 强制全屏-->
<meta name=”x5-fullscreen” content=”true”> <!--QQ 强制全屏-->
<meta name=”browsermode” content=”application”> <!--UC 应用模式-->
<meta name=”x5-page-mode” content=”app”> <!--QQ 应用模式-->
<meta name=”msapplication-tap-highlight” content=”no”> <!--windows phone 点击无高光-->
<!--设置页面不缓存-->
<meta http-equiv=”pragma” content=”no-cache”>
<meta http-equiv=”cache-control” content=”no-cache”>
<meta http-equiv=”expires” content=”0″>
<!--自动刷新并指向新页面, 停留2秒钟后自动刷新到URL网址-->
<meta http-equiv="Refresh" content="2;URL=http://www.jb51.net">
<!--强制页面在当前窗口以独立页面显示, 用来防止别人在框架里调用自己的页面-->
<meta http-equiv="Window-target" content="_blank">
<!--显示语言的设定-->
<meta http-equiv="Content-Language" content="zh-cn"/>
<!--说明网站采用的什么软件制作-->
<meta name="generator" content="信息参数"/>
<!--网站版权信息-->
<meta name="copyright" content="信息参数">
<!--是否显示图片工具栏-->
<meta http-equiv="imagetoolbar" content="false"/>
```

### ２.5.1 SEO优化

```html
<meta charset="utf-8">
<meta http-equiv="X-UA-Compatible" content="IE=Edge">
<meta name="viewport" content="width=device-width, initial-scale=1.0,user-scalable=no">

<!-- Start of Baidu Transcode -->
<meta http-equiv="Cache-Control" content="no-siteapp" />
<meta http-equiv="Cache-Control" content="no-transform" />
<meta name="applicable-device" content="pc,mobile">
<meta name="MobileOptimized" content="width"/>
<meta name="HandheldFriendly" content="true"/>
<meta name="mobile-agent" content="format=html5;url=https://www.jianshu.com/p/b1334dc59dfb">
<!-- End of Baidu Transcode -->

<meta name="description"  content="webpack配置文件，增加插件transform-es3-property-literals和transform-es3-member-expression-literal">

<meta name="360-site-verification" content="604a14b53c6b871206001285921e81d8" />
<!-- Apple -->
<meta name="apple-mobile-web-app-title" content="简书">

<!--  Meta for Smart App Banner -->
<meta name="apple-itunes-app" content="app-id=888237539, app-argument=jianshu://notes/10802047">
<!-- End -->

<!--  Meta for Twitter Card -->
<meta content="summary" property="twitter:card">
<meta content="@jianshucom" property="twitter:site">
<meta content="Babel转ES5后IE8下的兼容性问题解决方案" property="twitter:title">
<meta content="1、webpack配置文件，增加插件transform-es3-property-literals和transform-es3-member-expression-liter..." property="twitter:description">
<!-- End -->
<!--  Meta for Facebook Applinks -->
<meta property="al:ios:url" content="jianshu://notes/10802047" />
<meta property="al:ios:app_store_id" content="888237539" />
<meta property="al:ios:app_name" content="简书" />

<meta property="al:android:url" content="jianshu://notes/10802047" />
<meta property="al:android:package" content="com.jianshu.haruki" />
<meta property="al:android:app_name" content="简书" />
<!-- End -->
```

[参考博客](https://www.cnblogs.com/qiumohanyu/p/5431859.html)

## 2.6 input 的 disabled 与 readonly的区别

​	disabled 禁用此元素，input 内容不会随着表彰提交

​	readonly 规定输入字段为只读，会随着表单提交

​	！！！注意，无论是 readonly 还是 disabled，都可以使用 JS 更改它的内容

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

### 4.3.2 title 和 h1 的区别

​	**定义**

​		title 是网站标题，一个页面只能有一个

​		h1 是文章主题

​	**作用**

​		title 高度概括网页信息，告诉搜索引擎和用户这个页面是关于什么内容和主题的，直接是显示在浏览器标签栏上

​		h1 突出文章主题，对页面信息的抓取也有很大的影响，h1是在没有外界干扰下除 title 以外第二个能强调页面主旨的标记

> 如果 title 为空，浏览器会尝试以 h1 的内容作为标签页标题

## 4.4 autocomplete

​	浏览器的自动填充

​	autocomplete 适用于 **`<form>`**以及下面的 **`<input>`**类型：**`text、search、url、telephone、email、password、detepickers、range、color`**

## 4.5 canvas 与 svg 的区别

**canvas**

1. 是一种通过 JavaScript 来绘制 2D 图形的方式
2. 标量图， 逐像素渲染，缩放会失真

**svg**

1. 使用 xml 描述 2D 图形的语言
2. 由于是基于 xml，所以 svg DOM 的每个元素都是可用的，可以为其添加事件监听函数
3. 保存的是图形的绘制方法，缩放不会失真

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

# 7. 实现浏览器多个标签页之间的通信

​	思路就是通过 **中介者模式** 实现，因为标签页之间无法直接通信

## 7.1 webSocket

​	两个标签页连接同一个服务器，以服务器作为中转。

## 7.2 localStroage

​	通过 **`storage`** 事件，以 **`localStorage`**作为中转

## 7.3 同一浏览上下文组 ( browsing context group)

​	在当前标签页打开的标签页可以使用 window.opener 访问原标签页的 window，使用 window.open 返回的 newWindow 可以访问打开标签页的 window

- `const newWindow = window.open('url...')`
- `<a href='url...'>...</a>`

```typescript
// 处于同一浏览上下文的两个标签页可以通过 opener.postMessage 方法和 message 事件通信
const referrer = window.open('url...')
referrer.addEventListener("message", (e)=>console.log(e)) // 'hello'
referrer.postMessage({a:1,b:[1,2,3]}, '*') 
```

```typescript
window.addEventListener("message", (e)=>console.log(e)) // {a:1,b:[1,2,3]}
opener.postMessage('hello', '*')
```

[postMessage. MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/postMessage)

> 注意！！！只有同时属于浏览上下文和满足同源策略的两个标签页，才会被分配到同一渲染进程，才可以互相操作 DOM
>
> <a rel='noopener noreferrer'/>
> 使用这种方式打开的新标签，就算原/新标签页是同一站点，也会分别使用不同的渲染进程
>
> noopener
> 告诉浏览器，通过此A标签打开的新标签页中的opener设为null
>
> noreferrer
> 告诉浏览器，新打开的标签页不要有引用关系

## 7.4 sharedWorker

​	Safari 和 IE 不支持

```typescript
// sharedWorker.js
var clients = [];
onconnect = function(e) {
    var port = e.ports[0];
    clients.push(port);
    port.addEventListener('message', function(e) {
        for (var i = 0; i < clients.length; i++) {
            var eElement = clients[i];
            eElement.postMessage(e.data)
        }
    });
    port.start();
}
```

```typescript
// 需要接收信息的标签页
myWorker = new SharedWorker("script/scenesetting/ShareWorker.js")
myWorker.port.onmessage=function(e) {
    var result=e.data;//此处就是共享现成推送过来的数据可以是字符串、数组、json
    /***********上面拿到数据后，就可以在下面做一些你想造做的事************/
};
myWorker.port.postMessage(newData) // 推送消息
```

# 8. Page Visibility API

## 8.1 以往的API

​	以往监听用户离开页面的行为，通过以下三个事件实现

​	**事件：** **`pagehide`** 、 **`beforeunload`**、**`unload`**

​	**问题：** 这些事件在移动端支持度不高，因为手机系统可以将一个进程直接转入后台，然后为了节省资源就会杀死。

## 8.2  使用场景 

​	以前，开发者想要指定一种在任何情况下页面卸载都会执行的代码，是无法做到的。因为页面被系统切换以及系统清除浏览器进程是无法监听到的（主要在手机端）。而 Page Visiblity API 不管在手机还是PC端，都能监听到页面的可见性的变化。

- 监听网页的可见性，预判网页的卸载
- 页面不可见时暂停页面的行为，节省资源。比如对服务器的轮询、网页动画、正在播放的音频或视频

## 8.3 API

```typescript

document.visibilityState: 'hidden' | 'visible' | 'prerender' 
// 1. 只要页面可见，哪怕仅露出一个角，值都是 visible
// 2. 以下情况会返回 hidden
// 		a. 浏览器最小化
//		b. 浏览器没有最小化，但是当前页面切换到了其它标签页
//		c. 浏览器将要卸载页面（unload）
// 		d. 操作系统将触发锁屏屏幕
// 3. prerender 只在支持预渲染的浏览器出现
// 注意！！！ 内嵌的 <iframe> 页面的 document.visibilityState 属性由顶层窗口决定

document.hidden:boolean
// 1. 出于历史原因保留，应尽量使用 document.visibility
// 2. 仅在 document.visibility 为 visible 时返回 true，其余情况均返回 false

// 事件
document.addEventListener('visibilitychange', ()=>{})
```

## 8.4 页面卸载

​	页面卸载分为有三种情况，前两种情况 **`pagehide`** 、 **`beforeunload`**、**`unload`** 都可以监听到，第三种只能 **`visibilitychange`** 才能监听到

1. 页面可见时，用户关闭标签页或浏览器窗口
2. 页面可见时，用户在当前窗口前往另一个页面
3. 页面不可见时，用户或系统关闭浏览器进程

> **`beforeunload`** 适合在用户修改了表单，没有提交就离开当前页面
>
> 注意！！！ 指定了 unload 和 beforeunload 事件的监听函数，浏览器不会缓存当前页面

## 9. webSocket 兼容低版本浏览器

1. Adobe Flash Socket
2. ( IE ) ActiveX HTMLFile
3. 基于 multipart 编码发送 XHR
4. 基于长轮询的 XHR

# 9. 渐进增强与优雅降级的定义

**渐进增强**：针对低版本浏览器进行构建页面，保证最基本的功能，然后再针对高级浏览器 进行效果、交互等改进和追加功能达到更好的用户体验。 

**优雅降级**：一开始就根据高版本浏览器构建完整的功能，然后再针对低版本浏览器进行兼容。

# 10. Web 标准中，可用性、可访问性、可维护性的理解

**可用性** Usability

​	从用户的角度看待产品的质量，产品是否容易上手，用户是否能完成任务，完成效率以及使用过程的用户体验

**可访问性** Accessibility

​	Web 内容对于残障用户的可阅读和可理解性

**可维护性** Maintainability

	1. 当系统出问题时，快速定位并解决问题的成本，成本低则可维护性好
 	2. 代码是否容易被人理解，是否容易修改和增强功能

# 11. 浏览器架构

1. 浏览器基本服务
   1. Profile 进程
   2. UI 进程
   3. GPU 进程
   4. 网络进程
   5. 文件进程
   6. 设备进程
   7. Audio 进程
   8. Video 进程
   9. ...
2. 浏览器主进程
3. 渲染进程 * n
4. 插件进程 * n

> 内存不足时，会将基本服务里面的进程作为服务合并到浏览器主进程中

# 12. css reset 与 normalize.css 的区别

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

# 13. 获取位置信息

```typescript
if ("geolocation" in navigator) {
    var watchID = navigator.geolocation.watchPosition(function(position) {
      console.log(position)
    });
    navigator.geolocation.getCurrentPosition(function(position) {
      do_something(position.coords.latitude, position.coords.longitude);
    });
} else {
  /* 地理位置服务不可用 */
}
```

