# CSS 层叠样式表 Cascading Style Sheets

 ![css-logo.jpg](../static/images/css--logo.jpg)

## 什么是CSS
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


## CSS 属性选择器
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


## CSS 子元素选择器
子元素选择器只能选择作为某元素子元素的元素.
以 `>` 来定义
```css
p strong {...}
```
可以结合其他选择器使用。


## CSS 相邻兄弟选择器
可选择紧接在另一元素后的元素，且两个有相同的父元素，以"+"来定义。
```css
h1 + p {...}
```
定义了仅在`h1`元素后出现`p`元素的样式。
可以结合其他选择器使用。


## CSS 选择器优先级
#### 特指度

特指度表示一个css选择器表达式的重要程度，可以通过一个公式来计算出一个数值，数越大，越重要。
这个计算叫做“I-C-E”计算公式：

- I——Id； ID选择器，比重为100
- C——Class；类选择器，比重为10
- E——Element；类型选择器，比重为 1

另外，!important优先级最高，高于上面一切。
在html元素中定义的style仅次于前者，* 选择器最低，低于一切。
还有，当前设置的样式，优先级高于从父辈继承来的样式


## CSS 伪类
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

## CSS 伪元素
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

## CSS 插入样式
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


## CSS 背景
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
- padding-box 背景设定从 padding-box 开始，默认值
- border-box 背景设定从 border-box 开始
- content-box 背景设定从 content-box 开始

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
- length	
- percentage
- cover 将背景图片调整大小以覆盖整个容器，即使需要拉伸图像或在其中一个边缘略微裁剪。
- contain 调整背景图片大小以确保图像完全可见。

### 背景关联
`background-attachment`

参数：
- scroll 随文档滚动，默认值
- fixed 对可视区固定 


## CSS 文本
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


## CSS 字体
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

## CSS 列表
### 列表样式 (列表前的点)

`list-style-type`

参数：none disc circle square decimal ......

### 列表项图像

`list-style-image`

参数：url(...)

### 列表标志位置

`list-style-position`

参数：inside outside


## CSS 表格
表格边框
border

折叠边框
border-collapse
参数：separate collapse

表格的宽度和高度
width height

表格文本对齐
text-align vertical-align

表格内边距
padding

表格颜色
color
background-color

相邻单元格边框间距
border-spacing

表格标题放置位置
caption-side
参数：top bottom

空单元格的边框
empty-cells
参数：automatic 由单元格内容设定 fixed由表格宽度和列宽度设定


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


## CSS 框模型

由外到内 margin border padding element(width height)

padding
参数：长度值 百分数值 不接受负值 可按照上右下左的顺序规定
padding-top
padding-right
padding-bottom
padding-left

border
元素的背景是内容、内边距和边框区的背景

边框样式
boder-style
参数：none hidden dotted dashed solid double groove ridge inset outset
border-top-style
border-right-style
border-bottom-style
border-left-style
如果没有设置边框样式，边框就不会出现，相应的，宽度，颜色等都无效

边框颜色
border-color 
border-top-color
border-right-color
border-bottom-color
border-left-color
边框颜色可以设置为transparent，可创建有宽度的不可见边框

边框宽度
border-width
参数：thin medium thick length
border-top-width
border-right-width
border-bottom-width
border-left-width


## CSS 外边距
margin
参数：长度值 百分数值
margin-top
margin-right
margin-bottom
margin-left


## CSS 外边距合并
当两个垂直外边距相遇时，它们将形成一个外边距。
合并后的外边距的高度等于两个发生合并的外边距的高度中的较大者。
当一个元素出现在另一个元素上面时，第一个元素的下外边距与第二个元素的上外边距会发生合并。
当一个元素包含在另一个元素中时（假设没有内边距或边框把外边距分隔开），它们的上和/或下外边距也会发生合并。
当有一个空元素，它有外边距，但是没有边框或填充。在这种情况下，上外边距与下外边距就碰到了一起，它们会发生合并。


## CSS overflow
当元素框内内容溢出时发生的事情
visible 默认值。内容不会被修剪，会呈现在元素框之外。
hidden内容会被修剪，并且其余内容是不可见的。
scroll内容会被修剪，但是浏览器会显示滚动条以便查看其余的内容。
auto如果内容被修剪，则浏览器会显示滚动条以便查看其余的内容。
inherit 规定应该从父元素继承 overflow 属性的值。

