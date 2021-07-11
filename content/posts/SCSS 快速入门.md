---
title: "scss快速入门"
date: 2019-08-18T19:02:47+08:00
draft: false
tags: ["前端"]
---

# **变量**
scss使用 **$** 符号来标识变量，比如 **$highlight-color** 和 **$sidebar-width**。
>变量不区分减号和下划线<br/>例如变量home-name与home_name等价 是同一个变量
### <font color="blue">变量声明</font>
任何可以用作css属性值的赋值都可以用作scss的变量值，甚至是以空格分割的多个属性值，或以逗号分割的多个属性值。
```scss
$highlight-color: #F90;
$basic-border: 1px solid black;
$plain-font: "Myriad Pro","Helvetica Neue","Liberation Sans";
```
变量可以在css规则块定义之外存在。当变量定义在css规则块内，那么该变量只能在此规则块内使用。如果它们出现在任何形式的{...}块中（如@media或者@font-face块），情况也是如此：
```scss
$nav-color: #F90;
.nav {
  $width: 100px;
  width: $width;
  color: $nav-color;
}
```
>编译后
```css
.nav {
  width: 100px;
  color: #F90;
}
```
### <font color="blue">变量的使用</font>
直接上例子
```scss
$highlight-color: #F90;
$highlight-border: 1px solid $highlight-color;
.selected {
  border: $highlight-border;
}
```
>编译后
```css
.selected {
  border: 1px solid #F90;
}
```
# **CSS嵌套**
### <font color="blue">简单嵌套</font>
css预处理都会有的功能 不累述
```scss
#content {
  article {
    h1 { color: #333 }
    p { margin-bottom: 1.4em }
  }
  aside { background-color: #EEE }
}
```
>编译后
```css
#content article h1 { color: #333 }
#content article p { margin-bottom: 1.4em }
#content aside { background-color: #EEE }
```
### <font color="blue">父选择器的标识符 **&** </font>
话不多说 上例子
```scss
article a {
  color: blue;
  &:hover { color: red }
}
```
>编译后
```css
article a { color: blue }
article a:hover { color: red }
```
### <font color="blue">群组选择器的嵌套</font>
```scss
.container {
  h1, h2, h3 {margin-bottom: .8em}
}
```
>编译后
```css
.container h1, .container h2, .container h3 { margin-bottom: .8em }
```

```scss
nav, aside {
  a {color: blue}
}
```
>编译后
```css
nav a, aside a {color: blue}
```

### <font color="blue">子组合选择器和同层组合选择器 **> + ~** </font>
上边这三个组合选择器必须和其他选择器配合使用，以指定浏览器仅选择某种特定上下文中的元素。
```scss
article section { margin: 5px }
article > section { border: 1px solid #ccc }
```
你可以用子组合选择器>选择一个元素的直接子元素。上例中，第一个选择器会选择article下的所有命中section选择器的元素。第二个选择器只会选择article下紧跟着的子元素中命中section选择器的元素。

在下例中，你可以用同层相邻组合选择器+选择header元素后紧跟的p元素：
```scss
header + p { font-size: 1.1em }
```
你也可以用同层全体组合选择器~，选择所有跟在article后的同层article元素，不管它们之间隔了多少其他元素：
```scss
article ~ article { border-top: 1px dashed #ccc }
```
可以把它们放在外层选择器后边，或里层选择器前边：
```scss
article {
  ~ article { border-top: 1px dashed #ccc }
  > section { background: #eee }
  dl > {
    dt { color: #333 }
    dd { color: #555 }
  }
  nav + & { margin-top: 0 }
}
```
>编译后
```css
article ~ article { border-top: 1px dashed #ccc }
article > footer { background: #eee }
article dl > dt { color: #333 }
article dl > dd { color: #555 }
nav + article { margin-top: 0 }
```
### <font color="blue">属性嵌套</font>
虽然很有意思 但是我觉得并没有什么用
```scss
.nav {
  border: {
    style: solid;
    width: 1px;
    color: #ccc;
  }
}

.cf {
  border: 1px solid #ccc {
    left: 0px;
    right: 0px;
  }
}
```
>编译后
```css
.nav {
  border-style: solid;
  border-width: 1px;
  border-color: #ccc;
}

.cf {
  border: 1px solid #ccc;
  border-left: 0px;
  border-right: 0px;
}
```
# **导入SCSS文件**
**@import** scss的import功能，简单说就是复制粘贴的功能。
导入时可以不写文件名后缀
### <font color="blue">不编译专门用来**imported**的文件</font>
当通过@import把scss样式分散到多个文件时，你通常只想生成少数几个css文件。那些专门为@import命令而编写的scss文件，并不需要生成对应的独立css文件，这样的scss文件称为局部文件。对此，scss有一个特殊的约定来命名这些文件。

