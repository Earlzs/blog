---
layout: react-native
title: Android打包生成Apk
date: 2017-12-12 12:01:37
tags:
- react-native
---

 react-native Android打包生成apk的过程
<!-- more -->


# 1, 产生签名的key

该过程会用到keytool，开发过安卓的都应该接触过该东西。详细请见[密钥和证书管理工具](https://docs.oracle.com/javase/7/docs/technotes/tools/solaris/keytool.html)

在项目的主目录中执行：

` keytool -genkey -v -keystore my-release-key.keystore -alias my-key-alias -keyalg RSA -keysize 2048 -validity 10000`
然后就会出现下图
![没啥说的只想告诉大家最后一步,一定要输入个是~](http://upload-images.jianshu.io/upload_images/4985985-a5e7befee857b9a5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
现在自己看你的项目跟目录发现多了一个 `my-release-key.keystore文件`

**[注：在产生的时候需要提供密钥和存储密码，后续会用到]**

# 2, 设置gradle变量
1.把my-release-key.keystore文件放到你工程中的android/app文件夹下。
2.编辑 android/gradle.properties（没有这个文件你就创建一个），添加如下的代码（注意把其中的****替换为相应密码）
```
MYAPP_RELEASE_STORE_FILE=my-release-key.keystore
MYAPP_RELEASE_KEY_ALIAS=my-key-alias
MYAPP_RELEASE_STORE_PASSWORD=xx 
MYAPP_RELEASE_KEY_PASSWORD=xx
** [注意替换xx为你自己设置的密钥和存储密码]**
```
>关于密钥库的注意事项:
一旦你在Play Store发布了你的应用，如果想修改签名，就必须用一个不同的包名来重新发布你的应用（这样也会丢失所有的下载数和评分）。所以请务必备份好你的密钥库和密码。

#3, 添加签名到项目的gradle配置文件
**编辑你项目目录下的android/app/build.gradle，添加如下的签名配置：**
```
...
android {
    ...
    defaultConfig { ... }
    signingConfigs {
        release {
            storeFile file("my-release-key.keystore文件的绝对路径")
            storePassword  "密钥密码"
            keyAlias "my-key-alias" 
            keyPassword "存储密码"
        }
    }
    buildTypes {
        release {
            ...
            signingConfig signingConfigs.release
        }
    }
}
...
```


#4,生成发行APK包
`$ cd android && ./gradlew assembleRelease`
然后就可以在下图目录中找到apk文件了
![apk](http://upload-images.jianshu.io/upload_images/4985985-e6e230f9ab27ffc6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**注意每次打包新的apk时候要删除之前的!!!**