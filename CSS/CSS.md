





1. 



# display 的值

1. `block`，块元素，宽度默认为父元素宽度，可设置宽高，独占一行
2. `none`，不显示，并从文档流中移除
3. `inline`，行内元素，默认宽度为元素宽度，不可设置宽高，同行显示，`margin`和 `padding` 只显示左右，`margin`的上下无效，`padding`的上下有效但不直接显示
4. `inline-block`，行内块元素，宽度默认为元素宽度，可设置宽高，同行显示 
5. `list-item`，块元素，并添加列表的默认样式
6. `table`，作为块级表格显示
7. `inherit`，使用父元素的值

# position 定位原点

## 1. absolute

 	生成绝对定位元素，相对值不为 `static` 的祖先元素的 **`paddingbox`** 定位

​	注意！！！祖先元素的 `margin` 会影响定位

## 2. relative

​	生成相对定位元素，相对于元素本身的正常位置进行定位

## 3. fixed

​	生成绝对定位元素，相对于浏览器窗口进行定位（老 IE 不支持）

## 4. static

​	默认。没有定位，元素正常出现流中（忽略 `top、bottom、left、right、z-index` 的声明）

# display、postition、float 的相互关系

1. disply:none 直接从文档流中删除元素，position 和 float 完全无效
2. position 为 absolute 或者 fixed 的元素，float 属性失效，并且此时 display 的值为 table 或者 block
3. position 为 relative 的元素，且 float 不为 none，元素相对于浮动后的位置定位
4. position 为 static，float 为 none 的元素，display 才会使用设置时的值

> 简单理解就是： position 绝对定位 > float > position 相对定位 > 默认

# visibility: collapse 的作用

1. 对于一般的元素，它的表现跟 visibility:hidden 一样，元素不可见，不可点击，但仍然占用页面空间
2. 但对于 table 相关的元素，例如 table行、tablegroup、table列、tablecolumngroup，他的表现和 display:none 一样，也就是它们占用的空间也会释放

## 1. 在不同浏览器的区别

1. chrome 里面，使用 collapse 和使用 hidden 没有区别
2. 在 FireFox、Opera 和 IE11 里，使用 collapse 的 table 的行会消息，它下面的一行会补充它的位置

# width:100% 和 width:auto 的区别

1. width:100% 会让元素的宽度等于包含容器的宽度，如果此时设置了 padding、border 或者 margin ，那么元素的整体宽度就会溢出了
2. width:auto 会使元素撑满包含窗口的宽度，margin、border、padding、content 会自动进行分配

# margin 重叠的问题

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

	2. ### 父元素与 第一/最后一 个子元素

    1. 父元素独立为一个新的 BFC
    2. 设置 boder-top/bottom 或者 padding-top/bottom 
    3. 使用 行内元素 作为父元素的第一个子元素

	3. 父元素与最后一个子元素

    	1. 设置父元素的 height、min-height 或者 max-height

	4. 空元素

    	1. 设置 border-top/bottom 或者 padding-top/bottom
    	2. 添加行内元素（空格无效）
    	3. 设置 height 或者 min-height



1. 

# 修改 chrome 自动填充的输入框的样式

```scss
input:-webkit-autofill{
	box-shadow:99999px solid #fff inset;
    // background-color 和 color 是无法修改的，只能使用其它方法替代
}
```

# font-soomthing

​	 仅 Mac OS 下有效，并且需要加前缀（ **-webkit-** ），非标准属性，没有用的必要

# font-style 的 italic 和 oblique 的区别

​	**italic** 会优先使用当前字体的斜体版本，**oblique** 是通过计算让文字倾斜。**litalic** 如果没有当前字体的斜体版本会解析为 **oblique**

# transition 和 animation 的区别

​	 可以皅 transition 当作 animation 的简化版，tansition 主要关注一个属性或者多个属性从一个值到别一个值的过渡，而 animation 关注的是一个元素的变化,   可以使用关键帧控制更小粒度的变化

# height:100% 为什么会失效

​	对于普通文档流中的元素, 如果包含块没有显式指定高度值, 它的 height 值始终为 auto, 也就是由它来撑起包含块的高度. 

# min-width/max-width 与 min-height/max-height 之间的覆盖规则

1. max-width/min-width 会覆盖 width，即使 width 是行内样式或者 !important
2. min-width 会覆盖 max-width，此规则发生在两者冲突时

# 常见隐藏元素的方式

1. display:none 渲染树不会包含该渲染对象和其下的子元素，在页面中不占据位置，不响应绑定的监听事件
2. visibility:hidden 元素不可视，在页面中仍然占据位置，但不响应绑定的监听事件
3. opacity:0 元素不可视，在页面中仍然占据位置，响应绑定的监听事件

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

#     