## CSS 定位
定位允许你定义元素框相对于其正常位置应该出现的位置，或者相对于父元素、另一个元素甚至浏览器窗口本身的位置

块级元素
div，p，h1等元素被称为块级元素(block)，而span，strong被称为内联元素(inline)

display
修改生成框的类型

CSS定位机制
普通流，浮动，绝对定位
行内框在一行中水平布置。可以使用水平内边距、边框和外边距调整它们的间距。
但是，垂直内边距、边框和外边距不影响行内框的高度。
由一行形成的水平框称为行框（Line Box），行框的高度总是足以容纳它包含的所有行内框。
不过，设置行高可以增加这个框的高度。

position
参数：top right bottom left
static 元素框正常生成。块级元素生成一个矩形框，作为文档流的一部分，行内元素则会创建一个或多个行框，置于其父元素中。
relative 
absolute 
fixed 元素框的表现类似于将 position 设置为 absolute，不过其包含块是视窗本身。


## CSS 相对定位
relative
元素框偏移某个距离。元素仍保持其未定位前的形状，它原本所占的空间仍保留。
在使用相对定位时，无论是否进行移动，元素仍然占据原来的空间。
因此，移动元素会导致它覆盖其它框。

## CSS 绝对定位
absolute
设置为绝对定位的元素框从文档流完全删除，并相对于其包含块定位，
包含块可能是文档中的另一个元素或者是初始包含块。
元素原先在正常文档流中所占的空间会关闭，就好像该元素原来不存在一样。元素定位后生成一个块级框，而不论原来它在正常流中生成何种类型的框。
绝对定位的元素的位置相对于最近的已定位祖先元素，如果元素没有已定位的祖先元素，那么它的位置相对于最初的包含块。
*根据用户代理的不同，最初的包含块可能是画布或 HTML 元素。
*可以用z-index来设置堆叠顺序

## CSS clip
clip 属性剪裁绝对定位元素。
参数：
shape 设置元素的形状。唯一合法的形状值是：rect (top, right, bottom, left)
auto默认值。不应用任何剪裁。
inherit 规定应该从父元素继承 clip 属性的值。


## CSS 浮动
float
参数：left right none 
clear该属性规定元素的哪一侧不允许其他浮动元素。
参数：left right both none
*谁动谁设定


## CSS 水平对齐
使用margin属性
margin:auto;
*宽度不能为100%

使用position属性左右对齐
position:absolute;
使用position在IE浏览器中必须声明<!DOCTYPE>属性

使用float属性左右对齐
float:left;
float:right;


## CSS 尺寸
height
width
line-height
max-height
min-height
max-width
min-width


## CSS 分类
clear: none left right none inherit
cursor: default auto url（使用指定url） pointer crosshair ......
display: inline none block inli-block ......
float: none left right inherit
position: static absolute relative fixed inherit
visibility: visible hidden collapse inherit


## CSS 媒介类型
媒介类型(Media Types)
允许你定义以何种媒介来提交文档。
文档可以被显示在显示器、纸媒介或者听觉浏览器等等。

参数：
all aural braille embossed handheld print projection screen tty tv

实例：
<style>
@media screen
{
p.test {font-family:verdana,sans-serif; font-size:14px}
}
@media print
{
p.test {font-family:times,serif; font-size:10px}
}
@media screen,print
{
p.test {font-weight:bold}
}
</style>


## CSS 兼容
opacity
/* for IE */
filter:alpha(opacity=60);
/* CSS3 standard */
opacity:0.6;


## CSS3 边框
border-radius方框圆角
参数：指定圆角半径 接受 像素值 百分数值 
兼容：
-moz-border-radius:... /* Old Firefox */

box-shadow方框阴影
语法：box-shadow: h-shadow v-shadow blur spread color insert;
参数：
h-shadow 必需，水平阴影的位置
v-shadow 必需，垂直阴影的位置
blur 模糊距离
spread 阴影尺寸
color 阴影颜色
insert 外部阴影改为内部阴影
兼容：
-moz-box-shadow:... /* Old Firefox */

border-image边框图片
border-image是一个简写属性
border-image-source路径
border-image-slice边框向内偏移
border-image-width边框宽度
border-image-outset边框图像区域超出边框的量。
border-image-repeat平铺(repeat) 铺满(round) 拉伸(stretch)

兼容：
-moz-border-image:... /* Old Firefox */
-webkit-border-image:... /* Safari 5 */
-o-border-image:... /* Opera */

