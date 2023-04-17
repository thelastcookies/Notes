# CSS 层叠样式表 Cascading Style Sheets

 ![css-logo.jpg](../static/images/css--logo.jpg)

## 什么是 CSS ？
CSS是用来为 HTML 文档添加样式的语言，描述了 HTML 元素应如何显示。

## 层叠次序
- 浏览器缺省设置
- 外部样式表
- 内部样式表（位于 <head标签内部）
- 内联样式（在 HTML 元素内部）


## CSS 缩写
详情请见《CSS缩写》

## CSS 语法

```css
selector {
    property1: value;
    property2: value;
    ...
    propertyN: value;
}
/* 实例：*/
h1 {
    color: red;
    font-size: 14px;
}
```

## CSS 选择器
### 选择器的分组
需要用逗号将所有需要分组的选择器分开
```css
h1, h2, h3, h4, h5, h6 {
    color: green;
}
```

### 派生选择器
通过依据元素在其位置的上下文关系来定义样式，你可以使标记更加简洁。
比如，要规定`<li>`元素里的<strong>，则
```css
li strong {
    ...
}
```
这样的话在`<li>`里面的所有`<strong>`被设定为上述的格式，而其他地方没有改动。
这样可以更少的定义`id`、`class`，使代码更加简洁。
派生选择器两个元素之间的层次间隔可以是无限的。

### ID选择器
ID 选择器可以为标有特定 ID 的 HTML 元素指定特定的样式。
ID 选择器以 "#" 来定义。

```css
#red {
    color: red;
}
```
```html
<p id="red">这个段落是红色。</p>
```

对于老版本要特别指明这个选组起所属的元素，如 `div#sidebar`

多结合派生选择器使用。比如在特定 ID 的 div 中为元素设置格式。
```css
#sidebar p {...}
```

### 类选择器
类选择器可以为所有标有该 `class` 的 HTML 元素指定样式。
类选择器以"."来定义。
```css
.center {
    text-align: center
}
```

```html
<h1 class="center">This heading will be center-aligned</h1>
```

也可结合派生选择器。
```css
.fancy td {...}
```

也可基于元素的类而被选择。
```css
td.fancy {...}
```

结合元素选择器
```css
p.important {...}
```

### 多类选择器
`.class1.class2`，选择所有在其class属性中同时设置了class1和class2的元素。
```html
<p class="important warning"></p>
```
可以用`.important.warning`来为此选择器设置样式


### 属性选择器
对带有指定属性的 HTML 元素设置样式。
也就是说属性选择器影响到的都是元素中规定了指定属性的元素。

属性选择器以`[...]`来定义。
```css
[title] {
    color: red;
}
```

可以根据多个属性进行选择：
```css
a[href][title] {...}
```
此选择器只会选择有href和title属性的链接

可以根据具体属性值选择：
```css
[title=Title] {...}
```
只会选择title=Title

属性与属性值必须完全匹配：

```html
<p class="important warning"></p>
```
```css
/* 可以匹配 */
p[class="important warning"]{...}
/* 不可匹配 */
p[class="important"]{...}
```

#### 根据部分属性值匹配 
```css
[title~=hello] {...}
```
```html
<h2 title="hello world">Hello world</h2>
```

#### 部分值属性选择器和类选择器的区别
可以有一个包含大量图像的文档，其中只有一部分是图片。
对此，可以使用一个基于 title 文档的部分属性选择器，只选择这些图片。
```css
img[title~="Figure"] {...}
```
```html
<img title="Figure 1" src="/i/figure-1.gif" />
<img title="Figure 2" src="/i/figure-2.gif" />
```

#### 子串匹配属性选择器
```css
[attribute^=value] {/*匹配属性值以指定值开头的每个元素*/}
[attribute$=value] {/*匹配属性值以指定值结尾的每个元素*/}
[attribute*=value] {/*匹配属性值中包含指定值的每个元素*/}
```

#### 特定属性选择器
`[attribute|=value]`，用于选取带有以指定值开头的属性值的元素，该值必须是整个单词
```css
[lang|=en] {...} 
```
```html
<p lang="en-us">Hi!</p>
```
这种结构是给lang属性中凡是有en开头的元素设定样式。适用于由"-"分隔的属性值。


