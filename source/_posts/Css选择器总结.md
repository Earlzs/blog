---
title: Css选择器总结
date: 2017-10-20 15:17:06
tags: 
- css 
---
在工作中不管是写css还是js我们都需要用到css选择器来定位到需要操作的元素，但使用了这么久才发现，自己用来用去好像一直是用的id,class之类的比较简单的选择器，不过也确实能满足我们的绝大部分需求，今天突然想对所有的选择器进行个知识梳理，以便于在今后的开后的开发过程中，更加灵活应用、得心应手！ （id选择器和class子代,后代,标签，分组,通配符之类的就不废话，相信大家也都知道。这篇文章主要记录一些我们平时用的比较少的选择器，但是在某些场合下不得不用的选择器！）


<!-- more -->


# 概念性的东西

> 要使用css对HTML页面中的元素实现一对一，一对多或者多对一的控制，这就需要用到> CSS选择器。
>   在 CSS 中，选择器是一种模式，用于选择需要添加样式的元素。


<br />

###   1 紧邻同胞选择符 +  
我一般叫它加号选择器    &nbsp;&nbsp;&nbsp;&nbsp; &nbsp; &nbsp;标签 1 + 标签 2
标签 2必须紧跟在其同胞标签 1的后面。
``` bash

只有内容为1的p标签会变红

h2 + p {background:red;}

<h2>hello</h2>
<p>1</p>
<p>2</p>
<p>3</p>
```
当我们需要给一列li添加border-right 为1px的边框时候，而最后一个添加border-right显然不太合适这个时候**紧邻同胞选择符**超级好用！


###   2 一般同胞选择符 ~
一般同胞选择符~  标签 2（不一定紧跟）在其同胞标签 1后面。
另外一句话说就是标签1的所有同级元素

```  bash
三个p标签都会变红

h2 ~ p {background:red;}

<h2>hello</h2>
<p>1</p>
<p>2</p>
<p>3</p>
```
<br />
###  3 属性选择器  a[attr]

选择所有指定属性的元素
```  bash
选择所有带有 target 属性的 a 元素。那些没有此属性的锚点将不会应用此样式。

a[target]{ 
     background:red;
 }
```


```  bash
当然我们还可以这样 选择所有属性为 target="_blank" 的 a 元素。只应用于在新标签页打开的链接，其他锚点将不受影响。

a[target=_blank]{
    background:blue
}
```


``` bash
还有这样（css2的属性选择器）  a[attr~=value] （选择属性值包含指定值的元素）  
这样      a[attr|=value]   （选择属性以指定值开始的元素）

~= 和 *= 都可以选择属性值包含指定值的元素，前者必须是一个独立的单词，
比如 test-a 和 test a 可以被选中，而 testa 不能被选中。

 

（css3的属性选择器）
a[href*="zs"]{
     background-color:red;
     font-size:20px;
 }
X[attr*=value]  这个选择器匹配元素属性值包含指定值的元素。
该选择器类似于与上面的选择器，但是比上面的选择器更强大更灵活。
* 符号可以选择指定属性值包含子字符串的元素，
也就说，只要属性值中带有指定的值，无需是一个单词，无需空格分开，就可以匹配。  

 
a[href^="https"]{
     background-color:red;
}
X[attr^=value]  这个选择器用于匹配元素属性值带有指定的值开始的元素。


img[src$=".png"]{
     border:2px solid red;
}
X[attr$=value]  这个选择器匹配元素属性值带有指定的值结尾的元素。

```


<br/>
 
### 4 伪类选择器 

伪类选择器遵循  love hate原则 （哈哈爱恨情仇呀）
什么是Love hate呢 ？
伪类选择器有 :link   :visited  :hover  :active 
为一个a标签添加点击链接的各个阶段的状态，只需要按照上面的顺序添加就不会出现奇怪的状态
当然伪类选择器还有 :target  :focus


###  5 伪元素选择器

伪元素和伪类虽然都可以用`:`来表示，但我们最好还是区分开来使用
- `:` 是伪类选择器
- `::` 是伪元素选择器
伪元素选择器我们用的最多的就是用来清除浮动了,