浏览器支持：
Internet Explorer 9+ 支持 border-radius 和 box-shadow 属性。
Firefox、Chrome 以及 Safari 支持所有新的边框属性。
对于 border-image，Safari 5 以及更老的版本需要前缀 -webkit-。
Opera 支持 border-radius 和 box-shadow 属性，但是对于 border-image 需要前缀 -o-。


## CSS3 背景
background-size背景图片尺寸
语法：
background-size: length|percentage|cover|contain;
length: 设置宽高
percentage: 设置宽高
cover: 把背景图像扩展至足够大，以使背景图像完全覆盖背景区域。
 背景图像的某些部分也许无法显示在背景定位区域中。
contain:把图像图像扩展至最大尺寸，以使其宽度和高度完全适应内容区域。
兼容：
-moz-background-size:.../*Old Firefox*/

background-origin背景图片定位区域
参数：
padding-box: 相对于内边距框定位
border-box: 相对于边框盒定位
content-box: 相对于内容框定位
相当于指定background-position开始的参照点是在哪里

background-clip背景绘制区域
参数：
border-box: 背景被裁剪到边框盒
padding-box: 背景被裁剪到内边距框
content-box: 背景被裁剪到内容框
相当于指定从哪里开始可以显示背景色和背景图片

多重背景
background-image:url(bg_flower.gif),url(bg_flower_2.gif);


## CSS3 文本效果
text-shadow文字阴影
参数：
h-shadow 必需。水平阴影的位置。允许负值。
v-shadow必需。垂直阴影的位置。允许负值。
blur可选。模糊的距离。 
color 可选。阴影的颜色。参阅 CSS 颜色值。

word-wrap截断长单词
参数：
normal: 默认
break-word: 截断

word-break
参数：
normal: 使用浏览器默认的换行规则。
break-all: 允许在单词内换行。
keep-all: 只能在半角空格或连字符处换行。

text-overflow
参数：
clip: 修剪文本
ellipsis: 用省略号代替被修建文本
string: 用指定的该字符串代替被修剪文本


## CSS3 字体
@font-face
语法：
@font-face
{
font-family: ...;
src: url('...'),
 url('...');
}
参数：
font-family: 必需。规定字体的名称。
src: 必需。定义字体文件的 URL。
font-stretch: 拉伸字体
font-style:字体样式
font-weight:字体粗细

兼容性：
Firefox、Chrome、Safari 以及 Opera 支持 .ttf (True Type Fonts) 和 .otf (OpenType Fonts) 类型的字体。
Internet Explorer 9+ 支持新的 @font-face 规则，但是仅支持 .eot 类型的字体 (Embedded OpenType)。


## CSS3 2DTransform
translate()位移translateX(n)translateY(n)
参数：像素值 x,y的位置参数
兼容：
transform: translate(50px,100px);
-ms-transform: translate(50px,100px); /* IE 9 */
-webkit-transform: translate(50px,100px); /* Safari and Chrome */
-o-transform: translate(50px,100px);/* Opera */
-moz-transform: translate(50px,100px);/* Firefox */

rotate()旋转
参数：元素顺时针转动角度，允许负值，n deg。
兼容：
transform: rotate(30deg);
-ms-transform: rotate(30deg); /* IE 9 */
-webkit-transform: rotate(30deg); /* Safari and Chrome */
-o-transform: rotate(30deg);/* Opera */
-moz-transform: rotate(30deg);/* Firefox */

scale()缩放scaleX(n)scaleY(n)
参数：宽高倍数
兼容：
transform: scale(2,4);
-ms-transform: scale(2,4);/* IE 9 */
-webkit-transform: scale(2,4);/* Safari 和 Chrome */
-o-transform: scale(2,4); /* Opera */
-moz-transform: scale(2,4); /* Firefox */

skew()翻转skewX(angle)skewY(angle)
参数：沿x,y轴翻转的角度
兼容：
transform: skew(30deg,20deg);
-ms-transform: skew(30deg,20deg); /* IE 9 */
-webkit-transform: skew(30deg,20deg); /* Safari and Chrome */
-o-transform: skew(30deg,20deg);/* Opera */
-moz-transform: skew(30deg,20deg);/* Firefox */

matrix()用6个参数对元素进行操作
兼容：
transform:matrix(0.866,0.5,-0.5,0.866,0,0);
-ms-transform:matrix(0.866,0.5,-0.5,0.866,0,0); /* IE 9 */
-moz-transform:matrix(0.866,0.5,-0.5,0.866,0,0);/* Firefox */
-webkit-transform:matrix(0.866,0.5,-0.5,0.866,0,0); /* Safari and Chrome */
-o-transform:matrix(0.866,0.5,-0.5,0.866,0,0);/* Opera */

