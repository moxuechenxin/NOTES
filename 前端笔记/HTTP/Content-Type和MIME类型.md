## Content-Type
`Content-Type`实体头部用于指示资源的MIME类型 `media type`

在响应中，`Content-Type`标头告诉客户端实际返回的内容的内容类型。浏览器会在某些情况下进行MIME嗅探，并不一定遵循此标题的值; 为了防止这种行为，可以将标题 `X-Content-Type-Options` 设置为 `nosniff`

### 句法

```
Content-Type: text/html; charset=utf-8
Content-Type: multipart/form-data; boundary=something
```

### 指令
- **media-type**：资源或数据的 `MIME type`
- **charset**：字符编码标准
- **boundary**：对于多部分实体，`boundary` 是必需的，其包括来自一组字符的1到70个字符，已知通过电子邮件网关是非常健壮的，而不是以空白结尾。它用于封装消息的多个部分的边界

### 例子

**Content-Type 在HTML表单中**
在通过HTML form提交生成的POST请求中，请求头的`Content-Type`由``<form>``元素上的[`enctype`](http://www.w3school.com.cn/tags/att_form_enctype.asp)属性（默认值：`application/x-www-form-urlencoded`）指定

```html
<form action="/" method="post" enctype="multipart/form-data">
  <input type="text" name="description" value="some text">
  <input type="file" name="myFile">
  <button type="submit">Submit</button>
</form>
```
请求头看起来像这样（在这里省略了一些 headers）

```
POST /foo HTTP/1.1
Content-Length: 68137
Content-Type: multipart/form-data; boundary=---------------------------974767299852498929531610575
Content-Disposition: form-data; name="description"
---------------------------974767299852498929531610575

some text

---------------------------974767299852498929531610575
Content-Disposition: form-data; name="myFile"; filename="foo.txt"
Content-Type: text/plain

(content of the uploaded file foo.txt)

---------------------------974767299852498929531610575
```

## MIME type
`MIME类型`是一种通知客户端其接收文件的多样性的机制:文件后缀名在网页上并没有明确的意义。因此，使服务器设置正确的传输类型非常重要，所以正确的MIME类型与每个文件一同传输给服务器。在网络资源进行连接时，浏览器经常使用MIME类型来决定执行何种默认行为。

### 语法
**通用结构**
```
type/subtype
```
- **type**：表示可以被分为复数子类的独立类型
- **subtype**：表示细分后的每个类型
`MIME类型`对大小写不敏感，但是传统写法都是小写

#### 独立类型
型	| 描述 | 典型示例
---|---|---
text | 表明文件是普通文本，理论上是可读的语言 | `text/plain`, `text/html`, `text/css`, `text/javascript`
image | 表明是某种图像。不包括视频，但是动态图（比如动态gif）也使用image类型 | `image/gif`, `image/png`, `image/jpeg`, `image/bmp`, `image/webp`, `image/svg+xml`, `image/x-icon`
audio | 表明是某种音频文件	| `audio/midi`, `audio/mpeg`, `audio/webm`, `audio/ogg`, `audio/wav`
video	| 表明是某种视频文件	| `video/webm`, `video/ogg`
application	| 表明是某种二进制数据 | `application/octet-stream`, `application/pkcs12`, `application/vnd.mspowerpoint`, `application/xhtml+xml`, `application/xml`,  `application/pdf`

注：对于text文件类型若没有特定的subtype，就使用 `text/plain`。类似的，二进制文件没有特定或已知的 subtype，即使用 `application/octet-stream`

#### Multipart types

```
multipart/form-data
multipart/byteranges
```
`multipart/form-data`： 可用于HTML表单从浏览器发送信息给服务器。 作为多部分文档格式，它由边界线（一个由'--'开始的字符串）划分出的不同部分组成。每一部分有自己的实体，以及自己的 HTTP 请求头，`Content-Disposition`和 `Content-Type` 用于文件上传领域，最常用的 (`Content-Length` 因为边界线作为分隔符而被忽略）  

`multipart/byteranges`： 用于把部分的响应报文发送回浏览器。当发送状态码 `206 Partial Content` 时，这个MIME类型用于指出这个文件由若干部分组成，每一个都有其请求范围。就像其他很多类型`Content-Type`使用分隔符来制定分界线。每一个不同的部分都有`Content-Type`这样的HTTP头来说明文件的实际类型，以及`Content-Range`来说明其范围

**参考**：
- [MDN：Content-Type](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Content-Type)
- [MDN：MIME 类型](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Basics_of_HTTP/MIME_types)
