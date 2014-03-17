---
layout: post
title: 仿iOS开关按钮SwitchButton
---

{{ page.title }}
================

<p class="meta">12 Jun 2013</p>

&emsp;&emsp;iOS上的UISwitch,由于不可以定制Frame,导致很多时候无法满足项目需求，所以我曾经使用CALayer写过一个[LPSwitch](https://github.com/cocoamad/LPSwitchControl)，不同UISwitch的是，LPSwitch可以随意设置Frame,同时可以设置背景色彩或者图片，支持iOS4.0以上。

![Mou icon](https://github-camo.global.ssl.fastly.net/5b98aff196a8bb4f78f89a14ef4f3583ada168f2/687474703a2f2f7777322e73696e61696d672e636e2f6c617267652f36636239656531316a773165356c6f7767713636326a323067693064733074352e6a7067)

&emsp;&emsp;这几天在编写Ever Weibo for Mac的时候，由于自己一直非常喜欢Metro UI Design, 想在界面设计上尝试使用这样一个风格，所以大量的控件需要重新绘制，刚好之前写过刚提到的LPSwitch，所以顺便把它移植到Mac上替代NSSegmentControl啦。相比较而言增加了两种风格：一种是RoundType，两外一种是SquareType。不多说了看图吧。

&emsp;&emsp;源码地址(GitHub)：[LPSwitchButton](https://github.com/cocoamad/LPSwitchButton)