此约定即，scss局部文件的文件名以下划线开头。当你 **@import** 一个局部文件时，还可以不写文件的全名，即省略文件名开头的下划线。举例来说，你想导入 **themes/_night-sky.scss** 这个局部文件里的变量，你只需在样式表中写 `@import "themes/night-sky";`
### <font color="blue">默认变量值</font>
一般情况下，你反复声明一个变量，只有最后一处声明有效且它会覆盖前边的值。<br />
关键字 **!default** <br />
含义是：如果这个变量被声明赋值了，那就用它声明的值，否则就用这个默认值。<br />
下面是你的一个局部文件
```scss
$fancybox-width: 400px !default;
.fancybox {
  width: $fancybox-width;
}
```
在上例中，如果用户在导入你的scss局部文件之前声明了一个$fancybox-width变量，那么你的局部文件中对$fancybox-width赋值400px的操作就无效。如果用户没有做这样的声明，则$fancybox-width将默认为400px。

### <font color="blue">嵌套导入</font>
假设你有一个局部文件_blue-theme.scss
```scss
aside {
  background: blue;
  color: white;
}
```
>然后把它导入到一个CSS规则内
```scss
.blue-theme {@import "blue-theme"}
```
>编译后
```css
.blue-theme {
  aside {
    background: blue;
    color: #fff;
  }
}
```
### 原生的CSS导入
即是用css的原生import机制，需发起额外的http请求，下面的三种情况会造成这种情况

- 被导入文件的名字以.css结尾
- 被导入文件的名字是一个URL地址
- 被导入文件的名字是CSS的url()值

如果需要导入css文件，可以把原始的css文件改名为.scss后缀，即可直接导入
# 混合器
又是一个可怕的“复制粘贴”功能

**@mixin** 定义混合器<br />
比如下面这个混合器(mix in)
```scss
@mixin rounded-corners {
  -moz-border-radius: 5px;
  -webkit-border-radius: 5px;
  border-radius: 5px;
}
```
**@include** 使用混合器
```scss
notice {
  background-color: green;
  border: 2px solid #00aa00;
  @include rounded-corners;
}
```
>最终编译后
```css
.notice {
  background-color: green;
  border: 2px solid #00aa00;
  -moz-border-radius: 5px;
  -webkit-border-radius: 5px;
  border-radius: 5px;
}
```
混合器中不仅可以包含属性，也可以包含css规则，包含选择器和选择器中的属性
```scss
@mixin no-bullets {
  list-style: none;
  li {
    list-style-image: none;
    list-style-type: none;
    margin-left: 0px;
  }
}

ul.plain {
  color: #444;
  @include no-bullets;
}
```
>编译后
```css
ul.plain {
  color: #444;
  list-style: none;
}
ul.plain li {
  list-style-image: none;
  list-style-type: none;
  margin-left: 0px;
}
```
混合器中的规则甚至可以使用scss的父选择器标识符&。使用起来跟不用混合器时一样，scss解开嵌套规则时，用父规则中的选择器替代&。

### <font color="blue">给混合器传参</font>
混合器并不一定总得生成相同的样式。可以通过在@include混合器时给混合器传参，来定制混合器生成的精确样式。当@include混合器时，参数其实就是可以赋值给css属性值的变量。
例如：
```scss
@mixin link-colors($normal, $hover, $visited) {
  color: $normal;
  &:hover { color: $hover; }
  &:visited { color: $visited; }
}
```
使用时传入参数
```scss
a {
  @include link-colors(blue, red, green);
}
```
>编译后
```css
a { color: blue; }
a:hover { color: red; }
a:visited { color: green; }
```
支持`$name: value`的形式指定每个参数的值
```scss
a {
  @include link-colors(
    $normal: blue,
    $visited: green,
    $hover: red
  );
}
```
为了在@include混合器时不必传入所有的参数，我们可以给参数指定一个默认值
```scss
@mixin link-colors(
    $normal,
    $hover: $normal,
    $visited: $normal
  ){
  color: $normal;
  &:hover { color: $hover; }
  &:visited { color: $visited; }
}
```
<hr />
关于继承(extend)不做记录，我想象不到用在何处，另外函数啊，if、for、while等等都不做记录，毕竟css在网页中不是“动态”的 这些特性用处我觉得有限
<hr />