### 子元素选择器
子元素选择器只能选择作为某元素子元素的元素.
以 `>` 来定义
```css
p strong {...}
```
可以结合其他选择器使用。


### 相邻兄弟选择器
可选择紧接在另一元素后的元素，且两个有相同的父元素，以"+"来定义。
```css
h1 + p {...}
```
定义了仅在`h1`元素后出现`p`元素的样式。
可以结合其他选择器使用。


### 选择器优先级
#### 特指度

特指度表示一个css选择器表达式的重要程度，可以通过一个公式来计算出一个数值，数越大，越重要。
这个计算叫做“I-C-E”计算公式：

- I——Id； ID选择器，比重为100
- C——Class；类选择器，比重为10
- E——Element；类型选择器，比重为 1

另外，!important优先级最高，高于上面一切。
在html元素中定义的style仅次于前者，* 选择器最低，低于一切。
还有，当前设置的样式，优先级高于从父辈继承来的样式


### 伪类
用于向某些选择器添加特殊的效果
```css
selector :pseudo-class {...}
```

- 锚伪类
  - `a:link`
  - `a:visited`
  - `a:hover`
  - `a:active`

- 子元素伪类
  - `:first-child` 选择其父元素的第一个子元素为该类型的所有元素。
  - `:last-child` 选择其父元素的最后一个子元素为该类型的所有元素。
  - `:nth-child(n)` 选择其父元素的第n个子元素为该类型的所有元素。
  - `:nth-last-child(n)` 选择其父元素的倒数n个子元素为该类型的所有元素。
  - `:first-of-type` 选择其父元素的第一个该类型元素为当前元素的所有元素。
  - `:last-of-type` 选择其父元素的最后一个该类型元素为当前元素的所有元素。
  - `:nth-of-type(n)` 选择其父元素的第n个该类型元素为当前元素的所有元素。
  - `:nth-last-of-type(n)` 选择其父元素的倒数n个该类型元素为当前元素的所有元素。
  - `:not(selector)` 选择所有不是该类元素的元素


- :lang伪类

  :lang 伪类使你有能力为不同的语言定义特殊的规则。

### 伪元素
用于向某些选择器设置特殊效果。
```css
selector:pseudo-element {...}
```

- `::first-line`
  用于向文本的首行设置特殊样式。
  
  只能用于块级元素。
  
  可设置的样式：`font color background word-spacing letter-spacing text-decoration vertical-align text-transform line-height clear`

- `::first-letter`
  用于向文本的首字母设置特殊样式

  只能用于块级元素

  可设置的样式：`font color background margin padding border text-decoration vertical-align (仅当 float 为 none 时) text-transform line-height float clear`

- `::before ::after`
  可以在元素的内容前后插入新内容。

## 样式引入
### 外部样式表
```html
<head>
    <link rel="stylesheet" type="text/css" href="style.css"/>
</head>
```

rel 属性用于指定当前文档与被链接文档的关系。

### 内部样式表
```html
<head>
    <style>
      
    </style>
</head>
```

### 内联样式
```html
<p style="color: sienna; margin-left: 20px">
This is a paragraph
</p>
```
### 多重样式
颜色属性将继承于外部样式表，而文字排列（text-alignment）和字体尺寸（font-size）会被内部样式表中的规则取代。


## 背景
### 背景色 
`background-color`

参数：合法的颜色值 默认值：transparent
实例：p {background-color: gray;}
不能继承 

### 背景图片
`background-image`

参数：url(...)
不能继承 

### 背景重复
`background-repeat`

参数：
- repeat-x 在水平方向上重复
- repeat-y 在垂直方向上重复
- no-repeat 不允许平铺

### 背景
`The background-origin`

指定背景图片的原点位置（背景定位区域）。

参数：
- padding-box 相对于内边距框定位，默认值
- border-box 相对于边框盒定位
- content-box 相对于内容框定位

### 背景定位
`background-position`

参数 
- 关键字：top bottom left right center 
- 长度值：px cm等 偏移值从左上角算起 先算水平 后算垂直
- 百分数值：前一个数值规定水平方位从左到右 后一个数值规定数值方位 从上到下
0% 0% 表示左上角 100% 100% 表示右下角

