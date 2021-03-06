## [[连接管理]]
[[连接管理#1 短连接与长连接]]
[[连接管理#2 流水线]]
## [[Cookie]]
HTTP 协议是无状态的，主要是为了让 HTTP 协议尽可能简单，使得它能够处理大量事务。HTTP/1.1 引入 Cookie 来保存状态信息。
## [[HTTP缓存]]
优点
- 缓解服务器压力；
- 降低客户端获取资源的延迟：缓存通常位于内存中，读取缓存的速度更快。并且缓存服务器在地理位置上也有可能比源服务器来得近，例如浏览器缓存。
## [[HTTP内容协商]]
通过内容协商返回最合适的内容，例如根据浏览器的默认语言选择返回中文界面还是英文界面。
## [[HTTP内容编码]]
内容编码将实体主体进行压缩，从而减少传输的数据量。
常用的内容编码有：gzip、compress、deflate、identity。
## [[HTTP范围请求]]
如果网络出现中断，服务器只发送了一部分数据，范围请求可以使得客户端只请求服务器未发送的那部分数据，从而避免服务器重新发送所有数据。
## 分块传输编码
Chunked Transfer Encoding，可以把数据分割成多块，让浏览器逐步显示页面。
## [[多部分对象集合]]
一份报文主体内可含有多种类型的实体同时发送，每个部分之间用 boundary 字段定义的分隔符进行分隔，每个部分都可以有首部字段。
## [[ 虚拟主机]]

## [[通信数据转发]]
[[通信数据转发#1 代理]]
[[通信数据转发#2 网关]]
[[通信数据转发#3 隧道]]

## 六、[[HTTPS]]
HTTPS 并不是新协议，而是让 HTTP 先和 SSL（Secure Sockets Layer）通信，再由 SSL 和 TCP 通信，也就是说 HTTPS 使用了隧道进行通信。
通过使用 SSL，HTTPS 具有了加密（防窃听）、认证（防伪装）和完整性保护（防篡改）。

## 七、[[HTTP2.0]]

## 八、[[HTTP1.1 新特性]]
- 默认是长连接
- 支持流水线
- 支持同时打开多个 TCP 连接
- 支持虚拟主机
- 新增状态码 100
- 支持分块传输编码
- 新增缓存处理指令 max-age
## 九、[[GET和POST比较]]
[[GET和POST比较#作用]]
[[GET和POST比较#参数]]
[[GET和POST比较#安全]]
[[GET和POST比较#幂等性]]
[[GET和POST比较#可缓存]]
[[GET和POST比较#XMLHttpRequest]]
