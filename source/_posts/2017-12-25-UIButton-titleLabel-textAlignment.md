---
layout:     post
title:      "UIButton的textAlignment属性无效"
subtitle:   "Can't change UIButton titleLabel property textAlignment"
date:       2017-12-25
author:     "ChenBing"
header-img: "img/post-bg-touch.jpg"
header-mask: 0.3
tags:		[UIButton, textAlignment]
---

如果想改变 UIButton 上按钮文字居左或者居右边对其，btn.titleLabel.textAlignment 属性是无效的。

需求：居左对其

无效方式
```
_btn.titleLabel.textAlignment = UITextAlignmentLeft;
```
应该这么做
```
_btn.contentHorizontalAlignment = UIControlContentHorizontalAlignmentRight;
```
这种对其方式会贴紧边框，可以设置内缩进
```
_btn.contentEdgeInsets = UIEdgeInsetsMake(0,0,0,20); // 上，左，下，右
```