### 背景尺寸
`background-size`

参数：
- auto
- length 设置宽高
- percentage 设置宽高
- cover 将背景图片调整大小以覆盖整个容器，即使需要拉伸图像或在其中一个边缘略微裁剪。
- contain 调整背景图片大小以确保图像完全可见。

### 背景关联
`background-attachment`

参数：
- scroll 随文档滚动，默认值
- fixed 对可视区固定 

### 背景绘制区域
`background-clip`

参数：

- border-box: 背景被裁剪到边框盒
- padding-box: 背景被裁剪到内边距框
- content-box: 背景被裁剪到内容框
  相当于指定从哪里开始可以显示背景色和背景图片

### 多重背景
```css
background-image: url(bg_flower.gif), url(bg_flower_2.gif);
```

### 渐变
#### `linear-gradient` 线性渐变

- `<side-or-corner>`
描述渐变线的起始点位置。它包含 to 和两个关键词：第一个指出水平位置 left or right，第二个指出垂直位置 top or bottom。关键词的先后顺序无影响，且都是可选的。 to top, to bottom, to left 和 to right 这些值会被转换成角度 0 度、180 度、270 度和 90 度。其余值会被转换为一个以向顶部中央方向为起点顺时针旋转的角度。渐变线的结束点与其起点中心对称。

- `<angle>`
用角度值指定渐变的方向（或角度）。角度顺时针增加。

- `<linear-color-stop>`
由一个<color>值组成，并且跟随着一个可选的终点位置（可以是一个百分比值或者是沿着渐变轴的<length>）。CSS 渐变的颜色渲染采取了与 SVG 相同的规则。

- `<color-hint>`
颜色中转点是一个插值提示，它定义了在相邻颜色之间渐变如何进行。长度定义了在两种颜色之间的哪个点停止渐变颜色应该达到颜色过渡的中点。如果省略，颜色转换的中点是两个颜色停止之间的中点。

#### `radial-gradient` 径向渐变

- `<position>` 与 background-position 或者 transform-origin 类似。如果没有指定，默认为中心点。

- `<ending-shape>` 渐变结束时的形状。圆形（渐变的形状是一个半径不变的正圆）或椭圆形（轴对称椭圆）。默认值为椭圆。

- `<size>` 确定渐变结束形状的大小。如果省略，则默认为最远角。它可以显式给出，也可以通过关键字给出。出于关键字定义的目的，将梯度框边缘视为在两个方向上无限延伸，而不是有限线段。

- `<linear-color-stop>` 色值结束点（color stop）的 <color> 值，后跟一个或两个可选的停止位置（沿渐变轴的 <percentage> 或 <length>）。0% 的百分比，或者 0 的长度，代表渐变的中心；值 100% 表示结束形状与虚拟渐变射线的交点。两者之间的百分比值线性定位在梯度射线上。包括两个停止位置相当于在两个位置声明了两个颜色相同的色值结束点。

- `<color-hint>` color-hint 是一个插值提示，定义了相邻色标之间的渐变如何进行。长度定义了两种颜色之间的哪个点渐变颜色应该到达颜色过渡的中点。如果省略，颜色过渡的中点是两个色值结束点之间的中点。

## 文本
### 缩进文本
`text-indent`

参数：
- 长度值，可以为负值
- 使用负值，可以“悬挂缩进”，但是要设置内边距或外编剧
- 使用百分数值：缩进父元素的百分值

### 水平对齐：
`text-align`

参数：
- left
- right
- center
- justify

### 字间隔
`word-spacing`

参数：长度值 可为负值

### 字母间隔
`letter-spacing`

参数：长度值 可为负值

letter-spacing 与 word-spacing的区别
前者是修改每一个字母之间的间隔，后者是修改每一个单词之间的间隔

### 字符装换
`text-transform`

参数值：
- none 不做改动
- uppercase 全部大写
- lowercase 全部小写
- capitalize 首字母大写

### 文本装饰
`text-decoration`

可以在一个规则中结合多种样式。

参数值：
- none 关闭所有装饰
- underline 增加下划线
- overline 增加上划线
- line-through 增加贯穿线
- blink 增加文本闪烁

### 处理空白符
`white-space`

