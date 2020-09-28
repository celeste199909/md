# HTTP缓存

1. 是么是缓存？为什么要使用缓存？

- 缓存的分类：私有与共享缓存。

1. 什么是私有缓存？
2. 什么是共享缓存？

这次先了解浏览器（私有）与代理缓存（共享），除此之外还有网关缓存、CDN、反向代理缓存和负载均衡器等部署在服务器上的缓存方式

- 缓存控制

*Cache-Control头*

HTTP/1.1定义的 Cache-Control 头用来区分对缓存机制的支持情况， 请求头和响应头都支持这个属性。通过它提供的不同的值来定义缓存策略。

1. 没有缓存

```
Cache-Control: no-store   
```

这样设置意味着缓存中将不存储任何内容。每次客户端请求都会下载完整的响应内容。

2. 缓存但重新验证

```
Cache-Control: no-cache
```

这样意味着，每次发送请求的时候会向服务器先进行验证，如果缓存没过期（304）则使用缓存。

3. 私有和公共缓存

```
Cache-Control: pravite
Cache-Control: public
```

当设置public时，表示该响应可以被任何中间人（中间代理、CDN）缓存。
pravite（默认），中间人不能缓存，只应用于浏览器私有缓存。

4. 过期

```
Cache-Control: max-age=<seconds>
```

设置过期时间

5. 验证方式

```
Cache-Control: must-revalidate
```

在使用一个*陈旧的*资源之前必须进行验证。

- 缓存驱逐

由于存储空间有限，所以缓存会定期清除一些副本。
缓存副本不会被直接清除，而是先发请求向服务器验证是否过期（新鲜），返回304说明是新鲜的。

- 参考：

[HTTP缓存-MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Caching_FAQ#Cache_validation)