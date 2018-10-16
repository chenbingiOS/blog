---
layout:     post
title:      "UITableViewHeaderFooterView在Xib下背景色警告"
subtitle:   "UITableViewHeaderFooterView Under Xib Background Color Warning"
date:       2017-12-22
author:     "ChenBing"
header-img: "img/post-bg-touch.jpg"
header-mask: 0.3
tags:		[Xib,Nib,UITableViewHeaderFooterView]
---

[TableView] Setting the background color on UITableViewHeaderFooterView has been deprecated. Please set a custom UIView with your desired background color to the backgroundView property instead.

无法改变UITableViewHeaderFooterView的背景色

出现这个警告，如果你是使用XIB创建的View，检查background是否改为Default
![](/img/blog-2017-12-22-UITableViewHeaderFooterView-background-color.png)