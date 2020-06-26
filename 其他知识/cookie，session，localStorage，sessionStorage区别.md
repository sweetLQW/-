# cookie，session，localStorage，sessionStorage

- #### cookie

  - 数据存储在客户端（一般是服务器向客户端发送cookie，之后客户端每次向同一服务器发送http请求时都会携带上）
  - 最多只能存4KB数据，很多浏览器都限制一个站点最多保存20个cookie
  - 不设置过期时间，关闭访问服务器的窗口cookie消失。设置了过期时间，直到过期时间之前都是有效的。（Cookie 可设置为长时间保持，比如我们经常使用的默认登录功能）
  - cookie不安全，考虑到安全应当使用session
  - cookie不可跨域，一级域名和二级域名之间允许共同使用(通过设置domain)
  - Cookie 只支持存字符串数据，想要设置其他类型的数据，需要将其转换成字符串
  - cookie属性
    - name=value
    - domain=域名(设置cookie所属域名，默认当前域名)
    - path(指定cookie在哪个路径下生效，默认"/")
    - maxAge(cookie 失效的时间，单位秒。如果为整数，则该 cookie 在 maxAge 秒后失效。如果为负数，该 cookie 为临时 cookie ，关闭浏览器即失效，浏览器也不会以任何形式保存该 cookie 。如果为 0，表示删除该 cookie 。默认为 -1。 - **比 expires 好用**。)
    - expires（过期时间，在设置的某个时间点后该 cookie 就会失效。 一般浏览器的 cookie 都是默认储存的，当关闭浏览器结束这个会话的时候，这个 cookie 也就会被删除）
    - secure（该 cookie 是否仅被使用安全协议传输。安全协议有 HTTPS，SSL等，在网络上传输数据之前先将数据加密。默认为false。 当 secure 值为 true 时，cookie 在 HTTP 中是无效，在 HTTPS 中才有效。）
    - httpOnly（如果给某个 cookie 设置了 httpOnly 属性，则无法通过 JS 脚本 读取到该 cookie 的信息，但还是能通过 Application 中手动修改 cookie，所以只是在一定程度上可以防止 XSS 攻击，不是绝对的安全）

- #### session

  - 数据大小：没有限制
  - Session 可以存任意数据类型
  - 存在服务器端
  - 用于存放敏感数据
  - 服务器会把长时间没有活动的session删除，一般为30分钟，具体根据服务器的设置；关闭访问服务器的窗口，session失效（Session 一般失效时间较短，客户端关闭（默认情况下）或者 Session 超时都会失效）

- #### localStorage

  - 本地存储
  - 跨会话存储（没有时间限制）
  - 只支持 string 类型的存储，存储number类型会自动转换为string类型

- #### sessionStorage

  - 本地存储
  - 会话存储（只在当前窗口有效）
  - 只支持 string 类型的存储，存储number类型会自动转换为string类型