# HTTP 协议

## 介绍

HTTP（hypertext transport protocol）协议也叫==超文本传输协议==，是一种基于 TCP/IP 的应用层通信协议，这个协议详细规定了浏览器和万维网服务器之间互相通信的规则。

协议主要规定了两方面的内容

* 客户端向服务器发送数据，称之为==请求报文==
* 服务器向客户端返回数据，称之为==响应报文==

## Fiddle 工具

Fiddler 是一个 http 协议调试代理工具，使用它我们可以抓取网页的所有请求与响应，也就是咱们俗称的抓包。

![1573826028707](F:%5C%E7%AC%94%E8%AE%B0%5CTypora%E7%AC%94%E8%AE%B0%5CHTTP%20%E5%8D%8F%E8%AE%AE.assets%5C1573826028707.png)

进入软件之后-> tools -> options -> https -> 勾选左侧两个选项框 -> 弹出的窗口中 点击 『是』

## 内容

### 请求

HTTP 请求报文包括四部分

* 请求行
* 请求头
* 空行
* 请求体

```http
GET http://localhost:3000/index.html?username=sunwukong&password=123123 HTTP/1.1
Host: localhost:3000
Connection: keep-alive
Pragma: no-cache
Cache-Control: no-cache
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/64.0.3282.140 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9


```

*  GET http://localhost:3000/hello.html HTTP/1.1：GET请求，请求服务器路径为http://localhost:3000/hello.html，?后面跟着的是请求参数（查询字符串），协议是HTTP 1.1版本

*  Host: localhost:3000：请求的主机名为localhost，端口号3000

*  Connection: keep-alive：处理完这次请求后继续保持连接，默认为3000ms

*  Pragma: no-cache：不缓存该资源，http 1.0的规定

*  Cache-Control: no-cache： 不缓存该资源 http 1.1的规定，优先级更高

*  Upgrade-Insecure-Requests: 1：告诉服务器，支持发请求的时候不用 http 而用 https

