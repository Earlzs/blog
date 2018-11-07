---
title: hexo搭建个人博客笔记
date: 2017-10-19 11:16:05
tags:  hexo
---
之前一直都很羡慕别人有自己的博客，这几天空闲下来。想搭建一个属于自己的博客。在查阅资料后发现[hexo](https://hexo.io/) + [github](github.com)就可以搭建,在此记录一下搭建博客过程中遇到的一些问题

<!-- more -->

## 配置环境
+ 安装Node（必须）
   作用：用来生成静态页面的
   到Node.js官网下载相应平台的最新版本，一路安装即可。
+ 安装Git（必须）
  作用：把本地的hexo内容提交到github上去.
+ 申请GitHub（必须）


> 安装的教程我都不多说了，贴几个链接自行安装
> 1. [node.js下载完傻瓜式安装](https://nodejs.org/en/)
> 2. [hexo有中文版教程](https://hexo.io/)
> 3. github要建立仓库


###   1 配置hexo关联github
找到hexo新建项目根目录下的 `_config.yml文件`  来建立关联,翻到最下面改成这个样子
``` bush
deploy:
     type: git
     // ！这个是你github项目下的仓库地址  新建仓库名字一定要对应起来！
     repo: git@github.com:Earlzs/Earlzs.github.io.git   
     branch: master
```
然后执行命令  `npm install hexo-deployer-git --save`
这个应该是装了一个hexo的插件 根据 `_config.yml`文件下刚才我们配置的东西直接方便直接推送至git上面


###   2 部署步骤 

我们可以在本地浏览自己的博客  执行`hexo server (简写hexo s)`

每次部署到github上的步骤，可按以下三步来进行。
1. hexo clean (可以省略不写,如果直接执行2，3有问题的话试一下)
2. hexo generate(简写 hexo g)
3. hexo deploy(简写 hexo d)

###  3 主题推荐

hexo 默认为我们配置了一个主题在 `themes目录下可以找到`
当然如果你不喜欢的话  [主题列表](https://github.com/hexojs/hexo/wiki/Themes) 这里有大量的主题供我们下载
我的博客使用的是  [Yilia](https://github.com/litten/hexo-theme-yilia)
找到自己喜欢的主题后
``` bush
git clone https://github.com/litten/hexo-theme-yilia.git themes/yilia
```
然后在`项目根目录下的themes目录下就可以看到了`
在将根目录下的` _config.yml文件` 修改  ： `theme: yilia`


###  4 添加『RSS订阅』

安装 [hexo-generator-feed](https://github.com/hexojs/hexo-generator-feed) 插件
```bush
npm install hexo-generator-feed --save
```
执行 hexo g  然后在 public目录下就可以看到  `atom.xml` 文件
最后在去你选择的主题目录下找到`_config.yml `配置如下
```bush
subnav:
  rss: "/atom.xml"
```



###  5  在网站底部加上访问量

1. 打开\themes\yilia\layout\footer.ejs文件,在底部加上
```bush
<script async src="https://dn-lbstatics.qbox.me/busuanzi/2.3/busuanzi.pure.mini.js"></script>
```
2. 然后再合适的位置添加显示统计的代码，如图：
```bush
<div class="powered-by">
<i class="fa fa-user-md"></i><span id="busuanzi_container_site_uv">
本站访客数:<span id="busuanzi_value_site_uv"></span>
</span>
</div>
```
3. 添加之后再执行hexo d -g，然后再刷新页面就能看到效果


### 6 添加顶部加载条

打开/themes/next/layout/_partials/head.ejs文件
```bush
<script src="//cdn.bootcss.com/pace/1.0.2/pace.min.js"></script>
<link href="//cdn.bootcss.com/pace/1.0.2/themes/pink/pace-theme-flash.css" rel="stylesheet">   //pink可以自行改成自己喜欢的颜色
```


### 7 添加404公益页面

新建404.html页面放到source目录下面。（注意，不要放到source下的post里面）
放在source下的文件会被上传但不会被解析到文章里面
```bush
<!DOCTYPE html>
<html>
<head>
	<meta charset="UTF-8">
	<title>404</title>
</head>
<body>
	自定义
</body>
</html>
```
或者 加入腾讯公益
```bush
<!DOCTYPE html>
<html>
<head>
	<meta charset="UTF-8">
	<title>404</title>
</head>
<body>
	<script type="text/javascript" src="//qzonestyle.gtimg.cn/qzone/hybrid/app/404/search_children.js" charset="utf-8" homePageUrl="http://yoursite.com/yourPage.html" homePageName="回到我的主页"></script></script>
</body>
</html>
```
在本地是看不到的~ 需要到你的Github托管地址看


