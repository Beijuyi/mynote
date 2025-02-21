#tomcat

# 概念

## Servlet

所谓Servlet，其实就是Sun为了让Java能实现动态可交互的网页，从而进入Web编程领域而制定的一套标准！Servlet 可理解为 server 加 applet, 即在服务端运行的小型程序.

一个Servlet主要做下面三件事情：
- 创建并填充Request对象，包括：URI、参数、method、请求头信息、请求体信息等
- 创建Response对象
- 执行业务逻辑，将结果通过Response的输出流输出到客户端

**Servlet没有main方法，所以，如果要执行，则需要在一个容器里面才能执行，这个容器就是为了支持Servlet的功能而存在，Tomcat其实就是一个Servlet容器的实现**。

一个servlet 可以配置支持多个 url 模式匹配, 原生 servlet 编程中需要程序员在实现中对不同 url 执行不同的方法调用.
而在 springmvc 框架下, 基于注解的方式可以更简单的匹配, 如@RequestMapping 根据 url 匹配.

Servlet 是 sun 公司定义的一个规范, 是一个接口

```java
public interface Servlet {  
    void init(ServletConfig var1) throws ServletException;  
  
    ServletConfig getServletConfig();  
  
    void service(ServletRequest var1, ServletResponse var2) throws ServletException, IOException;  
  
    String getServletInfo();  
  
    void destroy();  
}
```

HttpServlet 是 tomcat 实现的一个 Servlet 抽象类, 重写了 service () 方法, 根据请求方式细分为 doGet,doHead,doPost,doPut,doDelete,doOptions,doTrace 等方法

 DispatcherServlet，它是 Spring 中定义的一个 Servlet，实现了 Servlet 接口，本质也是一个 Servlet。
## Servlet容器

上面说到Servlet 只是一个处理请求的应用程序，光有Servlet是无法运行起来的，需要有一个 main 方法去调用你的这段 Servlet 程序才行。Servlet 容器就产生了, 其主要作用:
1. 建立连接；
2. 调用Servlet处理请求；
3. 响应请求给客户端；
4. 释放连接；

Servlet 容器可以有多个 servlet.


# 整体架构

> [!参考]
> https://www.cnblogs.com/zfcq/p/16069366.html
> https://tomcat.apache.org/tomcat-10.1-doc/architecture/overview.html
> https://www.cnblogs.com/kukuxjx/p/17325621.html

![](Pasted%20image%2020250121132937.png)

- **Server组件**：指的就是整个 Tomcat 服务器，包含多组服务（Service），负责管理和启动各个Service，同时监听 8005 端口发过来的 shutdown 命令，用于关闭整个容器 。
- **Service组件**：每个 Service 组件都包含了若干用于接收客户端消息的 **Connector** 组件和处理请求的 **Engine** 组件。 Service 组件还包含了若干 **Executor** 组件，每个 Executor 都是一个线程池，它可以为 Service 内所有组件提供线程池执行任务。
- **连接器Connector组件**：连接器对 Servlet 容器屏蔽了不同的应用层协议及 I/O 模型，无论是 HTTP 还是 AJP，在容器中获取到的都是一个标准的 ServletRequest 对象。
- **Engine**：引擎，Servlet 的顶层容器，用来管理多个虚拟站点，一个 Service 最多只能有一个 Engine
- **Host**：虚拟主机，负责 web 应用的部署和 Context 的创建。可以给 Tomcat 配置多个虚拟主机地址，而一个虚拟主机下可以部署多个 Web 应用程序, 即多个 Context.
- **Context**：Web 应用上下文，包含多个 Wrapper，负责 web 配置的解析、管理所有的 Web 资源。一个Context对应一个 Web 应用程序。
- **Wrapper**：表示一个 Servlet，最底层的容器，是对 Servlet 的封装，负责 Servlet 实例的创建、执行和销毁。一个 Context 可以有多个Servlet。
对应关系如下图: (少了 server, 一个 server 就是一个 tomcat进程, 各自监听不同的端口)
![](Pasted%20image%2020250122132622.png)

# Pipeline-Valve管道

Pipeline-Valve是责任链模式,责任链模式是指在一个请求处理的过程有多个处理者依次对请求进行处理,每个处理者负责做自己相应的处理,处理完成后将处理后的请求返回,再让下一个处理者继续处理.