影响对空格，换行，tab字符的处理。

参数：
- 值空白符换行符自动换行
- pre-line合并保留允许
- normal合并忽略允许
- nowrap合并忽略不允许
- pre 保留保留不允许
- pre-wrap保留保留允许

### 文字方向
`direction`

参数：
- ltr 默认 从左到右
- rtl 从右到左
- inherit 从父元素继承

### 文本颜色
`color`

参数：所有合法的颜色值

### 行间距
`line-height`

参数：长度值 百分数值

## CSS3 文本效果
`text-shadow` 文字阴影

参数：
h-shadow 必需。水平阴影的位置。允许负值。
v-shadow必需。垂直阴影的位置。允许负值。
blur可选。模糊的距离。
color 可选。阴影的颜色。参阅 CSS 颜色值。

`word-wrap` 截断长单词

参数：
normal: 默认
break-word: 截断

`word-break`

参数：
normal: 使用浏览器默认的换行规则。
break-all: 允许在单词内换行。
keep-all: 只能在半角空格或连字符处换行。

`text-overflow`

参数：
clip: 修剪文本
ellipsis: 用省略号代替被修建文本
string: 用指定的该字符串代替被修剪文本

## 字体

### 指定字体
`font-family`

参数：
- 通用字体：Serif Sans-serif Monospace Cursive Fantasy
- 特定字体

建议在font-family中提供一个通用字体系列，作为后路
字体名称包含空格，#，$时要用引号

### 字体风格
`font-style`

参数：
- normal 文本正常显示
- italic 文本斜体显示 改变字母结构
- oblique 文本倾斜显示 只是对字幕进行倾斜

### 字体加粗
`font-weight`

参数：100~900 normal(400) bold(700)

### 字体大小
`font-size`

参数：像素 em(推荐) 百分数值


## CSS3 字体
`@font-face`

语法：
```css
@font-face {
    font-family: . . .;
    src: url('...'),
    url('...');
}
```

参数：
- font-family 必需。规定字体的名称。
- src 必需。定义字体文件的 URL。
- font-stretch 拉伸字体
- font-style 字体样式
- font-weight 字体粗细


## CSS 列表
略


## CSS 表格
略

## CSS 轮廓

### 轮廓颜色

`outline-color` 

### 轮廓样式

`outline-style`

参数：
- dotted
- dashed 
- solid 
- double 

### 轮廓宽度
`outline-width` 

参数：
- thin 细轮廓 
- medium 默认 
- thick 粗轮廓 
- length 可规定轮廓粗细值


## CSS 盒模型

### 由外到内 `margin` `border` `padding` `element`。

### padding

内边距，是盒模型中border与element中的部分。

参数：长度值 百分数值 不接受负值 可按照上右下左的顺序规定

### border

边框。

`boder-style` 边框样式

参数：`none hidden dotted dashed solid double groove ridge inset outset`

如果没有设置边框样式，边框就不会出现，相应的，宽度，颜色等都无效。

`border-color` 边框颜色

边框颜色可以设置为transparent，可创建有宽度的不可见边框。

`border-width` 边框宽度

参数：thin medium thick length

### margin

外边距，是盒模型中border之外的部分。

参数：长度值 百分数值

### CSS 外边距合并

当两个垂直外边距相遇时，它们将形成一个外边距。

合并后的外边距的高度等于两个发生合并的外边距的高度中的较大者。

当一个元素出现在另一个元素上面时，第一个元素的下外边距与第二个元素的上外边距会发生合并。

当一个元素包含在另一个元素中时（假设没有内边距或边框把外边距分隔开），它们的上和/或下外边距也会发生合并。

当有一个空元素，它有外边距，但是没有边框或填充。在这种情况下，上外边距与下外边距就碰到了一起，它们会发生合并。

## CSS3 边框
### `border-radius` 圆角

参数：
指定圆角半径 接受 像素值 百分数值

### `border-image` 边框图片
`border-image`是一个简写属性，按照以下的顺序指定属性。
- `border-image-source` 路径
- `border-image-slice` 边框向内偏移
- `border-image-width` 边框宽度
- `border-image-outset` 边框图像区域超出边框的量。
- `border-image-repeat` 平铺(repeat) 铺满(round) 拉伸(stretch)



