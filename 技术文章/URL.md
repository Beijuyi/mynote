#http #url

# URL 结构

```text
scheme：//user：pass@host：port/path？query=value#fragment
```


1. **scheme** (协议)：
    
    - 这是URL的第一个部分，指定了访问资源时所使用的协议。常见的协议包括 `http`、`https`、`ftp` 等。它告诉客户端应该如何与服务器通信。
2. **user:pass** (用户名:密码)：
    
    - 这个部分是可选的，用于HTTP基本认证。它可以包含一个用户名和密码，用冒号分隔，并跟随在协议之后且主机之前。格式为 `username:password`。由于安全原因，现代浏览器通常不推荐通过URL直接传递认证信息。
3. **@** (分隔符)：
    
    - 分隔认证信息（如果有的话）和主机名。
4. **host** (主机)：
    
    - 指定要连接的服务器的域名或IP地址。例如 `www.example.com` 或者 `192.168.1.1`。
5. **:port** (端口)：
    
    - 可选部分，指定服务器上的服务端口。如果省略，则使用默认端口，如HTTP的80端口，HTTPS的443端口等。
6. **/path** (路径)：
    
    - 指向服务器上资源的具体位置。路径以斜杠开始，后面跟随文件或目录的名字。它可以指向静态文件（如HTML页面）、动态脚本（如PHP、ASP.NET），或者其他类型的资源。
7. **?query=value** (查询字符串)：
    
    - 查询字符串由键值对组成，用来向服务器传递参数。多个键值对之间用`&`符号分隔。查询字符串对于GET请求特别重要，因为它允许网页或API根据传入的数据生成动态内容。例如：`?search=keyword&page=2`。
8. **#fragment** (片段标识符)：
    
    - 片段标识符（也称为锚点）是指向文档内部某个位置的标识符。它不会被发送到服务器，而是由浏览器用来定位页面内的特定部分。例如，在HTML页面中，它通常指向一个ID属性匹配的元素。

综上所述，一个完整的URL可能看起来像这样：`https://user:pass@www.example.com:8080/path/to/resource?query=value#section1`


# URL 解析实战

```java
String url = "http://localhost:8080@baidu.com#/encode"
```
1. Localhost: 8080 被当作用户名: 密码
2. 遇到 `#` 符号, url 会终止, 后面的字符不再处理!
最后发起的实际请求是: `http://baidu.com`