transform-origin
设置旋转元素的基点位置
参数：
left center right 长度值 百分数值

兼容：
Internet Explorer 10、Firefox、Opera 支持 transform-origin 属性。
Internet Explorer 9 支持替代的 -ms-transform-origin 属性（仅适用于 2D 转换）。
Safari 和 Chrome 支持替代的 -webkit-transform-origin 属性（3D 和 2D 转换）。
Opera 只支持 2D 转换。


## CSS3 3D Transform
rotateX() rotateY()
参数：绝对角度
兼容：
transform: rotateX(120deg);
-webkit-transform: rotateX(120deg); /* Safari 和 Chrome */
-moz-transform: rotateX(120deg);/* Firefox */


## CSS3 过渡
transition: property duration timing-function delay;
*transition 写在要被过渡的盒子里
属性：
property 规定应用过渡效果的 CSS 属性的名称
duration 过渡持续时间 参数：time
timing-function 过渡开始结束时的过渡效果 参数：linear ease ease-in ease-out...
delay 过渡之前的等待时间
兼容：
Internet Explorer 10、Firefox、Chrome 以及 Opera 支持 transition 属性。
Internet Explorer 9 以及更早的版本，不支持 transition 属性。
transition: ...;
-moz-transition: ...;/* Firefox 4 */
-webkit-transition: ...; /* Safari 和 Chrome */
-o-transition: ...;/* Opera */


## CSS3 动画
@keyframes
先用@keyframes制作动画，再在某个选择器中设定animation和时间
@keyframes 中可以用from，to也可用百分数值规定

animation属性
name: 规定动画的名称
duration: 规定一个周期所花时间 0
timing-function: 速度曲线 ease
delay: 延迟 0
iteration-count:动画被播放次数 1
direction: 规定动画在下一周期是否逆向播放 normal alternate
play-state: 规定动画是否正在运行或暂停 running 结合js使用
fill-mode: 规定对象动画时间之外的状态

兼容：
@keyframes myfirst
@-moz-keyframes myfirst /* Firefox */
@-webkit-keyframes myfirst /* Safari 和 Chrome */ 
@-o-keyframes myfirst /* Opera */


## CSS3 多列
创建多个列来对文本进行布局
column-count: 规定元素被分隔的列数
column-gap: 规定列之间的间隔
column-rule: 规定列之间的宽度，样式和颜色规则
属性：
color: 颜色值
style: none hidden dotted dashed solid
width: medium thin thick 长度值
column-width: 列的宽度 auto 长度值
column-span: 1 all 跨列
column: width和count的简写属性
兼容：
Internet Explorer 10 和 Opera 支持多列属性。
Firefox 需要前缀 -moz-。
Chrome 和 Safari 需要前缀 -webkit-。
Internet Explorer 9 以及更早的版本不支持多列属性。


## CSS3 用户界面
resize: 规定是否可由用户调节元素尺寸
参数：none both horizontal vertical
*若想此属性生效，则需要设置overflow属性，参数：auto, hidden ,scroll
box-sizing: content-box（default），border-box，padding-box。

outline-offset: 对轮廓进行偏移，并在边框边缘进行绘制参数：长度值
兼容：
Firefox、Chrome 以及 Safari 支持 resize 属性。
Internet Explorer、Chrome、Safari 以及 Opera 支持 box-sizing 属性。Firefox 需要前缀 -moz-。
所有主流浏览器都支持 outline-offset 属性，除了 Internet Explorer。

## CSS 按钮 （选择器选到button）
按钮颜色background-color
按钮大小font-size
远郊按钮border-radius
按钮边框颜色border-color
鼠标悬停:hover
按钮阴影详见border-shadow
按钮宽度 width
按钮动画

## CSS3 nth-child() 选择器
匹配属于其父元素的第 N 个子元素，不论元素的类型。
even表示单数，odd表示复数
<style>
p:nth-child()
{
 CSSCode
}
<div>
<p></p>
<p></p>
<p></p>
</div>


## CSS 颜色名
Coral
CornflowerBlue
DarkViolet
DeepSkyBlue
FloralWhite
HotPink
LightGreen
Lime
Navy
OrangeRed
Plum
Silver
SlateGray
Snow
Tomato
SteelBlue