```  bash
.clearfix::before{
    content:'.';
    height:0;
    visibility:hidden;
    overflow:hidden;
    display: block;
    clear:both;
}
.clearfix{ 
    /*兼容 IE*/
     zoom: 1;
}
```
伪元素`:：before ::after`的用法非常多，我在此就不一一列举了


### 6 具体子元素选择器？（好吧我也不知道这类该叫什么）

一直对这类元素选择器的用法迷迷糊糊，使用起来都是尝试~怼样式

先把这类选择器统一列出来吧


- 1.  :first-child
  2.  :last-child
  3.  :nth-child(n)
  4.  :nth-last-child(n) 
- 5.  :first-of-type 
  6.  :last-of-type
  7.  :nth-of-type(n)
  8.  :nth-last-of-type(n)

我把他们分为两类方便记忆，一类是带of-type的一类是不带的
挑两个出来举个栗子

> :nth-child(n)    
>
>  w3c上的定义为选择器匹配属于其父元素的第 N 个子元素，不论元素的类型。

``` bash
 <style>
        div {
            margin-top: 60px;
        }
        p {
            background-color: #ccc;
        }
        .test > :nth-child(3) {
            background: red;
        }
</style>

<body>
    <p>1</p>
    <p>2</p>
    <p>3</p>
    <div class="test">
        <p>hello</p>
        <h1>我不是P标签</h1>
        <p>world</p>

    </div>
    <div class="one">
        <p>hello</p>
        <p>world</p>
        <h1>我不是P标签</h1>
    </div>
</body>


```
显示如图所示
![first-chid()](1.png)

所以我们在这里匹配的是属于其test的第 3 个子元素变成了红色！
<br/>
> :first-of-type   
>
>  w3c上的定义为选择器匹配属于其父元素的特定类型的首个子元素的每个元素。

我们稍微改下代码 

``` bash
.test > :first-of-type {
            background: red;
        }
```

可以看到test下的第一个P标签和第一个h1标签的颜色都发生了改变。
![first-of-type](2.png)
其他的应该都是一样的。我就不一一列举了

**其实我们用到最多的还是隔行变色的这个功能**
``` bash
 p:nth-child(2n){
     background:red
 }
 p:nth-child(2n+1){
     bgackground:blue
 }

 这里2n和2n+1 可以换成  odd 和even  当然如果填3n的话就是每隔3行执行一次
```
<br/>


###  7 :only-of-type 和 :only-child 


如何区分这两个呢

``` bash
<style>
      .test1 >:only-child{
        background-color: green;
        margin-bottom: 50px;
    }

    .test2 >:only-of-type{
        background-color: red
    }
</style>
    <div class="test1">
        <p>我是:only-child </p>
        <h1>因为加上我所以失效了</h1>
    </div>
    <div class="test2">
        <p>我是only-of-type</p>
        <h1>11111</h1>
        <h2></h2>
    </div>
    <div class="test2">
            <p>我是only-of-type</p>
            <p>因为加上我所以test2中所以得p标签失效</p>
            <h1>11111</h1>
            <h2>2222</h2>
    </div>
```
![only-chid](3.png)
- `:only-of-type` 父元素下可以有多个元素，但每种类型的标签只能有一个，如果同一类型标签出现了两次，那么本类型标签失效（其实就是没有选中）
- `:only-child ` 父元素下可以只能有一个！

<br/>
注意：IE9+ 以及所有浏览器都支持该选择器，IE8 以及更早版本的浏览器不支持这两个属性。

<br/>

###   8 :not(selector)  
这个选择器还是很强大的，用于匹配非指定元素/选择器的每个元素，可以理解为取反的意思，即除了指定的元素以外所有元素。


<br/>
注意：IE9+ 以及所有浏览器都支持该选择器，IE8 以及更早版本的浏览器不支持。

###   9 ::selection  
::selection 选择器匹配元素中被用户选择或处于高亮状态的部分。
::selection只可以应用于少数的CSS属性：color、background、cursor 和 outline。
下面的代码，当元素被用户选中后会显示为红色的字体：


<br/>
注意：IE9+ 以及所有浏览器都支持该选择器，但是 Firefox 需要通过其私有属性
::-moz-selection才能获得支持 。

<br/>

### 10 :empty
这个选择器用于匹配没有子元素的每个元素，包括文本节点。
选择所有没有任何子级的元素，也就是说选择页面中所有指定的空元素。