*  User-Agent: Mozilla/5.0 (...：与浏览器和OS相关的信息。有些网站会显示用户的系统版本和浏览器版本信息，这都是通过获取User-Agent头信息而来的

*  Accept: text/html,...：告诉服务器，当前客户端可以接收的文档类型。q 相当于描述了客户端对于某种媒体类型的喜好系数，该值的范围是 0-1。默认为1

*  Accept-Encoding: gzip, deflate, br：支持的压缩格式。数据在网络上传递时，服务器会把数据压缩后再发送

*  Accept-Language: zh-CN,zh;q=0.9：当前客户端支持的语言，可以在浏览器的工具选项中找到语言相关信息

### 响应

HTTP 响应报文也包括四个部分

- 响应行
- 响应头
- 空行
- 响应体

```http
HTTP/1.1 200 OK
X-Powered-By: Express
Accept-Ranges: bytes
Cache-Control: public, max-age=0
Last-Modified: Wed, 21 Mar 2018 13:13:13 GMT
ETag: W/"a9-16248b12b64"
Content-Type: text/html; charset=UTF-8
Content-Length: 169
Date: Thu, 22 Mar 2018 12:58:41 GMT
Connection: keep-alive

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>首页</title>
</head>
<body>
  <h1>网站首页</h1>
</body>
</html>
```

*  HTTP/1.1 200 OK：协议是HTTP 1.1版本，请求响应成功

*  X-Powered-By: Express：自定义的头，表示用的框架，一般不返回容易造成安全漏洞。

*  Accept-Ranges: bytes：告诉浏览器支持多线程下载

*  Cache-Control: public, max-age=0：强制对所有静态资产进行缓存，即使它通常不可缓存。max-age指定多久缓存一次

*  Last-Modified: Wed, 21 Mar 2018 13:13:13 GMT：这个资源最后一次被修改的日期和时间

*  ETag: W/"a9-16248b12b64"：请求资源的标记/ID

*  Content-Type: text/html; charset=UTF-8：返回响应体资源类型

*  Content-Length: 169：响应体的长度

*  Date: Thu, 22 Mar 2018 12:58:41 GMT：提供了日期的时间标志，标明响应报文是什么时间创建的

### WEB 服务

使用 nodejs 创建 HTTP 服务器

```js
//1. 引入 http 内置模块
var http = require('http');

//2. 创建服务
var server = http.createServer(function(request, response){
    response.end('hello world!!');
});

//3. 监听端口
server.listen(8000);
```

## 使用nodeJs 搭建服务

```js
//1.引入HTTP 模块
const http = require('http');
//2.调用函数返回一个服务对象
//  request  是对请求报文的封装对象,通过 request 对象可以获取到请求报文中的内容
//  response 是对响应报文的封装对象,通过 response 对象可以设置 http 相应报文
//  每一个 HTTP 请求到来之后,都是由回调函数执行处理请求,并设置响应
const server = http.creatServer(function(request,response){
    //简单的一个响应
    //end  方法是用来设置响应体的
    response.end('Hello HTTP Server');
})
//3.监听一个端口 启动服务  回调函数会在服务启动成功之后运行
//端口号 = 计算机的服务窗口号,一台计算机有65536 个服务窗口号
//前端常用端口号   3000 9000 8000 8080
server.listen(端口号,function(){
    console.log('服务已启动')
})
```



* request 是对请求报文的封装对象
* response 是对响应的封装对象

#### 获取请求

```js
// 引入 url 模块
const url = require('url');
//获取请求的方法(通过什么方法发出的请求)  method 方法
console.log(request.method);

//获取http版本  version 版本
console.log(request.httpVersion);

//获取请求路径   这里获取的是url的 [路径和查询字符串部分]
console.log(request.url);
//获取url中查询字符串内容  首先要引入url模块
const url = require('url');
//接收请求url解析对象   parse(解析)方法 用来解析查询字符串,返回值为一个对象
const data = url.parse(requist.url,true)'
//查询字符串在  解析对象的query属性中储存.query也是一个对象,查询字符串作为query的属性存储,键值对形式    query (查询)
console.log(data.query.属性名);
//只获取路径部分   pathname (路径名)
console.log(data.pathname);

//获取请求头 hearder(头部)
console.log(request.headers);

//获取请求体
request.on('data', function(chunk){})
request.on('end', function(){});
```





#### 设置响应

```js
//设置状态码  status(状态)  code(编码);   statusCode(状态码)
response.statusCode = 200;

//设置响应头  (响应头一般用于写 解析规则的 )
response.setHeader('content-type','text/html;charset-utf-8');

//设置响应体
response.write('body');

//结束
response.end();

```

**当相应体为一个完整的页面时,需要把页面单独写到一个html文件中,然后在服务js文件中对,对应的相应页面进行读取并写入;**

**注意:在html中所引入的外部文件,需要单独设置响应报文**

### 响应外部文件

```js
//1.如果外部文件体积小,使用同步读取文件,然后设置给响应体
response(fs.readFileSync('路径'));
// response.end(fs.readFile('./js/jsjs/login.js'));

//2.如果外部文件比较大,则使用读取流方式读取文件在设置给响应体
response.setHeader('Content-Type','text/js;charset=utf-8');
const rs = fs.createReadStream('./js/jsjs/login.js');
rs.on('data',chunk=>{
    response.end(chunk);
})
```



### GET 和 POST 的区别

GET 和 POST 是 HTTP 协议请求的两种方式

* GET 主要用来获取数据, POST 主要用来提交数据
* GET 带参数请求是将参数缀到 URL 之后, 在地址栏输入url访问网站就是 GET 请求, POST 带参数请求是将参数放到请求体中
* POST 请求相对 GET 安全一些, 因为在浏览器中参数会暴露在地址栏.
* GET 请求大小有限制, 一般为 2k, 而 POST 请求则没有大小限制
* GET 类型报文请求方法的位置为 GET , POST 类型报文请求方法为 POST

### Chrome 查看请求报文

![网络控制台查看报文的方式](F:%5C%E7%AC%94%E8%AE%B0%5CTypora%E7%AC%94%E8%AE%B0%5CHTTP%20%E5%8D%8F%E8%AE%AE.assets%5C%E7%BD%91%E7%BB%9C%E6%8E%A7%E5%88%B6%E5%8F%B0%E6%9F%A5%E7%9C%8B%E6%8A%A5%E6%96%87%E7%9A%84%E6%96%B9%E5%BC%8F.png)



## 附录

### 响应状态码

响应状态码是服务器对结果的标识，常见的状态码有以下几种：

- 200：请求成功，浏览器会把响应体内容（通常是html）显示在浏览器中；
- 301：重定向，被请求的旧资源永久移除了（不可以访问了），将会跳转到一个新资源，搜索引擎在抓取新内容的同时也将旧的网址替换为重定向之后的网址；
- 302：重定向，被请求的旧资源还在（仍然可以访问），但会临时跳转到一个新资源，搜索引擎会抓取新的内容而保存旧的网址；
- 304：（Not Modified）请求资源未被修改，浏览器将会读取缓存；
- 403：forbidden 禁止的
- 404：请求的资源没有找到，说明客户端错误的请求了不存在的资源；
- 500：请求资源找到了，但服务器内部出现了错误；


### Sec-Fetch-* 请求头

<https://www.w3.org/TR/fetch-metadata/#sec-fetch-mode-header>

