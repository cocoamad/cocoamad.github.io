#Message Forwarding 的一点思考

在实际使用Objc开发代过程中，我们常常会遇到下面这些编码的场景：

1. 当给一个非nil对象发送一个该对象并不存在的消息。
2. 类A想同时继承B,以及C。

以上两个都是我在实际的开发中实实在在所遇到问题，在此之前我都没有比较好的解决方案，但是直到了解了Message Forwarding...

###什么是Message Forwarding?
根据[Apple的开发文档](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/ObjCRuntimeGuide/Articles/ocrtForwarding.html)介绍，当给一个对象发送一个该对象不能处理的消息时会发生一些错误（你懂得），但是在发生错误之前系统运行时会给这个对象第二次机会处理这个消息，你犯了一个错误不要紧，我还给一次机会你。

###Message如何Forwarding?
当给一个对象发送他并不能处理的消息时，运行时会给你第二次机会处理这个消息，这里的第二次机会，其实就是会调用该对象的forwardInvocation:方法，随之并带有一个NSInvocation类型的参数，NSInvocation参数里面可以获取到原始消息以及消息的参数，这样如果你遇到了不能处理的消息，实现forwardInvocation就可以将消息转发给其他对象或者默认处理就好。

```
Note:跟动态函数调用一样，首先需要做respondsToSelector:或者isKindOfClass:类似的判断，在运行时决定是否调用forwardInvocation:之前，系统运行时会调用methodSignatureForSelector:方法，返回的是一个NSMethodSignature对象，这个对象包含了具体的是那个对象来相应哪个消息，如果返回nil，那么就会直接发生unrecognized selector sent to instance的crash。
```
###利用Message Forwarding如何解决上面两个问题？
#####给一个对象发送该对象不能处理的消息
这个就不用详细讲解了，你可以在你的类里面实现一个默认的方法，比如callUnrecognizedSelector:, 所有的不能处理的消息，全部转发给该自定义的默认处理函数，这个函数可以什么都不用做。如果想利用起来，可以在该函数里面统计上报所有转发到该出的消息（异常不能处理的消息），特别对于release版本。对于debug版本最好屏蔽转发默认，毕竟出现不能处理的消息不是我们所期望的，能够在debug发现就尽早发现解决掉。

#####实现多继承
Objc不能像其他语言一样实现多继承，对于其他语言转过来的开发同学，这用起来或多或少不太智能方便，但是使用Message Forwarding可以实现类似的多继承（伪多继承）。

![List icon](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/ObjCRuntimeGuide/Art/forwarding.gif)

如图如果想让勇士去谈判，但是勇士又不具备谈判的本领，这个时候勇士私底下就告诉自己的谈判专家去谈判，对外来看别人就会认为是勇士有了谈判的本领。其实还是谈判专家去谈的。和真正的多继承相比较而区别是：多继承是让一个单一对象结合多种不同的能力，就好比把谈判专家的谈判技能学会自己去谈判，趋向于更大多层面的对象。Message Forwarding是赋予不同的责任给不同的对象，分解问题给不同的小对象，通过透明的传递消息给这些不同的小对象而组合在一起。

因为Message Forwarding是运行时决定的，没有编译器约束，所以如果在没有明确的说明的情况下很容易出错。所以对于要使用消息传递的函数列表最好在明确的地方说明一下，这样自己或者其他人才会更清楚，避免犯错误。

利用消息传递还可以做其他很多的事情，比如在编写SDK的时候接口隐藏、代理功能等等，可以根据自己的需求是否使用Message Forwarding.


