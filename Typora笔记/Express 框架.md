

# Express 框架

就是一个运行在nodejs中用来搭建服务器的模块

**安装**

npm i express --save  安装express并添加到依赖项



## HTTP

请求报文

```
POST  /admin?id=100&wd=ddd  HTTP/1.1
Host: localhost
Accept:
Accept-language:
Accept-encoding:
Cookie:

a=100&b=200&c=300
{"a":100, "b": 200}
```

响应报文
304  403  404  500

```
HTTP/1.1  200  OK
Content-Type: text/html;charset=utf-8
Connection:
Set-Cookie:
Content-Length:

HTML
CSS
JS
图片
字体
JSON
```

## express 官网

https://www.expressjs.com.cn/

## espress 初体验

```js
//express 框架的使用步骤
//1. 命令行安装  npm i express
//2. 脚本中  引入 express 包
const express = require('express');

//3. 创建服务对象
const app = express();

//4. 创建路由规则
app.get('/home', (request, response) => {
    response.end('ok');
});

//5. 监听端口 启动服务
app.listen(80, () => {
    console.log('服务已经启动, 80 端口监听中.....');
});
```



## vscode 切换命令行工具

1. 点击命令行的下拉框
2. 点击下拉框中的『选择默认shell』
3. 在vscode上方出现新的选择项
4. 选择 『cmd』
5. 新建一个命令行窗口, 就会发现类型已经发生了变化

> 若发现全局安装的工具, 无法运行, 则可以切换命令行的类型

## express 提取请求报文参数

**原生的获取方式在 express 框架中都是可以使用的**

* request.query  //获取 查询字符串对象
* request.params //获取 请求参数路由的参数  拿到的是一个**对象**
  * 设置路径时,路径使用占位符来进行站位  ('/:pid.html') 进行站位
  * 然后通过request.params  进行获取请求路径

## 关于请求与响应API的总结

### 请求

* request.query  //获取 查询字符串对象
* request.params //获取 请求参数路由的参数  拿到的是一个**对象**
* request.headers //获取 请求头

### 响应

* response.statusCode = xx  //设置响应状态码
* response.setHeader();  //设置请求头
* response.end    response.send  //设置响应体
* response.download  //设置 下载
* response.redirect  // 网页重定向  跳转

| **属性/方法**     | **描述**                                             |
| ----------------- | ---------------------------------------------------- |
| request.query     | 获取get请求查询字符串的参数，拿到的是一个对象        |
| request.params    | 获取get请求参数路由的参数，拿到的是一个对象          |
| request.body      | 获取post请求体，拿到的是一个对象（要借助一个中间件） |
| request.get(xxxx) | 获取请求头中指定key对应的value                       |

### **Response对象的属性和方法**

| **属性/方法**              | **描述**                                   |
| -------------------------- | ------------------------------------------ |
| response.send()            | 给浏览器做出一个响应                       |
| response.end()             | 给浏览器做出一个响应（不会自动追加响应头） |
| response.download()        | 告诉浏览器下载一个文件                     |
| response.sendFile()        | 给浏览器发送一个文件                       |
| response.redirect()        | 重定向到一个新的地址（url）                |
| response.set(header,value) | 自定义响应头内容                           |
| res.status(code)           | 设置响应状态码                             |

## 中间件

中间件是一个函数,需要三个参数

1. request
2. response
3. next (下一个) 接受的是一个函数
   * 在中间件后的路由规则如果需要执行,则必须调用next( ) 函数,
   * 如果不调用next 则下面的路由回调将不起作用

* 全局中间件

  所有路由规则执行前,中间件都会执行一次

* 路由中间件

  只对其中一部分路由规则匹配,  

### 全局中间件的使用步骤

1. 声明中间件函数

2. 使用应用对象调用 use 方法进行设置

  ```
  app.use(logMiddleware);
  ```



### 路由中间件的使用步骤

1. 声明中间件函数

2. 在需要中间件执行的规则中, 设置中间件函数即可

3. ```js
      app.get('/admin', vipCheckMiddleware, (request, response) => {
      
   ​    response.send();
   
     })
   ```





### moment 时间工具包

官网http://momentjs.cn/

## express 静态资源

静态资源服务是 **express** 中的内置全局中间件

### 设置  静态资源服务

```js
//  可以自动根据客户端请求路径到指定文件夹下找到指定文件,将指定文件读取出来的内容设置为响应体
// static 是静态的意思
//在中间件中使用express 调用static方法 的返回值是一个函数
app.use(express.static(__dirname+'/public'));//设置全局中间件
//用来匹配某一文件夹下的某某文件
```



#### **静态资源**

长时间内容不会发生改变的资源,称之为  [ 静态资源 ]

* HTML
* CSS
* JS
* 图片
* 音视频
* 字体   等...

#### 动态资源

例如:淘宝

* 官网资源
* 列表页资源

## express 中 路由和静态资源服务应用场景

#### 路由规则

为客户响应动态资源内容(html)时.使用路由规则

