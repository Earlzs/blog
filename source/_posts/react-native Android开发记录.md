---
layout: react-native
title: react-native Android开发记录
date: 2017-10-27 16:16:20
tags: 
react-native
---

公司最近要用react-native开发App,我负责android端的开发,在本地大概做了一些后,需要打包个apk看一下,查看了官网教程看不太懂.结合搜集来的一些资料终于打包成功,在此记录一下

<!-- more -->


### 1,修改app名称(android)

一般Android在打包的时候，如果没经过修改，那么应用显示的名称就是我们在react-native init时设定的名称，这肯定不是我们需要的，那么，如何修改呢？

我们打开项目的android/app/src/main/AndroidManifest.xml文件可以看到名称的设定：

![](1.png)

这说明app的显示名称在@string/app_name中进行了设定。
那我们直接继续打开android/app/src/main/res/values/strings.xml，即可看到配置中的app_name，修改为你想要的即可，如：


```
<resources>
    <string name="app_name">新的名称</string>
</resources>
 
```


### 2,修改app图标(android)

文件路劲: android/app/src/main/res/mipmap--xxx,
或者
         android/app/src/main/res/drawable--xxx
每一个目录下有不同大小的图标-- xxx.png<适配安卓不同机型>



### 3,控制app只能横竖屏

打开  AndroidManifest.xml 文件在 
activity下添加   android:screenOrientation=”landscape”属性即可(landscape是横向，portrait是纵向)。


### 4,Android下Box-shadow

在官方文档里,box-shadow只支持ios, 
目前找到一种方法实现可以给盒子添加  `elevation: 10`  不过不可以改颜色,默认是灰色的阴影




### 5,让安卓实现push动画

```
import CardStackStyleInterpolator from 'react-navigation/src/views/CardStackStyleInterpolator';

// 在StackNavigator配置headerMode的地方，使用transitionConfig添加
{
    headerMode: 'screen',
    transitionConfig:()=>({
        screenInterpolator:CardStackStyleInterpolator.forHorizontal,
    })
}

```


### 6,实现Android返回键点击两次退出应用

这是一开始的思路
```
import  BackHandler from 'react-native'

componentWillMount(){  
    BackHandler.addEventListener('hardwareBackPress', this._onBackAndroid );  
}  
  
componentUnWillMount(){  
    BackHandler.addEventListener('hardwareBackPress', this._onBackAndroid);  
}  
  
_onBackAndroid=()=>{  
    let now = new Date().getTime();  
    if(now - lastBackPressed < 2500) {  
        return false;  
    }  
    lastBackPressed = now;  
    ToastAndroid.show('再点击一次退出应用',ToastAndroid.SHORT);  
    return true;  
}

```

但是这段代码不管是在哪个界面都会显示提示'再点击一次退出应用',而我们想要的效果是如果界面不是根界面，点击返回按钮，返回上一页；如果是根界面，点击提示“再点击一次退出应用”，再次点击退出应用。
那怎么判断它是不是根界面呢？又在哪里判断呢？

react-navigation中有一个onNavigationStateChange方法，可以得到导航状态的改变，打印log如下：
![](2.jpg)

其中：prevNav是之前导航状态，nav是当前导航状态，action是当前进行的操作。所以我们可以通过当前导航的状态中的routes的length属性来判断当前界面是否为根界面，代码如下：
```

let routes = [];
let lastBackPressed = null;


    componentDidMount() {
        BackHandler.addEventListener('hardwareBackPress', this.onBackAndroid);
    }


    componentWillUnmount() {
        BackHandler.removeEventListener('hardwareBackPress', this.onBackAndroid);
        lastBackPressed = null;
    }

    onBackAndroid() {
        if (routes.length === 1) { // 只需要判断length是否为1就能确定是否在根界面
            if (lastBackPressed && lastBackPressed + 2000 >= Date.now()) {
                return false;
            }
            lastBackPressed = Date.now();
            Toast.showShortCenter('再点击一次退出应用');
            return true;
        }
    }
    render() {
        return (
            <AppNavigator
                onNavigationStateChange={(prevNav, nav, action) => {
                console.log('prevNav=',prevNav);
                console.log('nav=',nav);
                console.log('action=',action);
                routes = nav.routes;
            }}/>
        );
    }

```
