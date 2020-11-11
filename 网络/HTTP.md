# 超文本传输协议

**Hyper Text Transfer Protocal**

# HTTP 0.9

​	诞生于 1991 年，主要用于学术交流，它的实现很简单，采用基于请求响应的模式，从客户端发出请求，服务器返回数据

## 1. 请求流程

	1. **TCP 三次握手**：HTTP 是基于 TCP 协议的，所以客户端先要根据 IP 地址、端口号和服务器建立 TCP 连接。
 	2. **请求**：发送一个 **GET** 请求行的信息，如 **GET /index.html** 用来获取 index.html
 	3. **响应**：服务器接收到请求后，读取对应的 HTML 文件，并将数据以 ASCII 字符流返回给客户端
 	4.  **TCP 四次挥手**：HTML 文档接收完毕后，断开连接

<img src="https://static001.geekbang.org/resource/image/db/34/db1166c68c22a45c9858e88a234f1a34.png" style="zoom:50%;" />

## 2. 特点

1. 只有请求行，没有 **请求头** 和 **请求体**，只需要一个请求行就足够表达诉求了
2. 没有 **响应头**，服务器并不需要告诉客户端太多信息，只需要返回数据就行了
3. 文件内容以 **ASCII** 字符流进行传输，毕竟只是 HTML 文件

# HTTP 1.0

​	1994年出现了拨号上网，网景又推出了一款浏览器，从此万维网就不再局限于学术交流了，而是进入了高速发展，随之而来的万维网联盟（W3C）和 HTTP工作组（HTTP-WG）的创建，他们致力于 HTML 的发展和 HTTP 的改进。

​	在浏览器中展示的不单是 HTML 文件了，还包括了 JavaScript、CSS、图片、音频、视频等不同类型的文件。因此支持多种类型的文件下载是 HTTP/1.0 的一个核心诉求，而且文件格式不仅仅局限于 ASCII 编码，还有很多其他类型编码的文件。

## 改进

1. 引入 **请求头** 和 **响应头**，它们都是以 `key-value` 的形式保存的，使 HTTP 支持传输多种不同类型的类型，为了实现这一目的，浏览器会在请求时在请求头加上以下数据，告诉服务器它期待返回什么样的文件

   1. 文件类型

   2. 文件的压缩的方法

   3. 文件的编码方式

   4. 指定语言（国际化支持）

      ```http
      GET /index.html HTTP/1.0
      accept: text/html
      accept-encoding: gzip, deflate, br
      accept-Charset: ISO-8859-1,utf-8
      accept-language: zh-CN,zh
      ```

      ```http
      HTTP/1.0 200 OK
      content-encoding: br
      content-type: text/html; charset=UTF-8
      ```

      > 服务器会根据 **请求头** 返回浏览器需要的文件，但是部分要求无法满足也是没有办法的，比如不支持 gzip 压缩，最终以 br 进行压缩返回
      >
      > 最终服务器会在 **响应头** 标明使用的压缩方式、文件类型及编码方式

2. 引入 **状态码**，有的请求服务器无法处理，或者处理出错 ，服务器就会通过将状态码存储在响应行告知浏览器。

3. 引入 **Cache 机制**，减轻服务器的压力，用于缓存已经下载过的数据

4. 加入 **user-Agent** 字段，帮助服务器了解客户端的基本信息

<img src="https://static001.geekbang.org/resource/image/b5/7d/b52b0d1a26ff2b8607c08e5c50ae687d.png" style="zoom:50%;" />

# HTTP 1.1 

​	HTTP 1.0 