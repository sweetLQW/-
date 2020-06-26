# HTTP消息结构及请求响应参数

## 请求报文格式

```
请求行 -> 请求方法 URL 协议版本
请求头 -> key:value
				 key:value
				 ……
空行
请求主体
```

### 请求头包括

- User-Agent: 浏览器信息

- Accept：指定客户端能够接收的内容类型

- Accept-Charset：浏览器可以接受的字符编码集。

- Accept-Encoding：指定浏览器可以支持的web服务器返回内容压缩编码类型。

- Accept-Language：浏览器可接受的语言

- Accept-Ranges

- Authorization：HTTP授权的授权证书

- Cache-Control：指定请求和响应遵循的缓存机制

  ```
  no-cache
  ```

  获取资源之前，把请求提交给原始服务器进行协商缓存验证，如果有协商缓存，则返回304，反之则请求数据。

  ```
  no-store
  ```

  缓存不应存储有关客户端请求或服务器响应的任何内容，即不使用任何缓存。

  ```
  max-age=<seconds>
  ```

  设置缓存存储的最大周期，超过这个时间缓存被认为过期(单位秒)。与`Expires`相反，时间是相对于请求的时间，如果max-age=0，则相当于设置了no-cache。

  ```
  max-stale[=<seconds>]
  ```

  表明客户端愿意接收一个已经过期的资源。可以设置一个可选的秒数，表示响应不能已经过时超过该给定的时间。

  ```
  min-fresh=<seconds>
  ```

  表示客户端希望获取一个能在指定的秒数内保持其最新状态的响应。

  ```
  no-transform
  ```

  不得对资源进行转换或转变。`Content-Encoding`、`Content-Range`、`Content-Type`等HTTP头不能由代理修改。例如，非透明代理或者如[Google's Light Mode](https://support.google.com/webmasters/answer/6211428?hl=en)可能对图像格式进行转换，以便节省缓存空间或者减少缓慢链路上的流量。`no-transform`指令不允许这样做。

  ```
  only-if-cached
  ```

  表明客户端只接受已缓存的响应，并且不要向原始服务器检查是否有更新的拷贝。

- Connection：是否持久连接（HTTP 1.1默认进行持久连接）keep-alive/close

- Cookie：HTTP请求发送时，会把保存在该请求域名下的所有cookie值一起发送给web服务器。

- Content-Length：请求的内容长度

- Content-Type：请求的与实体对应的MIME信息

- Date	请求发送的日期和时间

- Expect	请求的特定的服务器行为（100-continue）

- From	发出请求的用户的Email

- Host	指定请求的服务器的域名和端口号

- If-Match	只有请求内容与实体相匹配才有效

- If-Modified-Since	如果请求的部分在指定时间之后被修改则请求成功，未被修改则返回304代码，绝对时间

- If-None-Match	如果内容未改变返回304代码，参数为服务器先前发送的Etag

- If-Range	如果实体未改变，服务器发送客户端丢失的部分，否则发送整个实体。参数也为Etag

- Proxy-Authorization	连接到代理的授权证书

- Range	只请求实体的一部分，指定范围	Range: bytes=500-999

- Referer	先前网页的地址，当前请求网页紧随其后,即来路

## 响应报文格式

```
状态行 -> 协议版本 状态码 状态码描述
响应头 -> key:value
				 key:value
				 ……
空行
响应包体
```

### 响应头包括

- Content-Type：告诉客户端实际返回的内容的内容类型。浏览器会在某些情况下进行MIME查找，并不一定遵循此标题的值;

  - text/html ： HTML格式
  - text/plain ：纯文本格式
  - text/xml ： XML格式
  - image/gif ：gif图片格式
  - image/jpeg ：jpg图片格式
  - image/png：png图片格式
  - application/xhtml+xml ：XHTML格式
  - application/xml： XML数据格式
  - application/atom+xml ：Atom XML聚合格式
  - application/json： JSON数据格式
  - application/pdf：pdf格式
  - application/msword ： Word文档格式
  - application/octet-stream ： 二进制流数据（如常见的文件下载）
  - application/x-www-form-urlencoded ： <form encType="">中默认的encType，form表单数据被编码为key/value格式发送到服务器（表单默认的提交数据的格式）
  - multipart/form-data ： 需要在表单中进行文件上传时，就需要使用该格式

- Allow：服务器支持哪些请求方法

- Content-Encoding：文档的编码（Encode）方法

- Content-Length：表示内容长度。只有当浏览器使用持久HTTP连接时才需要这个数据

- Cache-Control：指定请求和响应遵循的缓存机制

  ```
  public
  ```

  表明响应可以被任何对象（包括：发送请求的客户端，代理服务器，等等）缓存，即使是通常不可缓存的内容（例如：1.该响应没有`max-age`指令或`Expires`消息头；2. 该响应对应的请求方法是 [POST](https://wiki.developer.mozilla.org/en-US/docs/Web/HTTP/Methods/POST) 。）

  ```
  private
  ```

  表明响应只能被单个用户缓存，不能作为共享缓存（即代理服务器不能缓存它）。私有缓存可以缓存响应内容，比如：对应用户的本地浏览器。

  ```
  no-cache
  ```

  获取资源之前，把请求提交给原始服务器进行协商缓存验证，如果有协商缓存，则返回304，反之则请求数据。

  ```
  no-store
  ```

  缓存不应存储有关客户端请求或服务器响应的任何内容，即不使用任何缓存。

  ```
  max-age=<seconds>
  ```

  设置缓存存储的最大周期，超过这个时间缓存被认为过期(单位秒)。与`Expires`相反，时间是相对于请求的时间，如果max-age=0，则相当于设置了no-cache。

  ```
  s-maxage=<seconds>
  ```

  覆盖`max-age`或者`Expires`头，但是仅适用于共享缓存(比如各个代理)，私有缓存会忽略它。

  ```
  no-transform
  ```

  不得对资源进行转换或转变。`Content-Encoding`、`Content-Range`、`Content-Type`等HTTP头不能由代理修改。例如，非透明代理或者如[Google's Light Mode](https://support.google.com/webmasters/answer/6211428?hl=en)可能对图像格式进行转换，以便节省缓存空间或者减少缓慢链路上的流量。`no-transform`指令不允许这样做。

- Date：当前的GMT时间。

- Expires：缓存到期时间，绝对时间。

- Last-Modified：文档的最后改动时间，绝对时间。

- Location：表示客户应当到哪里去提取文档。

- Refresh：表示浏览器应该在多少时间之后刷新文档，以秒计。

- Set-Cookie：设置和页面关联的Cookie。

- WWW-Authenticate：客户应该在Authorization头中提供什么类型的授权信息