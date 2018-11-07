---
title: React-native中使用redux
date: 2017-12-12 12:09:13
tags: react
---

 >    Flux就像眼镜,你会知道你在什么时候需要它  
 <!-- more  -->

 
# redux个人理解
使用redux的目的是方便我们理清各个组件的state状态,我们在组件中使用props传递数据传来传去也容易搞混,这个时候,就是redux大显神威的时候,
1. 单一数据源,所有数据都存储在store中,我们需要什么数据就是store调用
2. state只读，state 只能通过action进行更改.
3. 在改变state tree时，用到action，但是我们是通过reducers来处理函数的

#   添加redux相关依赖

```
1   npm install  redux react-redux   -S  //这两个是必须的
2   npm install redux-logger(日志记录)   redux-thunk(支持异步请求)    -S    //中间件可选
3   react-native run-android
 ```


# redux中几个重要的概念

#### 1. Action 
      
 中文译为行为.像它的名字一样,我们在redux体系中修改一个状态必须先发出action ,action是行为,一个行为可以分为不同的几种类型,比如打人,打别人左脸是一种type ,打右脸也是一种type,当然选择不打也是这种行为的一种体现方式.我们选择的type就会影响到这个人受挨打后的状态,那也就是他可能第二天左脸肿了,或右脸.
所以这里一种打人行为的几种处理方式,会造成不同的表现结果,也就是我们所谓的 ui对应的各种展现方式.

![计数](http://upload-images.jianshu.io/upload_images/4985985-a58fad88cb896f2f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
我在这里是定义了三种关于计数的type 

#### 2. dispatch 
说完了行为,该说调度了,我们定义了各种各样的行为,但是之前也说过redux是单向数据流,想要触发action我们需要使用我们触发了一个计数的增加操作dispath(increase)
![增加](http://upload-images.jianshu.io/upload_images/4985985-ec545f2f3d7d2b98.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



#### 3.  connect
![connect](http://upload-images.jianshu.io/upload_images/4985985-ee066f46592a0bab.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


  React-Redux 提供connect方法，用于从 UI 组件生成容器组件。connect的意思，就是将这两种组件连起来。
  connect 在我理解看来就是,连接reducers处理函数与ui的一个方法.
+ mapStateToProps会订阅 Store，每当state更新的时候，就会自动执行，重新计算 UI 组件的参数，从而触发 UI 组件的重新渲染。
+ mapDispatchToProps是connect函数的第二个参数，用来建立 UI 组件的参数到store.dispatch方法的映射。也就是说，它定义了哪些用户的操作应该当作 Action，传给 Store。它可以是一个函数，也可以是一个对象。

#### 4. reducers
    
 dispath 将action派发到reducers中我们根据派发过来的actions.type来具体确定使用什么业务逻辑来改变一些数据,致使ui发生改变,如图,我们可以自定义state的初始状态
![image.png](http://upload-images.jianshu.io/upload_images/4985985-f440cedc079d52ec.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
**顺带插一嘴**
combineReducers  
我们呢在实际项目中,不可能只有counter这一个reducers处理函数,肯定会有非常多,那么 combineReducers 就是用来管理这一堆reducers的
![将counter引入进来,组织好后统一暴露出去](http://upload-images.jianshu.io/upload_images/4985985-29738a8145bbab34.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 5. store
 说了半天,store哪里去了,刚才有没有好奇reducers处理完成后,store如何更新的?应该这么问reducers处理函数直接影响了新的视图,但是好像并没有看到他们在哪里关联?看完下面就明白了
![store](http://upload-images.jianshu.io/upload_images/4985985-73658aa970dcea32.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Provider包裹住整个app将 store传入,起点即终点,完成一次循环,看到这里全部连起来了吧.
![Provider](http://upload-images.jianshu.io/upload_images/4985985-a2833f93a0a0f138.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)









