



# CSS 选择器

## 1. 优先级

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

## 2. 分类

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

# 伪类和伪元素

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

## 1. CSS3 新增伪类

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

> `first-child` 是 css2 就有的

# 可继承的属性

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