<br/>
　注意：IE9+ 以及所以浏览器都支持该选择器，IE8 以及更早版本的浏览器不支持。
<br/>

###  11 :root
:root 匹配文档的根元素，在 HTML 中，根元素始终是 html 元素。
<br/>
　注意：IE9+ 以及所以浏览器都支持该选择器，IE8 以及更早版本的浏览器不支持。
<br/>


###  12 :enabled    :disabled 

- :enabled用于匹配每个启用的元素，主要用于表单元素。 
- :disabled 用于匹配每个禁用的元素，主要用于表单元素。
下面的例子，设置所有 type="text" 的已启用的 input 元素设置背景色：

``` bush
<style>
  input[type="text"]:enabled{
      background:yellow;
  }
</style>


<form action="">
     姓名: <input type="text" value="小明" /><br/>
     爱好: <input type="text" value="捣蛋" /><br/>
     籍贯: <input type="text" disabled value="汉" />
</form>
```
![enabled](4.png)

<br/>
　注意：IE9+ 以及所以浏览器都支持该选择器，IE8 以及更早版本的浏览器不支持。
<br/>

###  12 :checked 


匹配每个选中的输入元素，仅适用于单选按钮或复选框。
下面的例子，为所有被选中的 input 元素设置背景色：

``` bush
<style>
 input:checked{
      background:red;
}
</style>

 <form action="">
     <input type="radio" checked name="like" value="love" />喜欢<br>
     <input type="radio" name="like" value="noLove" />不喜欢<br>
     <input type="checkbox" checked value="散步" />散步<br>
     <input type="checkbox" value="跑步" />跑步
</form>
```
<br/>
注意：目前只有 IE9+ 和 Opera 浏览器支持该选择器，Chrome 和 Firefox 不支持。而且在 IE9/IE10/IE11 中显示有差异。
<br/>

在 IE9 和 IE10 中显示如下： ![ie9和ie10](5.png)
在 IE11 中取消了对复选框的支持，显示如下： ![ie11](6.png)


###  13 : readonly

readonly 属性规定输入字段为只读。
只读字段是不能修改的。不过，用户仍然可以使用 tab 键切换到该字段，还可以选中或拷贝其文本。
readonly 属性可以防止用户对值进行修改，直到满足某些条件为止（比如选中了一个复选框）。然后，需要使用 JavaScript 消除 readonly 值，将输入字段切换到可编辑状态。
readonly 属性可与 `<input type="text">` 或 `<input type="password">` 配合使用。



### 14 :optional 和 :required

匹配可选的输入元素，在表单元素是可选项时设置指定的样式，即未设置  required 属性的表单素。
required 属性是 HTML5 新增加的表单属性，用于规定必需在提交表单之前填写输入字段。
表单元素中如果没有特别设置 required 属性即为可选的。
```bush
 <style>
 input:optional{
     background-color:yellow;
 }
input:required{
     background-color:green;
 }
 </style>
 </head>
 <body>
 <p>可选的 input 元素：<input type="text" /></p>
 <p>必填的 input 元素：<input type="text" required /></p>
 </body>
```
![:optional和:required](8.png)
<br>
注意： :optional  :required 选择器只适用于表单元素：input、select 和 textarea。如下：
注意： IE10+ 以及所有浏览器都支持该选择器，IE9 以及更早版本的浏览器不支持。
<br>


### 15 :valid 和 :invalid

匹配输入值为合法的元素，在表单元素的值需要根据指定条件验证时设置指定样式。
**注意**： :valid 选择器只作用于能指定区间值的元素，例如 input 元素中的 min 和 max 属性，及正确的 email 字段，合法的数字字段等。如下：

``` bush

<style>
  input:valid{
      background-color:green;
  }
  input:invalid{
     border:2px solid red;
 }
</style>


<body>
 <p>合法邮箱：</p>
 <input type="email" value="demo@xx.com" />
 <p>非法邮箱：</p>
 <input type="email" value="demo.com" />
</body>
```

![valid和invalid](7.png)
<br>
注意：IE10+ 以及所有浏览器都支持该选择器，IE9 以及更早版本的浏览器不支持。
<br>


