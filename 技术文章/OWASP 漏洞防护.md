#漏洞 #xss

> [!网址]
> Owasp: [Introduction - OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/)
> 


# XSS 跨站脚本攻击

该术语已经扩大到基本上包括任何内容的注入.
XSS 攻击很严重，可能导致帐户冒充、观察用户行为、加载外部内容、窃取敏感数据等。

**没有单一的技术可以解决 XSS，因此需要使用正确的防御技术组合来防止 XSS。**

## 分类

反射型XSS攻击

存储型XSS攻击

DOM XSS攻击

## 框架安全性

使用框架对变量进行正确验证,转义和清理!


## 输出编码

拦截器/过滤器部署为 XSS 防御不是抵御 XSS 攻击的有用方法

有许多不同的输出编码方法，因为浏览器解析 HTML、JS、URL 和 CSS 的方式不同。使用错误的编码方法可能会引入弱点或损害应用程序的功能。

## HTML清理

## 无效的方法

1. csp表头
2. 对http拦截器的依赖


