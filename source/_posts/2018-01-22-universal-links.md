---
layout:     post
title:      "Universal Links 实现过程中的问题"
subtitle:   "The Universal Links implementation tutorial"
date:       2018-01-22
author:     "ChenBing"
header-img: "img/post-bg-touch.jpg"
header-mask: 0.3
tags:		[Universal Links]
---

应后台同学工作需要，研究了下 Universal Links 实现过程，这里提供几个文档参考：

> [官网文档][1] <br />
  [CocoaChina 上的一篇教程][2]

[1]: https://developer.apple.com/library/content/documentation/General/Conceptual/AppSearch/UniversalLinks.html#//apple_ref/doc/uid/TP40016308-CH12-SW2 
[2]: http://www.cocoachina.com/ios/20150902/13321.html

网上教程都有，就没有详细写的意义了，这边只列举注意事项，以及实现过程中遇到的问题：

**注意事项**
	
1. apple-app-site-association文件放到根目录后，通过浏览器访问会是一个下载文件，而不像其他公司能够打开这个文件。 <br />
这个问题我让后台调整了（确保上到到服务器中的文件是没有后缀的，是一个文本文件就可以）

2. 放 apple-app-site-association 的服务器是 Https 并且通过域名访问就足够了，和 Xcode 中工程配置文件 applink: 域名相同。其他 html 页面只需要调用对应域名的 Url 就能够唤起对应 App。

3. Xcode 运行 App 前，尽可能先卸载 App，App 在安装成功后会去 applink: 对应域名下载 apple-app-site-association。

4. 苹果的[测试网址](https://search.developer.apple.com/appsearch-validation-tool/)是不是通过，对 Universal Links 不影响（我的一直都是error）

5. 最坑的一个问题：微信 6.6.0 的版本居然开始封杀了 Universal Links，死跳不过，一定要点击页面右上角 ... 按钮，用 Safari 打开才能跳转。使用 6.5.15 版本测试却没有这个问题。（大坑）