## CSS overflow
当元素框内内容溢出时发生的事情。
- visible 默认值。内容不会被修剪，会呈现在元素框之外。
- hidden 内容会被修剪，并且其余内容是不可见的。
- scroll 内容会被修剪，但是浏览器会显示滚动条以便查看其余的内容。
- auto 如果内容被修剪，则浏览器会显示滚动条以便查看其余的内容。
- inherit 规定应该从父元素继承 overflow 属性的值。

## CSS 定位
定位允许你定义元素框相对于其正常位置应该出现的位置，或者相对于父元素、另一个元素甚至浏览器窗口本身的位置。

### position

参数：

`static`

该关键字指定元素使用正常的布局行为，即元素在文档常规流中当前的布局位置。此时 top, right, bottom, left 和 z-index 属性无效。

`relative`

该关键字下，元素先放置在未添加定位时的位置，再在不改变页面布局的前提下调整元素位置（因此会在此元素未添加定位时所在位置留下空白）。position:relative 对 table-*-group, table-row, table-column, table-cell, table-caption 元素无效。

`absolute`

元素会被移出正常文档流，并不为元素预留空间，通过指定元素相对于最近的非 static 定位祖先元素的偏移，来确定元素位置。绝对定位的元素可以设置外边距（margins），且不会与其他边距合并。

`fixed`

元素会被移出正常文档流，并不为元素预留空间，而是通过指定元素相对于屏幕视口（viewport）的位置来指定元素位置。元素的位置在屏幕滚动时不会改变。打印时，元素会出现在的每页的固定位置。fixed 属性会创建新的层叠上下文。当元素祖先的 transform、perspective、filter 或 backdrop-filter 属性非 none 时，容器由视口改为该祖先。

`sticky`

元素根据正常文档流进行定位，然后相对它的最近滚动祖先（nearest scrolling ancestor）和 containing block（最近块级祖先 nearest block-level ancestor），包括 table-related 元素，基于 top、right、bottom 和 left 的值进行偏移。偏移值不会影响任何其他元素的位置。 该值总是创建一个新的层叠上下文（stacking context）。注意，一个 sticky 元素会“固定”在离它最近的一个拥有“滚动机制”的祖先上（当该祖先的 overflow 是 hidden、scroll、auto 或 overlay 时），即便这个祖先不是最近的真实可滚动祖先。这有效地抑制了任何“sticky”行为。


### CSS 浮动
略


## CSS 媒体查询
媒体查询（Media queries）非常实用，尤其是当你想要根据设备的大致类型（如打印设备与带屏幕的设备）或者特定的特征和设备参数（例如屏幕分辨率和浏览器视窗宽度）来修改网站或应用程序时。

媒体查询常被用于以下目的：

有条件的通过 @media 和 @import at-rules 用CSS 装饰样式。
用 media= 属性为`<style>`, `<link>`, `<source>`和其他HTML元素指定特定的媒体类型。如：
```html
<link rel="stylesheet" src="styles.css" media="screen" />
<link rel="stylesheet" src="styles.css" media="print" />
```

### 定位媒体类型
媒体类型描述了给定设备的一般类别。
```css
@media screen, print {/* ... */}
```
### 定位媒体特性
媒体功能描述了给定的user agent的输出设备或环境的特定特征。例如，您可以将特定样式应用于特定宽度的显示器。
```css
@media (max-width: 1920px) {/* ... */}
```
### 创建复杂查询
使用 not，and，和 only 等来结合多种类型和特性。
### 版本 4 中的语法改进
媒体查询 4 级规范对语法进行了一些改进，以使用具有“范围”类型（例如宽度或高度，减少冗余）的功能进行媒体查询。级别 4 添加了用于编写此类的查询范围上下文。例如，使用最大宽度max- 功能，我们可以编写以下代码：
```css
@media (max-width: 30em) {/* ... */}
```
在媒体查询 4 级规范可以这样写：
```css
@media (width <= 30em) {/* ... */}
```
使用min-和max-可以测试一个在两个值之间的宽度。
```css
@media (min-width: 30em) and (max-width: 50em) {/* ... */}
```
用 4 级语法书写如下
```css
@media (30em <= width <= 50em ) {/* ... */}
```

