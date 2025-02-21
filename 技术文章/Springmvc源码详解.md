#DispatcherServlet #springmvc


读源码前的前提工作:
1. 先思考, 为什么要读源码? 这个项目解决了什么问题, 这个问题是什么, 为什么会催生出这个项目, 把项目背景搞清楚
2. 继续思考, 这个项目解决了某个问题, 是怎么解决的, 方案是什么, 原理是什么, 核心点在哪里
3. 然后才是读源码的事情
读源码不是一上来就看代码, 而是先了解设计思想, 学习流程和逻辑, 然后再从代码中看如何用代码实现的, 包括代码的设计和具体实现.

# Springmvc 的产生解决了什么问题

Springmvc 是基于 MVC 的软件架构设计理念的一种优秀实现.
MVC将模型, 视图, 控制器的功能进行了清晰的区分, 让软件结构更加清晰.
Springmvc 是 spring 专门用来开发 web 项目的框架, 主要有以下特点:
1. 基于 mvc 设计理念, 对模型, 视图, 控制器进行了区分
2. 代码清晰简洁, 性能卓越
3. 与 spring 的 ioc, aop 等功能无缝衔接
4. 基于原生 servlet, 实现了**DispatcherServlet**前端控制器, 对请求和响应进行统一处理


# Springmvc 的工作流程是怎样的?

从网上找到一张图:
![](Pasted%20image%2020250119110243.png)



客户端发起请求
经过 web 容器, 如 tomcat 容器
过滤器 Filter
请求进入 servlet 容器, 先到前端控制器 DispatcherServlet


`DispatcherServlet#doService()`

`DispatcherServlet#doDispatch()`

``DispatcherServlet#getHandler()`

`DispatcherServlet#getHandlerAdapter()`

`HandlerAdapter#handle()`

`RequestMappingHandlerAdapter#invokeHandlerMethod()`

`ServletInvocableHandlerMethod#invokeAndHandle()`

`InvocableHandlerMethod#doInvoke()`

`method.invoke(getBean(), args)` 最后是通过反射来调用控制器 controller中的方法

![](Pasted%20image%2020250120130433.png)


# 各组件作用

## 前端控制器

前端控制器处理所有从用户过来的请求。所有用户的请求都要通过前端控制器。
Springmvc 中DispatcherServlet 就是一种前端控制器,它和Spring容器无缝整合在了一起.

**过程描述**: 
- 一个http请求到达服务器，被DispatcherServlet接收。
- DispatcherServlet将请求委派给合适的处理器Controller，此时处理控制权到达Controller对象。
- Controller内部完成请求的数据模型的创建和业务逻辑的处理，
- 然后再将填充了数据后的模型即model和控制权一并交还给DispatcherServlet，委派DispatcherServlet来渲染响应。
- DispatcherServlet再将这些数据和适当的数据模版视图结合，向Response输出响应。

# Springmvc 的设计亮点

**父类抽象处理流程，子类给予具体的实现**

# Springmvc 和 tomcat 是如何交互的?

**DispatcherServlet是tomcat与Spring程序进行连接的纽带。**
DispatcherServlet 是 springmvc 中的组件, 同时DispatcherServlet 又继承了HttpServlet,HttpServlet 又继承了GenericServlet,GenericServlet 实现了 tomcat 中的Servlet 接口.



https://www.cnblogs.com/kukuxjx/p/17325621.html
