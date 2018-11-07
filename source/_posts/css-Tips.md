---
title: css-Tips
date: 2017-12-25 11:00:31
tags:
- css 
---




# 如何去掉 chrome input 的背景黄色

当我们在做登陆页面的时候，在 chrome 中 input 会带上自动补全的黄色背景，大大影响美观。很多网站都没有去处理，但这并不难处理。作为高逼格的前端，这里就可以体现出你的价值，解决方案如下：
```
input:-webkit-autofill {
  -webkit-box-shadow: 0 0 0px 1000px rgba(255, 255, 255, 0.5) inset !important;
}
```


当然，你也可以使用方案二，如下：

```
input:-webkit-autofill {
  -webkit-animation-name: autofill;
  -webkit-animation-fill-mode: both;
}
@-webkit-keyframes autofill {
  to {
    color: #fff;
    background: transparent;
  }
}
```

深入了解请移步：
1. [http://blog.csdn.net/wangxiaohui6687/article/details/10149579](chrome 表单自动填充去掉 input 黄色背景解决方案)
2. [https://stackoverflow.com/questions/2781549/removing-input-background-colour-for-chrome-autocomplete](Removing input background colour for Chrome autocomplete)



# CSS实现单行、多行文本溢出显示省略号（…）

### 1. 如果实现单行文本的溢出显示省略号我们应该都知道用text-overflow:ellipsis属性来，当然还需要加宽度width属来兼容部分浏览。

实现方法：

```
overflow: hidden;
text-overflow:ellipsis;
white-space: nowrap;
```

### 2. 如果我们要实现多行文本溢出显示省略号呢?

多行文本溢出，我们可以使用WebKit的CSS扩展属性，该方法适用于WebKit浏览器及移动端
```
/*
  display:-webkit-box;将对象作为弹性伸缩盒子模型显示 
  -webkit-box-orient:vertical;设置或检索伸缩盒对象的子元素的排列方式 。
  -webkit-line-clamp:3;设置显示多少行
*/
```