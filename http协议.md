---
title: http协议
image: /image/4.jpg  #设置本地图片
---

# http协议(超文本传输协议)
**作用**：用于从万维网服务器传输超文本到本地浏览器的传送协议

**定义**:http协议是基于TCP的应用层协议，它不关心数据传输的细节，主要是用来规定客户端和服务端的数据传输格式，最初是用来向客户端传输HTML页面的内容，默认端口是80

**特点**:http是基于请求与响应模式的、无状态的、应用层的协议

具体聊http协议之前先了解下面四层:

1.应用层:规定了向用户提供应用服务时的通信协议，http、DNS、FTP

2.传输层:提供处于网络连接中两台计算机之间的数据传输所使用的协议,TCP、UDP

TCP:全双工的，即发送和接收数据可以同时进行，在建立和断开连接时会有三次或者四次握手，所以传输比较稳定，但是没有UDP那么高效

UDP:面向无连接，在正式传递数据之前不需要建立连接，UDP协议不保证有序且不丢失地传递到对端，所以不稳定，但是高效和轻便

3.网络层:规定了数据通过怎样的传输路线到达对方计算机传送给对方,IP 

4.链路层:网络硬件（包括操作系统的）

web应用的通信传输流：
发送端应用层->发送端传输层->发送端网络层->发送端链路层->接收端链路层->接收端网络层->接收端传输层->接收端应用层

## http分为两块
### 一、请求：
1.请求行(URL地址)

URL: http://www.baidu.com/s?wd=柠檬班

scheme:协议，如http、https、ftp

host:域名或者IP地址

port:端口

path:资源路径

query-string:发送参数:/s?wd=柠檬班

2.请求头（重点）,附加的需要服务器了解的一些信息

补充：key:value的形式（键值对）

**host:主机IP地址或域名**

**User-Agent：客户端相关的信息**

**Accept:指定客户端接受信息类型,如image/jpg,text/html,application/json**

**Accept-Encoding:可接收的内容编码,如gzip**

**Accept-Language: 接收的语言,如zh-cn**

**Authorization：客户端提供给服务端，进行权限认证的信息,bearer xxxx,一般存token用**

**Cookie:携带的cookie信息**

**Referer:当前文档的URL，即从哪个链接过来的**

**Connection:如Keep-Alive,表示保持tcp连接不关闭,**

**Content——Type:请求体内容类型,如application/x-www-form-urlencoded**

**Cache-Control：缓存机制，如Cache-Control：no-cache(强制向服务器再次验证)，no-store(不做任何缓存),max-age=11111(资源可缓存的最大时间/s),public（客户端、代理服务器都可利用缓存）,private(代理服务器不可用缓存)**

Content-Length:数据长度

Accept-Charset：客户端接收的字符集，如gb2312\iso-8859-1

3.请求体(get请求没有请求体)

### 二、响应:
1.响应行

http版本和状态码

1xx：提示信息，请求被成功接收

2xx：成功，请求被成功处理

 200：ok,表示从客户端发来的请求在服务器端被正确处理\
 
 204：No content,表示请求成功，但是响应报文不含实体的主体部分
 
 206:partial Content，表示进行范围请求成功
 
3xx: 重定向相关

 301:moved parmanently，永久性重定向，表示资源已被分配了新的URL
 
 302：found，临时性重定向，表示资源临时被分配了新的URL
 
 303：see other，表示资源存在着另一个URL，应该使用GET方法获取资源
 
 304:not modified,表示服务器允许访问资源，但请求未满足条件的情况(与重定向无关)
 
 307:temporary redirect,临时重定向，和302含义类似，但是期望客户端保持请求方法不变，向新的地址发出请求
 
4xx: 客户端错误

 400: bad request,请求报文存在语法错误
 
 401 unauthorized,表示发送的请求需要通过HTTP认证的认证信息
 
 402: forbidden,表示对请求资源的访问被服务器拒绝，可在实体主体部分返回原因描述
 
 403: not found,表示在服务器上没有找到请求的资源
 
5xx: 服务端的错误 

 500:internal sever error,表示服务端在执行请求时发生了错误
 
 501:not Implemented,表示服务器不支持当前请求所需要的某个功能
 
 503:service unavaliable,表明服务器暂时处于超负载或正在停机维护，无法处理请求
 
2.响应头

Server：HTTP服务器的软件信息

Date: 响应报文的时间

**Expires:指定缓存过期时间**

**Set-Cookie：设置cookie,请求回来之后保存**

Last-Modified: 资源最后修改时间

**Content-Type:响应的类型和字符集,如 text/html；charset：utf-8**

Content-Length:内容长度

**Connection:如Keep-Alive,表示保持tcp连接不关闭,**

**Location:指明重定向的位置，新的URL地址,如304情况**

Pragma:防止页面被缓存，和Cache-Control：no-cache作用一样

3.响应体
