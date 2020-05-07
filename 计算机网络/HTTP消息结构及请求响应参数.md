# HTTP消息结构及请求响应参数

### 请求消息：请求行、请求头、请求数据

### 响应消息：状态行、消息报头、空行、相应正文



### Request请求头一般包括：

##### 请求方式

GET：使用GET的请求一般只用于获取数据.
HEAD：HEAD方法请求一个与GET请求的响应相同的响应，但没有响应体，用于获取报头。
POST：POST方法用于将实体提交到指定的资源，一般用于新增数据。 
DELETE：DELETE方法删除指定的资源。
PUT：一般用来替换资源，无论执行多少次结果是一样的。
PATCH：一般用于更新部分资源，如果重复提交，程序可能执行多次。
CONNECT：用于与代理服务器建立连接，CONNECT方法建立一个到由目标资源标识的服务器的隧道。
OPTIONS：OPTIONS方法用于描述目标资源的通信选项。用于获取当前URL所支持的方法。若请求成功，则它会在HTTP头中包含一个名为“Allow”的头，值是所支持的方法，如“GET, POST”。
TRACE：回显服务器收到的请求，主要用于测试或诊断。

##### Host：当前客户端发出请求的域名

##### Connection：表示是否需要持久连接

Close：完成本次请求的响应后，断开连接。
Keepalive：完成本次请求的响应后，保持连接。
Keep-Alive：如果浏览器请求保持连接，则该头部表明希望 WEB 服务器保持连接多长时间（秒），如Keep-Alive：300。

##### Accept：代表客户端希望接受的MIME类型，例如text/html等

##### Accept-Encoding：

accept-encoding表示你发送请求时告诉服务器，我可以解压这些格式的数据，网页会根据你发送的accept-encoding来决定用什么格式(content-encoding)传给你。

##### Accept-Language：表示浏览器所支持的语言类型

##### User-Agent：包含了一个特征字符串，用来让接收端来识别请求端的软件的类型、操作系统、软件开发商以及版本号等

##### Referer：用来识别访问来源，当前请求页面的来源页面的地址

##### Cookie：含有先前由服务器通过 [`Set-Cookie`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Set-Cookie) 首部投放并存储到客户端的 [HTTP cookies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies)

##### Upgrade-Insecure-Requests：用来向服务器端发送信号，表示客户端优先选择加密及带有身份验证的响应，并且它可以成功处理 [`upgrade-insecure-requests`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Content-Security-Policy/upgrade-insecure-requests) [CSP](https://developer.mozilla.org/en-US/docs/Web/Security/CSP) 指令。

##### If-Modified-Since: Mon, 18 Jul 2016 02:36:04 GMT

只可以用在 [`GET`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/GET) 或 [`HEAD`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/HEAD) 请求中，服务器只在所请求的资源在给定的日期时间之后对内容进行过修改的情况下才会将资源返回，状态码为 [`200`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/200) 。如果请求的资源从那时起未经修改，那么返回一个不带有消息主体的 [`304`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/304) 响应，而在 [`Last-Modified`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Last-Modified) 首部中会带有上次修改时间。当与 [`If-None-Match`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/If-None-Match) 一同出现时，它（**`If-Modified-Since`**）会被忽略掉，除非服务器不支持 `If-None-Match`。

##### If-None-Match: "c561c68d0ba92bbeb8b0fff2a9199f722e3a621a"

对于 GET[`GET`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/GET) 和 [`HEAD`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/HEAD) 请求方法来说，当且仅当服务器上没有任何资源的 [`ETag`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/ETag) 属性值与这个首部中列出的相匹配的时候，服务器端会才返回所请求的资源，响应码为 [`200`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/200) 。对于其他方法来说，当且仅当最终确认没有已存在的资源的 [`ETag`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/ETag) 属性值与这个首部中所列出的相匹配的时候，才会对请求进行相应的处理。

对于 [`GET`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/GET) 和 [`HEAD`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/HEAD) 方法来说，当验证失败的时候，服务器端必须返回响应码 304 （Not Modified，未改变）。对于能够引发服务器状态改变的方法，则返回 412 （Precondition Failed，前置条件失败）。需要注意的是，服务器端在生成状态码为 304 的响应的时候，必须同时生成以下会存在于对应的 200 响应中的首部：Cache-Control、Content-Location、Date、ETag、Expires 和 Vary 。

##### Cache-Control：通过指定指令来实现缓存机制

指定请求和响应遵循的缓存机制，值一般包括no-cache（指定 `no-cache` 或 `max-age=0` 表示客户端可以缓存资源，每次使用缓存资源前都必须重新验证其有效性。这意味着每次都会发起 HTTP 请求，但当缓存内容仍有效时可以跳过 HTTP 响应体的下载。），max-age（设置缓存存储的最大周期，超过这个时间缓存被认为过期(单位秒)。与`Expires`相反，时间是相对于请求的时间。）等。



### Response消息报头一般包括：

**Allow**：服务器支持哪些请求方法（如GET、POST等）

**Content-Encoding**：文档的编码（Encode）方法。只有在解码之后才可以得到Content-Type头指定的内容类型

**Content-Length**：表示内容长度。只有当浏览器使用持久HTTP连接时才需要这个数据。

**Content-Type**： 表示后面的文档属于什么MIME类型

**Date**：当前的GMT时间

 **Expires**：指明应该在什么时候认为文档已经过期，从而不再缓存它。

**Last-Modified**：文档的最后改动时间

