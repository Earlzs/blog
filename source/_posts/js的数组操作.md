---
title: js的数组操作
date: 2017-12-19 15:53:36
tags:
- js
---

对js数组操作的一些疑问

<!-- more -->


# 数组去重

老生常谈的问题


### indexOf去重法

```
var arr=[1,1,3,'string','1','string'];
var s=[];
for(var i=0,j=arr.length;i<j;i++){
    if(s.indexOf(arr[i])==-1){
       s.push(arr[i])
    }
};
console.log(s)           //  1,3,'string','1'
```



### filter去重法

```
var arr=[1,1,3,'string','1','string'];
arr.filter((i,index,item)=>{
    return item.indexOf(i)==index
})

```


### ES6的Set去重法
 
```
var arr=[1,1,3,'string','1','string']
[...new Set(arr)]                           //简直不要太简单~
```



### includes()

Array.prototype.includes方法返回一个布尔值，表示某个数组是否包含给定的值，与字符串的includes方法类似。ES2016 引入了该方法。

```
var arr=[1,1,3,'string','1','string'];
var s=[];
for(var i=0,j=arr.length;i<j;i++){
    if(!s.includes(arr[i])){
       s.push(arr[i])
    }
};
console.log(s)           //  1,3,'string','1'
```




# 数组的遍历



###  1.for()循环


```
这个是最简单的版本
for(i = 0; i < arr.length; i++) {
    
} 
使用临时变量，将长度缓存起来，避免重复获取数组长度，当数组较大时优化效果才会比较明显。
for(i = 0,len=arr.length; i < len; i++) {
   
}

```


其实这块比较模糊的是forEach 和 map的区别,但还是把所有的遍历都记录一下吧


### 2.forEach()和map()的区别

##### map()

map定义和用法： 
+ map方法返回一个新的数组，数组中的元素为原始数组调用函数处理后的值。 (新数组跟原始数组处于一个映射关系)
+ 我的理解就是：原数组进行处理之后对应的一个新的数组。 
+ map()方法按照原始数组元素顺序依次处理元素。 
+ 注意：map()方法不会对空数组进行检测。 
+ map()方法不会改变原始数组。 


相同点：
+ 都是循环遍历数组中的每一项
+ forEach和map方法里每次执行匿名函数都支持3个参数，参数分别是item（当前每一项）、index（索引值）、arr（原数组）
+ 匿名函数中的this都是指向window
+ 只能遍历数组,不支持对象.



##### forEach()

为每个元素执行对应的方法,forEach是用来替换for循环的




###  3.find() filter() 和findIndex()

```
//filter用来过滤出数组所有符合条件的
var arr=[1,2,3,44,10,15]
arr.filter((i)=> i>9)              //44,10,15

//find 用来找到数组第一个符合条件的
var arr=[1,2,3,44,10,15]
arr.find((i)=> i>9)                //44
```

//findIndex 用来找到数组第一个符合条件的下标,如果所有成员都不符合条件，则返回-1。
var arr=[1,2,3,44,10,15]
arr.findIndex((i)=> i>9)           // 3   数组中第一个大于9的是44下标为3 