## CSS3 Box
### `box-shadow` 阴影
语法：box-shadow: h-shadow v-shadow blur spread color insert;

参数：
- h-shadow 必需，水平阴影的位置
- v-shadow 必需，垂直阴影的位置
- blur 阴影模糊半径
- spread 阴影扩散半径
- color 阴影颜色
- insert 外部阴影改为内部阴影

### `box-sizing` 宽高计算方式
CSS 中的 box-sizing 属性定义了 user agent 应该如何计算一个元素的总宽度和总高度。
- `content-box`
  默认值，标准盒子模型。width 与 height 只包括内容的宽和高，不包括边框（border），内边距（padding），外边距（margin）。
- `border-box`
  width 和 height 属性包括内容，内边距和边框，但不包括外边距。


## CSS3 2DTransform
### `translate` 位移 

`translateX(n) translateY(n)`

参数：像素值 
x,y 的位置参数
```css
transform: translate(50px, 100px);
```

### `rotate`旋转
参数：元素顺时针转动角度，允许负值，n deg。
```css
transform: rotate(30deg);
```

### `scale`缩放

`scaleX(n) scaleY(n)`

参数：宽高倍数
```css
transform: scale(2,4);
```

### `skew`翻转
`skewX(angle) skewY(angle)`

参数：沿 x,y 轴翻转的角度
```css
transform: skew(30deg, 20deg);
```

### `matrix`用6个参数对元素进行操作
`matrix( scaleX(), skewY(), skewX(), scaleY(), translateX(), translateY() )`

```css
transform: matrix(0.866, 0.5, -0.5, 0.866, 0, 0);
```

### `transform-origin`
设置元素变形的原点。

参数：
left center right 长度值 百分数值


## CSS3 过渡
### `transition`
该属性是以下属性的简写形式。

属性：
- `transition-property` 规定应用过渡效果的 CSS 属性的名称。
- `transition-duration` 属性以秒或毫秒为单位指定过渡动画所需的时间。默认值为 0s，表示不出现过渡动画。
- `transition-timing-function` 速度曲线。参数：linear ease ease-in ease-out...
- `transition-delay` 过渡之前的等待时间


## CSS3 动画
### `@keyframes`
创建一个带名称的 `@keyframes` 规则，以便后续使用 `animation-name` 属性将动画同其关键帧声明匹配。

关键帧 `@keyframes` 通过在动画序列中定义关键帧（或 waypoints）的样式来控制 CSS 动画序列中的中间步骤。

参数：
- percentage
动画序列中触发关键帧的时间点，使用百分值来表示

- from
等价于 0%

- to
等价于 100%


### `animation` 属性 

该属性是以下属性的简写形式。

- `animation-name` 规定动画的名称，对应`@keyframes`提供的名称
- `animation-duration` 规定一个周期所花时间
- `animation-timing-function` 速度曲线
- `animation-delay` 过渡之前的等待时间
- `animation-iteration-count` 动画的播放次数。参数：
  - infinite 无限循环播放动画
  - number 指定播放次数
- `animation-direction` 规定动画播放形式。参数：
  - normal 正常播放
  - reverse 反向播放
  - alternate 反复播放
  - alternate-reverse 反向反复播放
  
- `animation-fill-mode` 设置 CSS 动画在执行之前和之后如何将样式应用于其目标。参数：
  - none 当动画未执行时，动画将不会将任何样式应用于目标，而是已经赋予给该元素的 CSS 规则来显示该元素。这是默认值。
  - forwards 目标将保留由执行期间遇到的最后一个关键帧计算值。
  - backwards 动画将在应用于目标时立即应用第一个关键帧中定义的值，并在animation-delay期间保留此值。
  - both 动画将遵循forwards和backwards的规则，从而在两个方向上扩展动画属性。
- `animation-play-state` 设置播放和暂停状态。参数：
  - running 当前动画正在运行。
  - paused 当前动画已被停止。


## Flex
参考链接：
[阮一峰的网络日志：《Flex 布局教程：语法篇》](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)

## Grid
参考链接：
[阮一峰的网络日志：《CSS Grid 网格布局教程》](https://www.ruanyifeng.com/blog/2019/03/grid-layout-tutorial.html)


