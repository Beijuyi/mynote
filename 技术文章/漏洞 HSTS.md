#漏洞 #HSTS

> [!参考文档]
> 网址: [owasp hsts 防护清单](https://cheatsheetseries.owasp.org/cheatsheets/HTTP_Strict_Transport_Security_Cheat_Sheet.html)

# 什么是 HSTS?

HTTP Strict Transport Security ( **HSTS**) ,http 严格传输安全, 是一种可选的 web 安全增强方式, 由 Web 应用程序通过使用特殊响应标头指定。受支持的浏览器收到此标头后，该浏览器将阻止通过 HTTP 向指定域发送任何通信，而是通过 HTTPS 发送所有通信。它还可以阻止浏览器上的 HTTPS 点击提示。

该规范已于 2012 年底由 IETF发布并公布为 [RFC 6797 （HTTP 严格传输安全 (HSTS)）](http://tools.ietf.org/html/rfc6797)

HSTS 解决了以下威胁：
- 用户收藏或手动输入`http://example.com`，容易受到中间人攻击
    - HSTS 自动将目标域的 HTTP 请求重定向到 HTTPS
- 原本应采用纯 HTTPS 的 Web 应用程序无意中包含 HTTP 链接或通过 HTTP 提供内容
    - HSTS 自动将目标域的 HTTP 请求重定向到 HTTPS
- 中间人攻击者尝试使用无效证书拦截受害用户的流量，并希望用户接受错误的证书
    - HSTS 不允许用户覆盖无效证书消息


**Strict-Transport-Security**
