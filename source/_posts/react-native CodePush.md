---
layout: react-native
title: CodePush
date: 2018-01-19 11:10:51
tags:
- react-native
---


## CodePush简介

<!-- more -->

CodePush 是微软提供的一套用于热更新 React Native 和 Cordova 应用的服务。
CodePush 是提供给 React Native 和 Cordova 开发者直接部署移动应用更新给用户设备的云服务。CodePush 作为一个中央仓库，开发者可以推送更新 (JS, HTML, CSS and images)，应用可以从客户端 SDK 里面查询更新。CodePush 可以让应用有更多的可确定性，也可以让你直接接触用户群。在修复一些小问题和添加新特性的时候，不需要经过二进制打包，可以直接推送代码进行实时更新。


CodePush 可以进行实时的推送代码更新：

- 直接对用户部署代码更新
- 管理 Alpha，Beta 和生产环境应用
- 支持 React Native 和 Cordova
- 支持JavaScript 文件与图片资源的更新

CodePush开源了react-native版本，[react-native-code-push](https://github.com/Microsoft/react-native-code-push) 托管在GitHub上。


## 安装 CodePush CLI


在终端输入:`npm install -g code-push-cli `   安装完毕以后提示输入：code-push -v确认是否安装成功 


## 创建Code-Push账号



在终端输入:code-push register，会打开注册页面让你选择授权账号

完成相应注册后将会获得一个key,复制到终端即可登录(code-push login)。


 账号相关命令：

 - code-push login 登陆

 - code-push logout 注销 (不注销的话,会一直登陆)

 - code-push access-key ls 列出登陆的token

 - code-push access-key rm <accessKye> 删除某个 access-key

 

 ## 集成CodePush SDK(Android)

下面我们通过如下步骤在Android项目中集成CodePush。
1. 在项目中安装 react-native-code-push插件，终端进入你的项目根目录然后运行
`npm install --save react-native-code-push`

2. 在Android project中安装插件。
CodePush提供了两种方式：RNPM 和 Manual，本次演示所使用的是RNPM。
运行npm i -g rnpm，来安装RNPM。

3. 运行 rnpm link react-native-code-push。这条命令将会自动帮我们在anroid文件中添加好设置。

`在终端运行此命令之后，终端会提示让你输入deployment key，这是你只需将你的deployment Staging key输入进去即可，如果不输入则直接单击enter跳过即可。`

4. 在 android/app/build.gradle文件里面添如下代码：

`apply from: "../../node_modules/react-native-code-push/android/codepush.gradle"  `
然后在/android/settings.gradle中添加如下代码:

```
include ':react-native-code-push' 
project(':react-native-code-push').projectDir = new File(rootProject.projectDir, '../node_modules/react-native-code-push/android/app')
```
5. 运行 code-push deployment -k ls <appName>获取 部署秘钥。

默认的部署名是 staging，所以 部署秘钥（deployment key ） 就是 staging。
第六步： 添加配置。当APP启动时我们需要让app向CodePush咨询JS bundle的所在位置，这样CodePush就可以控制版本。更新 MainApplication.java文件：

```
public class MainApplication extends Application implements ReactApplication {
  private final ReactNativeHost mReactNativeHost = new ReactNativeHost(this) {
    @Override
    protected boolean getUseDeveloperSupport() {
      return BuildConfig.DEBUG;
    }
    @Override
    protected String getJSBundleFile() {
      return CodePush.getJSBundleFile();
    }
    @Override
    protected List<ReactPackage> getPackages() {
     return Arrays.<ReactPackage>asList(
         ...  //这个BuildConfig.CODEPUSH_KEY在后面会看到
         new CodePush(BuildConfig.CODEPUSH_KEY, MainApplication.this, BuildConfig.DEBUG), // Add/change this line.
         ...
     );
  }

  };
  @Override
  public ReactNativeHost getReactNativeHost() {
      return mReactNativeHost;
  }
}

```

6. 关于deployment-key的设置

在上述代码中我们在创建CodePush实例的时候需要设置一个deployment-key,因为deployment-key分生产环境与测试环境两种,所以建议大家在build.gradle中进行设置。在build.gradle中的设置方法如下:

打开android/app/build.gradle文件,找到android { buildTypes {} }然后添加如下代码即可:

```
android {
    ...
    buildTypes {
        debug {
            ...
            // CodePush updates should not be tested in Debug mode
            ...
        }

        releaseStaging {
            ...
            buildConfigField "String", "CODEPUSH_KEY", '"<INSERT_STAGING_KEY>"'
            ...
        }

        release {
            ...
            buildConfigField "String", "CODEPUSH_KEY", '"<INSERT_PRODUCTION_KEY>"'
            ...
        }
    }
    ...
}

```


6. 修改versionName。
在 android/app/build.gradle中有个 android.defaultConfig.versionName属性，我们需要把 应用版本改成 1.0.0（默认是1.0，但是codepush需要三位数）。

```
android{
    defaultConfig{
        versionName "1.0.0"
    }
}
    
```

