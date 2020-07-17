## NSURLRequestCachePolicy

```objective-c
typedef NS_ENUM(NSUInteger, NSURLRequestCachePolicy)
{
    NSURLRequestUseProtocolCachePolicy = 0,
    NSURLRequestReloadIgnoringLocalCacheData = 1,
    NSURLRequestReloadIgnoringLocalAndRemoteCacheData = 4,
    NSURLRequestReloadIgnoringCacheData = NSURLRequestReloadIgnoringLocalCacheData,
    NSURLRequestReturnCacheDataElseLoad = 2,
    NSURLRequestReturnCacheDataDontLoad = 3,
    NSURLRequestReloadRevalidatingCacheData = 5,
};
```

- **NSURLRequestUseProtocolCachePolicy**
  默认的缓存策略，其行为是由协议指定的针对该协议最好的实现方式。其简单流程如下：

  1.如果请求的缓存响应不存在，则URL加载系统直接从源端加载数据；

  2.否则，如果缓存响应中没有明确表示每次请求必须重新验证，则如果不是响应的缓存过期了，则URL加载系统会返回缓存数据

  3.如果缓存的响应过期或者需要重新验证，URL加载系统发送`HEAD`请求到源端，查看资源是否发生了变化。如果变化了，则URL加载系统取出从始发源的数据。否则，它返回缓存的响应。

- **NSURLRequestReloadIgnoringCacheData**
   从服务端加载数据，完全忽略缓存。

- **NSURLRequestReturnCacheDataElseLoad**
   使用缓存数据，忽略其过期时间；只有在没有缓存版本的时候才从源端加载数据。

- **NSURLRequestReturnCacheDataDontLoad**

   只使用cache数据，如果不存在cache，请求失败；用于没有建立网络连接离线模式

  

## HTTP头部字段对应的缓存处理

对于缓存的响应过期或者需要重新验证的情况，可以通过HTTP中请求和响应头来判断

- Cache-Control
  在第一次请求到服务器资源的时候，服务器需要使用Cache-Control这个响应头来指定缓存策略，它的格式如下：Cache-Control:max-age=xxxx，这个头指指明缓存过期的时间
   `Cache-Control`头具有如下选项:

| 常量              | 意义                                                 |
| ----------------- | ---------------------------------------------------- |
| public            | 指示响应可被任何缓存区缓存                           |
| private           | 内容只缓存到私有缓存中(仅客户端可以缓存)             |
| no-cache          | 指示请求或响应消息不能缓存                           |
| no-store          | 所有内容都不会被缓存到缓存或 Internet 临时文件中     |
| must-revalidation | 如果缓存的内容失效，请求必须发送到服务器进行重新验证 |
| max-age           | 可以接收生存期不大于指定时间（以秒为单位）的响应     |
| min-fresh         | 可以接收响应时间小于当前时间加上指定时间的响应       |
| max-stale         | 可以接收超出超时期间的响应消息                       |

- **Expires**
  `Expires`表示存在时间，允许客户端在这个时间之前不去检查（发请求），等同max-age的效果。但是如果同时存在，则被Cache-Control的max-age覆盖。
   格式：Expires = "Expires" ":" HTTP-date"
   例如：Expires: Thu, 01 Dec 1994 16:00:00 GMT （必须是GMT格式）
- **Last-Modified/If-Modified-Since**
   `Last-Modified`是由服务器返回响应头，标识资源的最后修改时间.
   `If-Modified-Since` 则由客户端发送，标识客户端所记录的，资源的最后修改时间。服务器接收到带有该请求头的请求时，会使用该时间与资源的最后修改时间进行对比，如果发现资源未被修改过，则直接返回HTTP 304而不返回包体，告诉客户端直接使用本地的缓存。否则响应完整的消息内容。
- **Etag/If-None-Match**
   `Etag` 由服务器发送，告之当资源在服务器上的一个唯一标识符。
   客户端请求时，如果发现资源过期(使用Cache-Control的max-age)，发现资源具有Etag声明，这时请求服务器时则带上If-None-Match头，服务器收到后则与资源的标识进行对比，决定返回200或者304