* 首页  网页   表单页面   注册   登录  等...使用路由规则

#### 静态资源服务

为客户响应静态资源内容时,使用     `app.use(express.static());`

* html  css   js   图片   音视频  等...资源    使用静态资源服务

#### 网站根目录

脚本根据请求路径,到[哪个文件夹]下寻找对应的文件, [这个文件夹] 就是网站根目录

## 获取请求体数据

使用一个包来获取  `body-parser`  

**body-parser  的使用步骤**

```js
// 1,安装 npm i body-parser
// 2.引入 body-parser 模块
cosnt bodyParser = require('body-parser');
// 3.设置中间件   
// parse application/x-www-form-urlencoded 请求体的类型  对应的是查询字符串形式的参数
app.use(bodyParser.urlencoded({ extended: false }))
// 4.获取请求体数据
app.poat('/login',(request,response) => {
   //使用request的 body属性来获取查询字符串,
    console.log(request.body);//注: 返回的是一个对象  
})
```

## 路由器

路由器 是用来管理路由规则的

将一些路由规则放到不同的文件当中去,这些文件我们称之为 [路由器]

#### 路由器使用

```js
// 1. 引入express 模块
const express = require('express');
// 2. 创建路由器对象    Routor 是express对象上的一个方法
const router = express.Router();
// 3. 创建router 的规则
router.get('/',(request,response) => {
    
})

// 4. 暴露 router
module.exports = router;

// 5. 在服务器中引入路由器文件
const 名字 = require(__dirname+'/路径文件名')
// 6. 在服务器中设置中间件调用引入的路由器对象
app.use(名字);
```



## ejs 模板引擎

是一个工具包,用来分离页面(html)与数据(服务端 的js代码)的

#### ejs 使用

```js
// 1. 安装  npm i ejs
// 2. 引入 ejs 模块
const ejs = require('ejs');

let a = '天气';
// 调用ejs 的render 方法,需要两个参数  参数一:字符串内容    参数二:要使用的数据(服务端js代码)
ejs.render('今天<%= a%>真好',{a:a})
// 使用<%= 变量%>  作为 占位符, 

```

**实际开发中应用场景: html写在html文件中,需要js变量的地方使用 `<%= 变量%>` 进行占位,在服务器中使用fs读取html文件,然后使用ejs模块将js变量引入到读取到的html文件中**



ejs 中两种占位符

* ```js
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Document</title>
  </head>
  <body>
      <table>
          <tr><td>ID</td><td>歌手</td><td>歌曲</td></tr>
            //  可以使用js 代码 循环 和 if 判断等 逻辑运算  
          <% for(let i=0;i<data.length;i++){ %>
          <tr>
              <td><%= data[i].id %></td>
              <td><%= data[i].name %></td>
              <td><%= data[i].song %></td>
          </tr>
          <% } %>
      </table>
  </body>
  </html>
  ```

* 用占位符进行占位, 然后进行ejs解析时,把变量  替换掉相应的占位符

## express 中使用ejs

1. 安装ejs 包  npm i ejs

2. 设置                 set       设置模板引擎的类型

   ​	app.set('view engine' , 'ejs');//固定写法

   ​	app.set('views', \_\_dirname + '/模板文件夹');//设置模板存放的文件夹    具有ejs语法内容的文件就是模板文件

3. 

```js
const express = require('express');
const app = express();
//1. 安装ejs包
//2. 设置  set  设置模板引擎的类型
app.set('view engine', 'ejs');//固定写法
app.set('views', __dirname + '/html');// 设置模板的存放位置   模板 (具有ejs语法内容的文件)

app.get('/songs', (request, response) => {
    const songs = [
        {
            id:1,
            name: '孙燕姿',
            song: '逆光'
        }
    ];
    //3. 准备数据
    let data = {
        songs
    }
    //4. 页面的渲染(编译)  调用 render 方法
    //第一个参数为『模板的名称』,设置的时候不需要写后缀. 但是模板的名字必须为 ejs 后缀
    //第二个参数为『数据』
    response.render('table', data);
});

//响应一些明星的名字
app.get('/stars', (request, response) => {
    //准备数据
    let data = {
        stars: ['刘亦菲','王祖贤','刘德华','吴亦凡'],
        a:0
    };
    //响应
    response.render('abc', data);
});

app.listen(80);
```

## 页面内的url

**页面的 url 若没有填写『协议』『域名』『端口』, 发送请求时,使用的信息, 都是当前网页 url 的信息(『协议』『域名』『端口』)**

## 图片占位插件



```
https://github.com/imsky/holder
```
使用步骤
1. 在bootcen.cn官网搜索holder

2. 引入script  变迁

3. 语法:
	
	```js
	<img data-src="holder.js/200x200">             //图片占位200x200
	    
	<img data-src="holder.js/100px200">            //宽度100%高度200
	    
	<img data-src="holder.js/100px200?bg=#bfc">    //以?隔开多个属性设置,
	    
	<img data-src="holder.js/100px200?text=hello"> //设置文字
	```
	
	