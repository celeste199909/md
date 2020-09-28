# HTTP概述

http是一个client-server协议。
client-server架构也叫主从式架构，C/S架构。
另外还有P2P架构。（点对点网络）

http是应用层的协议。
OSI 开放式系统互联模型（Open System Interconnection Model）是一种概念模型，由国际标准化组织提出，一个试图使各种计算机在世界范围内互连为网络的标准框架。

![OSI模型](OSI模型.png)

请求 *request* 通过一个用户代理被发出。大多数情况下，这个用户代理都是指浏览器。

![request](HTTP_Request.png)

每一个发送到服务器的请求，都会被服务器处理并返回一个消息，也就是*response。*在这个请求与响应之间，还有许许多多的被称为 *proxies* 的实体。

![response](HTTP_Response.png)

在客户端与服务器能够交互之前，必须在这两者间建立一个[TCP]([https://zh.wikipedia.org/wiki/%E4%BC%A0%E8%BE%93%E6%8E%A7%E5%88%B6%E5%8D%8F%E8%AE%AE](https://zh.wikipedia.org/wiki/传输控制协议))链接，打开一个 TCP 连接需要多次往返（三次握手）交换消息。

- 参考

[HTTP概述-MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Overview)





