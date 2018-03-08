## 一个完整的HTTP请求过程，通常有以下7个步骤：
1. **建立TCP连接**
2. **Web浏览器向Web服务器发送请求命令**
3. **Web浏览器发送请求头信息**
4. **Web服务器应答**
5. **Web服务器发送应答头信息**
6. **Web服务器向浏览器发送数据**
7. **Web服务器关闭TCP连接**

## 一个HTTP请求一般由四部分组成：
- **HTTP请求的方法或动作**：比如是GET还是POST请求
- **正在请求的URL**
- **请求头**：包含一些客户端环境信息，身份验证信息等
- **请求体**：也就是请求正文，请求正文中可以包含客户提交的查询字符串信息，表单信息等等(一般请求体和请求头之间有一段空行，表示请求头结束)

## 一个HTTP响应一般由三部分组成
- 一个数字和文字组成的**状态码**，用来显示请求是成功还是失败
- **响应头**：响应头也和请求头一样包含许多有用的信息，例如服务器类型、日期时间、内容类型和长度等
- **响应体**，也就是响应正文

## HTTP状态码由3位数字构成，其中首位数字定义了状态码的类型：
- **1XX** ：信息类，表示收到Web浏览器请求，正在进一步的处理中
- **2XX** ：成功，表示用户请求被正确接收，理解和处理，例如：200 OK
- **3XX** ：重定向，表示请求没有成功，客户必须采用进一步的动作
- **4XX** ：客户端错误，表示客户端提交的请求有错误，例如：404 NOT Found
- **5XX** ：服务器错误，表示服务器不能完成对请求的处理，如 500

### 常见响应
**100**  Continue  继续，一般在发送post请求时，已发送了http header之后服务端将返回此信息，表示确认，之后发送具体参数信息  
**200**  OK   正常返回信息  
**201**  Created  请求成功并且服务器创建了新的资源  
**202**  Accepted  服务器已接受请求，但尚未处理  
**301**  Moved Permanently  请求的网页已永久移动到新位置。  
**302**  Found  临时性重定向。  
**303**  See Other  临时性重定向，且总是使用 GET 请求新的 URI。  
**304**  Not Modified  自从上次请求后，请求的网页未修改过。  
**400**  Bad Request  服务器无法理解请求的格式，客户端不应当尝试再次使用相同的内容发起请求。  
**401**  Unauthorized  请求未授权。  
**403**  Forbidden  禁止访问。  
**404**  Not Found  找不到如何与 URI 相匹配的资源。  
**500**  Internal Server Error  最常见的服务器端错误。  
**503**  Service Unavailable 服务器端暂时无法处理请求（可能是过载或维护）。  

### 1xx（临时响应）
表示临时响应并需要请求者继续执行操作的状态代码。

**100**   （继续） 请求者应当继续提出请求。 服务器返回此代码表示已收到请求的第一部分，正在等待其余部分。  
**101**   （切换协议） 请求者已要求服务器切换协议，服务器已确认并准备切换。  

### 2xx （成功）
表示成功处理了请求的状态代码。

**200**   （成功）  服务器已成功处理了请求。 通常，这表示服务器提供了请求的网页。  
**201**   （已创建）  请求成功并且服务器创建了新的资源。  
**202**   （已接受）  服务器已接受请求，但尚未处理。  
**203**   （非授权信息）  服务器已成功处理了请求，但返回的信息可能来自另一来源。  
**204**   （无内容）  服务器成功处理了请求，但没有返回任何内容。  
**205**   （重置内容） 服务器成功处理了请求，但没有返回任何内容。  
**206**   （部分内容）  服务器成功处理了部分 GET 请求。  

### 3xx （重定向）
表示要完成请求，需要进一步操作。 通常，这些状态代码用来重定向。

**300**   （多种选择）  针对请求，服务器可执行多种操作。 服务器可根据请求者 (user agent) 选择一项操作，或提供操作列表供请求者选择。  
**301**   （永久移动）  请求的网页已永久移动到新位置。 服务器返回此响应（对 GET 或 HEAD 请求的响应）时，会自动将请求者转到新位置。  
**302**   （临时移动）  服务器目前从不同位置的网页响应请求，但请求者应继续使用原有位置来进行以后的请求。  
**303**   （查看其他位置） 请求者应当对不同的位置使用单独的 GET 请求来检索响应时，服务器返回此代码。  
**304**   （未修改） 自从上次请求后，请求的网页未修改过。 服务器返回此响应时，不会返回网页内容。  
**305**   （使用代理） 请求者只能使用代理访问请求的网页。 如果服务器返回此响应，还表示请求者应使用代理。  
**307**   （临时重定向）  服务器目前从不同位置的网页响应请求，但请求者应继续使用原有位置来进行以后的请求。  

### 4xx（请求错误）
这些状态代码表示请求可能出错，妨碍了服务器的处理。

**400**   （错误请求） 服务器不理解请求的语法。  
**401**   （未授权） 请求要求身份验证。 对于需要登录的网页，服务器可能返回此响应。  
**403**   （禁止） 服务器拒绝请求。  
**404**   （未找到） 服务器找不到请求的网页。  
**405**   （方法禁用） 禁用请求中指定的方法。  
**406**   （不接受） 无法使用请求的内容特性响应请求的网页。  
**407**   （需要代理授权） 此状态代码与 **401**（未授权）类似，但指定请求者应当授权使用代理。  
**408**   （请求超时）  服务器等候请求时发生超时。  
**409**   （冲突）  服务器在完成请求时发生冲突。 服务器必须在响应中包含有关冲突的信息。  
**410**   （已删除）  如果请求的资源已永久删除，服务器就会返回此响应。  
**411**   （需要有效长度） 服务器不接受不含有效内容长度标头字段的请求。  
**412**   （未满足前提条件） 服务器未满足请求者在请求中设置的其中一个前提条件。  
**413**   （请求实体过大） 服务器无法处理请求，因为请求实体过大，超出服务器的处理能力。  
**414**   （请求的 URI 过长） 请求的 URI（通常为网址）过长，服务器无法处理。  
**415**   （不支持的媒体类型） 请求的格式不受请求页面的支持。  
**416**   （请求范围不符合要求） 如果页面无法提供请求的范围，则服务器会返回此状态代码。  
**417**   （未满足期望值） 服务器未满足"期望"请求标头字段的要求。  

### 5xx（服务器错误）
这些状态代码表示服务器在尝试处理请求时发生内部错误。 这些错误可能是服务器本身的错误，而不是请求出错。

**500**   （服务器内部错误）  服务器遇到错误，无法完成请求。  
**501**   （尚未实施） 服务器不具备完成请求的功能。 例如，服务器无法识别请求方法时可能会返回此代码。  
**502**   （错误网关） 服务器作为网关或代理，从上游服务器收到无效响应。  
**503**   （服务不可用） 服务器目前无法使用（由于超载或停机维护）。 通常，这只是暂时状态。  
**504**   （网关超时）  服务器作为网关或代理，但是没有及时从上游服务器收到请求。  
**505**   （HTTP 版本不受支持） 服务器不支持请求中所用的 HTTP 协议版本。  

