---
layout: post
title: iOS App Extension工作原理以及通信
---

{{ page.title }}
================

<p class="meta">07 July 2014</p>

首先要说的是一个App Extension并不是一个App，你可以把它理解成App提供的一个小的Help工具插件，插件可以帮助其他应用App处理一些简单的工作，比如分享内容到社交网络，PS照片，文件存储等。这里把提供App插件的应用叫Containing App，比如Apple的Stocks App，提供了一个插件可以在通知中心快速的查看股价；把调用插件的App称作Hosting App，比如调用Stocks插件的iOS系统通知中心Today App。

####App Extension的生命周期

因为extension并不是一个App,所以生命周期跟App的生命周期还是有很大的区别的。大多数情况下当用户从Hosting App中选择一个extension的时候，extension的生命周期便开始了，Hosting App为extension发送一个请求以响应用户操作这样就开启了Extension的生命周期序幕。举个栗子：用户在Safari里面选择了一段文字，然后点击Share Button，在弹出的Panel里面按下Extension的按钮，这个时候Safari作为Hosting App将选中的文字封装成一个Request，然后在用户点击的那一瞬间将request发送给了extension。具体可以看下面这张图。

![lifecycle icon](http://cl.ly/image/0j1o3m380P2l/app_extensions_lifecycle_2x.jpg)


在图中的第二步，系统根据request实例化了Extension并且在他们之间建立了一个通信渠道。Extension使用Hosting App Request的信息显示视图，在上面Safari的例子中就是显示要分享的文字。

在第三步中，Extension会响应用户的执行或者取消任务的操作，如有有必要也可以使用后台进程去处理。用户回到Hosting App中，当Extension的任务完成之后，Host App会收到执行的结果。

extension执行任务不久之后，系统会结束Extension的生命周期，如图第四步。

####App Extension如何通信
Extension在运行的过程中，只能和Host app进行直接通信，没有办法和Containing App进行直接通信，因为Containing App有可能都没有运行。Host app和Containing App也是不能通信的。

![com icon](http://cl.ly/image/3G3Y1N1q203j/simple_communication_2x.jpg)


Extension不能和Containing App进行直接的通信，那么是否可以间接的通信呢？Host App发送请求给Extension的时候会带NSExtensionContext，利用这个Extension可以和Containing App进行间接的通信，比如在Extension中打开Containing App.另外Extension和Containing App可以使用共同私有数据进行通信，比如共同访问NSUserDefault.

![com1 icon](http://cl.ly/image/2d1o1Z1B2d0w/detailed_communication_2x.jpg)
