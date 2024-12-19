#漏洞 #ssrf

> [!网址]
> [Server Side Request Forgery Prevention - OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/cheatsheets/Server_Side_Request_Forgery_Prevention_Cheat_Sheet.html)
> 

# 介绍

什么是服务端请求伪造攻击?

SSRF 漏洞允许攻击者从易受攻击应用程序的后端服务器发送精心设计的请求。犯罪分子通常使用 SSRF 攻击来**攻击位于防火墙后面且无法从外部网络访问的内部系统**。攻击者还可能利用 SSRF 访问通过被利用服务器的环回接口 （127.0.0.1） 提供的服务。

当攻击者对 Web 应用程序发送的请求拥有完全或部分控制权时，就会出现 SSRF 漏洞。一个常见的示例是攻击者可以控制 Web 应用程序向其发出请求的第三方服务 URL。

**例子**

攻击者拥有 _url_ 参数的完全控制权。他们可以向 Internet 上的任何网站和服务器 （_localhost_） 上的资源发出任意 GET 请求。

攻击者甚至可以利用 SSRF 发挥创意，[对内部 IP 运行端口扫描。](https://www.acunetix.com/blog/articles/ssrf-vulnerability-used-to-scan-the-web-servers-network/)

除了 ` http:// 和 https:// ` URL 架构之外，攻击者还可以利用鲜为人知或遗留的 URL 架构来访问本地系统或内部网络上的文件。如利用 `file:///` 架构 , `dict://` 架构

SSRF 漏洞通常允许攻击者使用其他危险技术进行跟进

# 减少 SSRF 的防护措施

## 低效方法

黑名单检查,攻击者总能找到绕过它们的方法。在这种情况下，攻击者可以使用 HTTP 重定向、通配符 DNS 服务（如 _xip.io_）甚至[备用 IP 编码](http://www.pc-help.org/obscure.htm)。

## 有效方法

**白名单和 DNS 解析**

避免服务器端请求伪造 （SSRF） 的最可靠方法是将应用程序需要访问的主机名（DNS 名称）或 IP 地址列入白名单。

**响应处理**

为防止响应数据泄露给攻击者，您必须确保收到的响应符合预期。在任何情况下，都不应将服务器发送的请求的原始响应正文传递给客户端。

**禁用未使用的 URL 架构**

如果您的应用程序仅使用 HTTP 或 HTTPS 发出请求，请仅允许这些 URL 架构。如果您禁用未使用的 URL 架构，攻击者将无法使用 Web 应用程序使用具有潜在危险的架构（如 ` _file:///_、_dict://_、_ftp://_ 和 _gopher://_`）发出请求。

**内部服务上的身份验证**

默认情况下，Memcached、Redis、Elasticsearch 和 MongoDB 等服务不需要身份验证。攻击者可以使用服务器端请求伪造漏洞来访问其中一些服务，而无需任何身份验证。因此，为了保护您的敏感信息并确保 Web 应用程序安全，最好尽可能启用身份验证，即使对于本地网络上的服务也是如此。

# 具体防护清单

输入验证
- String containing business data. --字符串
- IP address (V4 or V6). --Ip
- Domain name. --域名
- URL. 
注意:禁用以下**重定向**以防止逃过输入校验

白名单