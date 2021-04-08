# AJAX

## AJAX 介绍

AJAX 全称为Asynchronous Javascript And XML，就是异步的 JS 和 XML。

通过AJAX可以在浏览器中向服务器发送异步请求，最大的优势：无刷新获取数据。

AJAX 不是新的编程语言，而是一种将现有的标准组合在一起使用的新方式。

用来不刷新页面,向客户端相应数据



## XML简介

XML 可扩展标记语言。

XML 被设计用来传输和存储数据。

XML和HTML类似，不同的是HTML中都是预定义标签，而XML中没有预定义标签，全都是自定义标签，用来表示一些数据。

比如说我有一个学生数据：  

```js
name = "孙悟空" ; age = 18 ; gender = "男" ;  用XML表示： 

 <student>    
    <name>孙悟空</name>
	<age>18</age>    
	<gender>男</gender>  
</student>
```



现在已经被JSON取代了。

```js
用JSON表示：{"name":"孙悟空","age":18,"gender":"男"}
```



## AJAX 优点  和  缺点

#### 优点

1. 可以不刷新页面和服务端进行通讯
2. 允许根据用户事件更新部分页面内容

#### 缺点

1. 没有历史记录  不能进行回退
2. 存在跨域问题
3. 对SEO(搜索引擎) 不友好

## AJAX 使用

```js
// 定义AJAX对象
const xhr = new XMLHttpRequest();
// 请求方式和 地址
xhr.open('get','http://127.0.0.1');
// 发送请求
xhr.send();
// 绑定事件 操作相应数据
xhr.onreadystatechange = function(){
    // 当状态为4 时,表示响应数据已经接受完毕
    if(xhr.readyState === 4){
        //获取相应数据
        console.log(xhr.response)
    }
}
```



## 解决IE缓存问题

问题：在一些浏览器中(IE),由于缓存机制的存在，ajax只会发送的第一次请求，剩余多次请求不会在发送给浏览器而是直接加载缓存中的数据。

解决方式：浏览器的缓存是根据url地址来记录的，所以我们只需要修改url地址即可避免缓存问题

```
xhr.open("get","/testAJAX?t="+Date.now());
```



## 同源策略

同源策略(Same-Origin Policy)最早由 Netscape 公司提出，是浏览器的一种安全策略。

同源： 协议、域名、端口号 必须完全相同。 两个资源必须来自同一个服务.

违背同源策略就是跨域。



## 解决 AJAX  跨域

#### **设置CORS**

批量设置请求允许跨域(固定写法)

```js
app.all('*',(request,response,next) => {
    response.set('Access-Control-Allow-Origin','*');
    // response.setHeader('Access-Control-Allow-Headers','*')
    next();
})
```

#### 使用jsonp发送请求

<span style='color:red'>关于  script  标签 : script标签默认情况下会将src属性访问的文件或接口返回的内容,解析为js代码,即使该文件或接口返回的内容,为其他格式:例如:字符串</span>

****

1. jsonp 是什么

   ​		JSONP(JSON with Padding)，是一个非官方的跨域解决方案，纯粹凭借程序员的聪明才智开发出来，只支持get请求。

2. jsonp的工作原理

   ​		在网页有一些标签天生具有跨域能力，比如：img link iframe script。

   ​		JSONP就是利用script标签的跨域能力来发送请求的。

3. jsonp的使用(如何使用jsonp方式发送请求)

   * 客户端代码

   ```js
   // 定义一个函数
   function fn(str){
       console.log(str)
   }
   
   
   // 1. 通过触发dom事件,在事件中动态创建script标签
   btn.onclick = function(){
       const script = docuemnt.createElement('script');
       // 2. 给script标签添加src属性,并将值设置为一个服务接口
       script.src = 'http://127.0.0.1/a?collback='+fn;
       // 3. 将script标签添加到body标签中
       document.body.appendChild(script);
       // 4. 将刚增加的script 标签移除
       document.body.removeChild(script);
   }

   ```

   * 服务端代码
   
   ```js
   const express = require('express');
   const app = express();
   
   // 创建路由规则
   app.get('/a',(request,response) =>{
       // 接收数据 接收函数名
       const collback = request.query.collback;
       // 响应数据  将函数名进行拼接
       response.send(collback+'(123)');
   })
   app.listen(80);//监听80端口
   
   ```

   ### jsonp 实例

   通过在文本框中输入内容,自动更新与输入内容相关的关键词信息

   ```js

   ```

#### 服务代理

服务代理就是,使用ajax跨域访问其他站点服务的时候,无法通过`sors` 和`jsonp` 解决跨域,此时就需要,自己创建一个服务,用这个服务去访问,其他站点的服务,<span style="color:red">因为只有ajax存在跨域</span>

* 是一个npm 的组件  需要进行下载   npm i request -S

* 使用

```javascript
// 1. 引入 request 模块
const request = require('request');
// 2. 执行代理函数
/*
	需要两个参数:
		第一个参数: 要访问的服务地址
		第二个参数: 一个函数  需要三个参数
				1. err  存储报错信息,没有报错为null
				2. response  存储的是一个对象, 
						需要关注的三个属性 
							statusCode: 响应状态码
							statusMessage: 响应状态字符串
							body:响应的内容
				
				3. body   存储的是请求地址给响应的内容
*/
request('https://m.lagou.com/listmore.json?pageNo='+pageNo+'&pageSize=15',function(err,response,body){
        if(!err && response.statusCode === 200){
            // console.log(body);
            // 结合jsonp 响应信息
            res.send(cb+'('+JSON.stringify({
                ok:1,
                result:JSON.parse(a).content.data.page.result
            })+')')
            //正常响应信息
            res.json({
                ok:1,
              	result:JSON.parse(body).content.data.page.result
            })
        }else{
            res.json({
                ok:-1,
                msg:'请求失败'
            })
        }
    })
```



## CORS

1. CORS是什么？

   CORS（Cross-Origin Resource Sharing），跨域资源共享。CORS是官方的跨域解决方案，它的特点是不需要在客户端做任何特殊的操作，完全在服务器中进行处理，支持get和post请求。

2. CORS怎么工作的？

   CORS是通过设置一个响应头来告诉浏览器，该请求允许跨域，浏览器收到该响应以后就会对响应放行。

3. CORS的使用

   主要是服务器端的设置：

   ```js
   app.get("/testAJAX" , function (request , response) {
   
   	//通过res来设置响应头，来允许跨域请求
   
   	//response.set("Access-Control-Allow-Origin","http://127.0.0.1:3000");  
   
   	response.set("Access-Control-Allow-Origin","*");
    
   	response.send("testAJAX返回的响应");
   
   });
   ```

   

## 取消form表单的默认行为

**form表单默认是会跳转页面的,在ajax提交post请求时,不希望表单有默认行为,就需要通过事件对象的perventDefault方法来进行取消默认行为**

```js
btn.onclick = function(event){
    event.preventDefault();
    //该方法可以取消form表单的默认行为
}
```



## 以ajax提交post请求

## 请求体格式设置

##### 请求体为 字符串

设置请求头为``'content-type','application/x-www-form-urlexcoded'``

<span style="color:red">在open 和 send 之间设置, 在其他位置设置没有效果</span>

```
xhr.setRequestHeader('content-type','application/x-www-form-urlexcoded');//设置请求体的格式为字符串
```

##### 请求体为 JSON格式数据

设置请求头为``'content-type','application/json'``

<span style="color:red">在open 和 send 之间设置, 在其他位置设置没有效果</span>

```javascript
xhr.setRequestHeader('content-type','application/json');//设置请求体的格式为json格式
xhr.send(JSON.stringify({
	a:100
}))
```



## 请求体不同格式在服务端接收设置

##### 请求体为 字符串

首先引入body-parser 模块

```js
const app = require('express')();
const bodyParser = require('body-parser');
//设置中间件  指定要接收的格式为字符串
app.use(bodyParser.urlencoded({
    extended:false//设置是否允许深度解析  true为是   false为否
}));
// 必须设置
app.all('*',(request,response,next) => {
    response.set('Access-Control-Allow-Origin','*');//允许跨域
    response.setHeader('Access-Control-Allow-Headers','*');//允许携带数据
    next();
})

```



##### 请求体为 json 格式数据

首先引入body-parser 模块

```js
const app = require('express')();
const bodyParser = require('body-parser');
//设置中间件  指定要接收的格式为json格式
app.use(bodyParser.json());

```





## 数组扩展 map方法

map是es6中提供的一个关于数组扩展的方法,该方法可以映射出来一个新的数组,并返回一个新的数组,

* map接收一个参数,该参数的类型为函数
* 根据数组的长度,进行遍历,将原数组中的值作为返回值(return),放到一个新的数组当中去

```js
const hobby = ['1','2','3'];
/*
	value: 表示当前数组中的值
	index: 表示对应的索引
	arr: 表示当前数组本身
*/
const one = hobby.map(function(value,index,arr){
    return hobby[value];
})
//简写 
const one = hobby.map(value => hobby[value])
```

## AJAX中的属性

* onload   (事件)  当数据返回完成以后执行   值的类型是一个函数
* onreadystatechange   (事件)  监听状态, 当状态发生改变时,执行  值得类型是一个函数
  * 监听xhr的整个生命周期当中执行的阶段
  * 状态 : 0	初始状态
  * 状态 : 1    open执行了
  * 状态 : 2    send执行了
  * 状态 : 3    服务端返回了部分数据
  * 状态 : 4    服务端返回了全部数据
* onerror  (事件)  当发生异常时执行
* readyState  (属性)    状态的值   (0  1  2  3  4)
* status  (属性)    响应状态码
* statusText  (属性)    响应状态字符串
* responseText  (属性)    接收响应体  <span style="color:red">值只能为字符串</span>
* response  (属性)    接收相应内容(可以接收json数据)   <span style="color:red">值得类型和xhr.responseType 的类型一致</span>
* responseType  (属性)    设置响应体返回数据的类型  
  * 默认值: "text"或""     返回数据为字符串
  * "json"  返回数据为json格式
  * <span style="color:red">注:如果响应体为字符串,但是responseType设置为json,则无法接收到相应内容</span>
* xhr.getAllResponseHeaders()  (方法)  获得所有响应头的信息
* xhr.getResponseHeader('响应头名')  (方法)    获得指定响应头信息的值
* xhr.timeout  (属性)    设置相应的时间  单位:毫秒(单位不用写)
  * 可以指定请求过期的时间,如果超时,会取消请求
* xhr.ontimeout  (事件)   请求超时被触发 当timeout设置的时间过期后,执行该函数
* xhr.abort()   (方法)   用来主动取消请求
* xhr.onabort  (事件)    监听abort事件是否被执行,请求被取消执行该事件
* 

## jQuery 发送AJAX

#### 直接调用jquery中的get / post / put / delete  方法

* $.get()方法    参数:
  * 第一个参数:	请求地址
  
  * 第二个参数:    传递的参数,以对象的形式传递
  
  * 第三个参数:    回调函数(负责接收服务端返回的结果)  ,  <span style="color:red">服务端返回的响应体会作为回调函数的第一个参数进行传递</span>
  
  * 第四个参数:    设置响应体的类型  默认为 json 格式   ,如果传递  'text'  响应体会被解析成字符串
  
  * ```js
    $.get('http://127.0.0.1/jquery',{
        a:100,
        b:200
    },function(res){
        console.log(res);
    },'text');
    ```
  
  * 
* $.post()方法    参数:
  * 第一个参数:	请求地址
  
  * 第二个参数:    传递的参数,以对象的形式传递  <span style="color:red">默认以 application/x-www-form-urlencoded  (字符串)   格式传递数据</span>
  
  * 第三个参数:    回调函数(负责接收服务端返回的结果)  ,  <span style="color:red">服务端返回的响应体会作为回调函数的第一个参数进行传递</span>
  
  * 第四个参数:    设置响应体的类型  默认为 json 格式   ,如果传递  'text'  响应体会被解析成字符串
  
  * ```js
    $.post('http://127.0.0.1/jquery',{
        a:200,
        b:300
    },function(res){
        console.log(res);
    })
    ```
  
  * 





#### jquery中原生ajax

* $.ajax( { } )方法     参数为一个对象    对象中的属性规定了请求的类型,地址,传递的参数,接收参数

  * 对象中的参数

    * type :    决定请求的方式    <span style="color:red">如果不写该属性,则默认使用 `get` 方法发送请求</span>

    * url :    请求地址

    * headers :    设置请求头  以对象的形式进行设置  ,  <span style="color:red">如果不设置请求头,则默认请求体格式为字符串</span>

      * ```
        headers:{
        	'content-type':'application/json',//如果不设置json格式,则默认请求体格式为字符串
        }
        ```

      * 

    * data :  发送的数据   <span style="color:red">需要将数据转换成字符串进行传递</span>

      * ```js
        // 固定格式:
        // 因为可能同时发送多个参数,字符串形式书写不方便,所以使用将对象转换成字符串方式,进行传递
        data:JSON.stringify({a:100,b=200});
        data:JSON.stringify('a=100&b=200');
        ```
      
    * success(){}   :   接收成功的数据 (是一个回调函数) **<span style="color:red">该函数名必须为:  success</span>**
    
    * dataType  :     设置接收的响应体格式  **不设置则默认为json格式**  'text'  设置为字符串格式
    
    * timeout  :   设置请求过期时间
    
    * error(){}  :   请求失败的回调  



```

    $.ajax({
        type:'post',
        url:'http://127.0.0.1/jquery',
        headers:{
            'content-type':'application/json'
        },
        data:JSON.stringify({
            a:100
        }),
        success(res){
            console.log(res);
        },
        error(){
            console.log('请求失败')
        }
    })
```

  * 

#### getJSON  用来获取json数据  <span style='color:red'>只支持get方式</span>

* $.getJSON()  方法   可以理解为$.get() 的另外一种写法     主要用来获得json数据的

* <span style='color:red'>此时发送请求的方式为get方式</span>

* 参数:

  * 第一个参数:  请求地址

  * 第二个参数:  发送的数据

  * 第三个参数:  回调函数   响应体的信息会作为该回调函数的参数进行传递

  * ```js
    $.getJSON('http://127.0.0.1/jquery',{
    },function(res){
        console.log(res);
    })
    // 此时发送请求的方式为get方式
    
    ```

#### jquery  实现  jsonp

**通过getJSON 可以实现jsonp**

**<span style='color:red'>如果地址中的请求字符串中使用了 `?` 作为某一个键值对中的值,此时getJSON发送请求的方式为  script  方式</span>**

* 客户端代码

```js
// 地址中的abc=? 中的 ? 会被解析成一个随机的id,可以作为函数名来使用
// 此时的geiJDON发送请求的方式会变成script 方式
$.getJSON('http://127.0.0.1/jquery?abc=?',{
},function(res){//这个函数会作为,函数体,由`?`生成的最忌id 作为函数名进行调用
    console.log(res);
})
```

* 服务端代码

```js
app.get('/jquery',(request,response) => {
    console.log(111,request.query.abc);
    const {abc} = request.query;
    response.send(abc+'('+JSON.stringify({
        ok:1
    })+')');
})
```

**通过jquery中的AJAX  实现jsonp**

```js
$.ajax({
    type:"get",
    url:"https://www.baidu.com/sugrec",
    data:{
        prod:"pc",
        wd:"三"
    },
    jsonp:"cb",// 更改默认的参数名callback
    jsonpCallback:"abcdefg",// 指定名字  司改jquery生成的随机数
    dataType:"jsonp",//设置请求数据的类型
    success(res){//此时success回调的名字就是jsonpCallback的值
        console.log(res);
    }
})
```

## storage  存储数据 . 存储于客户端

*  **<span style="color:red">将数据缓存到浏览器(前端页面)刷新时,浏览器已经缓存了数据,就不需要再次提交请求,减小服务器的压力,提高用户体验</span>**
* **<span style="color:red">可以实现页面的数据共享</span>**
* **<span style="color:red">注: localstorage 是window下的一个属性</span>**





#### localStorage  永久性存储    存储在硬盘当中

**<span style="color:red">注: localstorage 是window下的一个属性</span>**

* **<span style="color:red">注: 读取内容,实现页面数据通讯,必须在同一个域名下面</span>**

* <span style="color:red">在  localStorage  中添加内容的值都会被解析成字符串类型</span>

##### 添加storage

* 任意一种方式都可以读取内容

```
// 1. 通过给 localStorage 添加属性的方式 添加内容
localStorage.属性名 = "属性值"
// 2. 通过给 localStorage["属性名"]  的方式添加内容
localStorage['属性名'] = 12
// 3. 通过给 localStorage.setItem()方法  的方式添加内容
// 参数1:  属性名     参数2: 属性值 
localStorage.setItem("属性名","属性值");
```

##### 读取Storage

* **<span style="color:red">当读取的内容没有值时`getItem` 方法的返回值为 `null` , 其他方式返回 `undefined`</span>**

```
// 任意一种方式都可以读取内容
console.log(localStorage.属性名);
console.log(localStorage['属性名']);
console.log(localStorage.getItem('属性名'))
```

##### 修改storage

```js
// 以下三种方式 都可进行修改操作
localStorage.属性名 = 新值
localStorage["属性名"] = 新值
localStorage.setItem("属性名") = 新值
```

##### 删除storage

```js
// 删除指定属性
localStorage.removeItem("属性名")

// 批量删除 删除所有内容
localStorage.clear()
```



存储在硬盘当中,

#### sessionStorage  浏览器存储     当关闭浏览器,即数据丢失

**<span style="color:red">注: sessionStorage 的页面数据共享需要通过超链接跳转打开的页面,才可一个进行数据共享  单独打开的页面不可以</span>**

**<span style="color:red">注: sessionStorage 是window下的一个属性</span>**

当关闭浏览器,即数据丢失

* <span style="color:red">在  localStorage  中添加内容的值都会被解析成字符串类型</span>

##### 添加storage

* 任意一种方式都可以读取内容

```
// 1. 通过给 localStorage 添加属性的方式 添加内容
localStorage.属性名 = "属性值"
// 2. 通过给 localStorage["属性名"]  的方式添加内容
localStorage['属性名'] = 12
// 3. 通过给 localStorage.setItem()方法  的方式添加内容
// 参数1:  属性名     参数2: 属性值 
localStorage.setItem("属性名","属性值");
```

##### 读取Storage

* **<span style="color:red">当读取的内容没有值时`getItem` 方法的返回值为 `null` , 其他方式返回 `undefined`</span>**

```
// 任意一种方式都可以读取内容
console.log(sessionStorage.属性名);
console.log(sessionStorage['属性名']);
console.log(sessionStorage.getItem('属性名'))
```

##### 修改storage

```js
// 以下三种方式 都可进行修改操作
sessionStorage.属性名 = 新值
sessionStorage["属性名"] = 新值
sessionStorage.setItem("属性名") = 新值
```

##### 删除storage

```js
// 删除指定属性
sessionStorage.removeItem("属性名")

// 批量删除 删除所有内容
sessionStorage.clear()
```







#### storage  事件

1. storage  事件  当storage发生变化时触发

* <span style="color:red">该事件属于dom2级事件  需要通过addEventListener()  进行绑定</span>

```js
window.addEventListener('storage',function(event){
 	// event 事件对象 该对象中有三个属性  newValue(新值)  oldvalue(旧值) key(属性名)
	// 需要判断key  是否等于 要修改的属性的属性名
   	if(event.key === '属性名'){
        //进行操作
    }
})
```





## canvas  画布



## md5 加密

md5 是用来将用户的密码等隐私进行加密的, 并且不能进行反编译,将加密后的





# cookie

#### 介绍

#### cookie 是什么

本质是一个储存在浏览器的文本,最终会放置到硬盘当中,随着http请求自动传递给服务器。

也可以说cookie是一个头（请求头/响应头）：

* 服务器以响应头的形式将Cookie发送给浏览器
* 浏览器收到以后会自动将Cookie保存
* 浏览器再次访问服务器时，会以请求头的形式将Cookie发回
* 服务器就可以通过检查浏览器发回的Cookie来识别出使用浏览器不同的用户



#### 使用cookie

1. 安装 cookie-parser 模块

```
npm i cookie-parser -S
```

2. 引入cookie 并添加中间件

```
const cookieParser = require("cookie-parser");
// 会在响应对象中增加 cookie 函数。将将接收的cookie转为对象
app.use(cookieParser());
```

3. 设置cookie  使用相应对象的cookie函数进行设置

```
// 参数1: 作为cookie的 key
// 参数2: 作为cookie的 value

// 注意: cookie的value 最终都会以字符串进行保存

res.cookie('userName',userName);
res.cookie('age',18);
res.cookie('email','123456@qq.con');
```

4. 客户端 操作cookie

```
// 获取 cookie 字符串
// userName=liu; age=18; email=123456%40qq.con
console.log(document.cookie)
```









#### 总结

服务端生成cookie,发送给浏览器,在浏览器中保存

cookie 就是从服务端发出的一个用来识别用户的id,会自动保存在客户端(浏览器)

当客户端再次发起请求时,会自动发送cookie,

当客户端关闭时,丢失

#### cookie 附录

浏览器在储存cookie时,会对一些特殊字符进行编译  当我们使用时需要将浏览器编译后的字符串再编译回原来的字符

```
// 手动将特殊字符编译
console.log(encodeURIComponent(email));
// 将编译后的特殊字符 进行编译回去
console.log(decodeURIComponent(email));
```



# session

#### session 是什么

Session 是一个对象，存储特定用户会话所需的属性及配置信息。Session是保存在服务器端的数据.（保存介质， 文件、数据库、内存）

#### session 运作流程

我们可以在服务器中为每一次会话创建一个对象，然后每个对象都设置一个唯一的id，并将该id以cookie的形式发送给浏览器，然后将会话中产生的数据统一保存到这个对象中，这样我们就可以将用户的数据全都保存到服务器中，而不需要保存到客户端，客户端只需要保存一个id即可。

#### session 的使用

1. 安装

```
npm i express-session --save
```

2. 引入session模块

```
const expressSession = require('express-session');
```

3. 设置中间件  **<span style="color:red">注意: secret 密钥设置好以后,不能进行更改,否则之前加密的数据,将不能进行解密编译</span>**
   * 设置中间件时,session函数中需要接收一个对象,对象中保存着session的配置信息
   * <span style="color:red">设置中间件其实是给请求对象request增加了一个session属性,他是一个对象,用来设置和读取session</span>
     * name属性:    
       * 返回客户端的key的名字，
       * 默认值是connect.sid,通过name属性，将key重新设置
     * secret属性:   
       * 值为一个字符串 以字符串中的内容进行加密和解密
     * resave属性:   
       * 是否在每次重新请求时都重新保存session  
       * 一般设置为false 不需要重新保存
     * saveUninitialized属性:  
       * 是否为每次请求都设置一个cookie用来存储session的id
       * 设置session的初始值
       * 值为false :  表示,没有设置session时,无值
       * 值为true :  表示不管有没有设置session,都有值
     * cookie属性:
       * 值是一个对象,在对象中可以设置过期的时间和是否允许前端通过js进行操作
       * maxAge:  设置过期时间  单位:毫秒
       * httpOnly: 是否允许前端通过js操作
         * true: 不允许前端操作
         * false: 允许前端操作

```
app.use(session({
  name: 'id22',   //设置cookie的name，默认值是：connect.sid
  secret: 'atguigu', //参与加密的字符串（又称签名）
  saveUninitialized: false, //是否为每次请求都设置一个cookie用来存储session的id
  resave: true ,//是否在每次请求时重新保存session  默认值为true保存  false  不保存
  cookie: {
    httpOnly: true, // 开启后前端无法通过 JS 操作  true可以操作  false  不能操作
    maxAge: 1000*30 // 这一条 是控制 sessionID 的过期时间的！！！  时间以毫秒为单位
  }
}));
```

4. 设置session   **<span style="color:red">需要通过  请求对象  中的  session  对象进行设置</span>**
   * <span style="color:red">设置中间件其实是给请求对象request增加了一个session属性,他是一个对象,用来设置和读取session</span>
   * req.session.key值  = value值

```
req.session.userName = "xixi";
req.session.age = 12;
// 以cookie的形式发送给浏览器, 加密以后的
// 在浏览器是没有办法直接读取session生成的cookie
// 只能在浏览器进行解密后进行查看
// 
```

5. 读取session
   * 客户端发起请求时,会自动将以保存的session以cookie的形式返回,
   * 通过请求对象中的session属性可以得到返回的session,<span style="color:red">返回的是一个对象</span>

```
req.session.userName
req.session.age  
// 再服务端可以通过请求对象中的session对象点key的形式读取
```

6. 删除session

   * (1) 因为session在浏览器中是以cookie的形式保存的可以来服务端设置session的过期时间来进行删除
   * (2) 通过设置session的destroy来删除session  需要传递一个函数作为回调,该回到会在session销毁以后被执行  destroy会自动将浏览器返回的session全部销毁,全部销毁以后,执行回调中的代码

   ```
   
   ```

   





#### session 总结

session是存储在服务器端的,在服务器生成session ,以cookie的形式发送给浏览器,

,浏览器不能够进行解密,只能将加密后的session以cookie的形式返回给服务器

然后在服务端进行解密,获取其中的数据,然后识别不同的用户

session 就是将加密后和cookie, 发送给客户端, 但是session保存在服务端的

* 以cookie的形式发送给浏览器, 加密以后的

* 在浏览器是没有办法直接读取session生成的cookie

* 浏览器发送请求时,在服务端通过密钥解密以后,可以查看session生成的cookie
* 





****

## session 和 cookie 的区别

#####存放的位置

cookie 存在于客户端

session 存在于服务器端，一个session域对象为一个用户浏览器服务

##### 安全性

cookie是以明文的方式存放在客户端的，安全性较低，可以通过一个加密算法进行加密后存放

session存放于服务器中，所以安全性较好

##### 网络传输量

cookie会传递消息给服务器

session本身存放于服务器，但是通过cookie传递id，会有少量的传送流量

##### 大小

cookie 保存的数据不能超过4K，很多浏览器都限制一个站点最多保存50个cookie

session 保存数据理论上没有任何限制

Storage :  保存在前端。无法与服务器端直接通讯。

保存的数量大。M  

无法设置过期时间。

只能够存储字符串。



# promise

## promise 是什么?

### 理解

1. 抽象表达

   * Promise是一门新的技术(ES6规范)

   * Promise是JS中进行异步编程的新解决方案

     备注：旧方案是单纯使用回调函数

2. 具体表达:

   * 从语法上来说: Promise是一个构造函数
   * 从功能上来说: promise对象用来封装一个异步操作并可以获取其成功/失败的结果值

### promise的状态改变

* 初始状态: pendding

* pending变为resolved  成功状态

* pending变为rejected  失败状态

* 一个Promise实例对象 每次执行,最终状态只会有一种,成功或失败,不会改变

* 说明: 只有这2种, 且一个promise对象只能改变一次

     无论变为成功还是失败, 都会有一个结果数据

     成功的结果数据一般称为value, 失败的结果数据一般称为reason

### 如何改变promise的状态

* resolve(value): 如果当前是pending就会变为resolved
* reject(reason): 如果当前是pending就会变为rejected
* 抛出异常: 如果当前是pending就会变为rejected

### promise的基本流程

![Promise执行流程图](F:%5C%E7%AC%94%E8%AE%B0%5CTypora%E7%AC%94%E8%AE%B0%5CAJAX.assets%5CPromise%E6%89%A7%E8%A1%8C%E6%B5%81%E7%A8%8B%E5%9B%BE.png)

## promise 使用

1. 通过new Promise 构造函数创建实例,并传递一个函数作为参数,也叫执行器then,
   * 在实例化对象时,执行器会在Promise内部立即同步调用,异步操作在执行器中执行
2. 执行器需要两个形参      最终运行的结果通过失败/成功的回调传递出去
   * 参数1:   resolve  成功的回调
   * 参数2:   reject     失败的回调 
3. 在执行器中写异步操作的代码
4. 通过实例对象的then() 方法,处理结果
   * 传递两个函数作为成功/失败的回调
   * 参数1:  成功的回调
   * 参数2: 失败的回调
   * 并且有返回值,也是一个promise的实例

```
new Promise(function(resolve,reject){
    	if(err) reject(err)
   	 	else resolve(value.toString())
})
```

## 使用promise的优点和缺点

### 优点

* 指定回调函数的方式更加灵活  <span style="color:red">可以在启动异步任务后制定回调</span>
  * promise: 启动异步任务 => 返回promie对象 => 给promise对象绑定回调函数
  * (甚至可以在异步任务结束后指定/多个)

```js
const p1 = new Promise(function(resolve,reject){
    setTimeout(()=>{
        resolve('success')
    },1000)
})

setTimeout(()=>{
    p1.then(value=>{
        console.log(value);
    })
},3000)
setTimeout(()=>{
    p1.then(value=>{
        console.log(value);
    })
},3000)
setTimeout(()=>{
    p1.then(value=>{
        console.log(value);
    })
},3000)


```

* 支持链式调用, 可以解决回调地狱问题

```
const fs = require('fs');
// 封装Promise 链式调用
function getProm(FileName){
    return new Promise(function(resolve,reject){
        fs.readFile(FileName,(err,value)=>{
            if(err) reject(err)
            else resolve(value.toString())
        })
    })
}

getProm(__dirname+'/text/a.txt')
.then(value=>{
    console.log(value)
    return getProm(__dirname+'/text/s.txt')
},reason=>{
    console.log(reason)
})
.then(value=>{
    console.log(value);
    return getProm(__dirname+'/text/c.txt');
},reason=>{
    console.log(1111111111,reason)

})
.then(value=>{
    console.log(value);
})
```

## Promise API

* Promise构造函数: Promise (excutor) {}

  * executor函数:  执行器  (resolve, reject) => { }
  * resolve函数: 内部定义成功时我们调用的函数 value => { }  then接收的第一个参数
  * reject函数: 内部定义失败时我们调用的函数 reason => { }  then接收的第二个参数
  * 说明: executor会在Promise内部立即同步调用,异步操作在执行器中执行

* Promise.prototype.then( ) 方法

  * 该方法通过promise的实例对象调用 传递两个函数作为参数

  * 参数1:  onResolved函数  成功的回调函数  (value) => { }  then接收的第一个参数

  * 参数2:  onRejected函数  失败的回调函数 (reason) => { }  then接收的第二个参数

  * <span style="color:red">成功/失败函数,只会执行其中一个,由实例对象的状态决定</span>

  * 说明: 指定用于得到成功value的成功回调和用于得到失败reason的失败回调

    返回一个新的promise对象

  ```
  const p1 = new Promise(function (resolve,reject) {
      reject(1);
  })
  p1.then(value=>{
      console.log(value);
  },reason => {
      console.error(reason);
  })
  ```

* Promise.prototype.catch方法: p1.catch(reason=>{console.log(reason);})

  * 该方法通过promise的实例对象调用 用于接收失败的回调
  * 说明: then()的语法糖, 相当于: then(undefined, onRejected)中的onRejected

  ```
  const p1 = new Promise(function (resolve,reject) {
      reject(1);
  })
  p1.catch(reason=>{
      console.log(reason);//1
  })
  
  // 等同于下面的代码
  p1.then(value=>{ // 不会被执行
      console.log(value);
  },reason => { //  会被执行
      console.error(reason);
  })
  ```

* Promise.resolve方法: Promise.resolve(value);

  * 该方法.是构造函数Promise对象的方法,
  * 说明: 返回一个成功/失败的promise对象
  * 需要传入一个参数
    * 传入promise实例以外的任意值,返回一个成功状态的promise实例
      * 传入的值会作为,返回的promise实例的成功回调的参数
    * 传入一个promise实例,
      * 该实例会作为resolve方法的返回值返回
  * 应用场景:  用来快速创建一个成功状态的Promise的实例对象

  ```
  // 传入一个非Promise,返回一个成功状态的Promise
  // 
  const p1 = Promise.resolve(1);
  p1.then(value=>{
      console.log(value);
  })
  
  // 传入的是一个Promise实例对象,那么resolve的返回值就是该实例对象
  // 返回值的状态是由 
  const p = new Promise(function (resolver,reject) {
      // resolve(1);
      // reject(2);
  })
  const p1 = Promise.resolve(p);
  // console.log(p1);
  // console.log(p1 === p);// true
  p1.then(undefined,function (reason) {
      console.log(reason);// 2
  })
  ```

* Promise.reject方法:Promise.reject(value);

  * reason: 失败的原因
  * 返回的永远都是状态为失败的Promise.
  * 需要传入一个参数
    * 传入promise实例以外的任意值,返回一个失败状态的promise实例
      * 传入的值会作为,返回的promise实例的失败回调的参数
    * 传入一个promise实例,
      * 该实例会作为resolve方法的返回值返回

  ```
  //  非promise
  const p1 = Promise.reject(1);
  p1.catch(reason => {
      console.log(reason);
  })
  
  // 传入的是一个失败状态的 promise ,p1是失败的,值是传入的promise
  const p1 = Promise.reject(new Promise(function (resolve,reject) {
      reject("异常")
  }));
  p1.catch(reason => {
      reason.then(value=>{
          console.log("成功")
      },reason => {
          console.error(reason)
      })
  })
  
  // 传入的是一个成功状态的promise,p1是失败的。值是传入的promise
  const p1 = Promise.reject(new Promise(function (resolve,reject) {
      resolve("异常")
  }));
  p1.catch(reason => {
      reason.then(value=>{
          console.log(value)
      },reason => {
          console.error(reason)
      })
  })
  
  ```

* Promise.all方法: Promise.all( [ ] );

  * 需要一个包含  n个promise的数组  作为参数
  * 返回一个新的promise
  * 只有所有的promise都成功才成功,只要有一个失败了就直接失败
  * 如果all方法传入的数组内的promise全部为成功状态
    * 则返回的promise的成功回调的    value 是一个数组  数组中的元素即是相对应的promise的返回值
  * 如果all方法传入的数组内的promise有为失败状态的 执行失败回调
    * 失败回调的参数就是 第一个失败状态promise的返回值
    * 只会返回第一个失败状态promise的返回值

  ```
  const p1 = Promise.resolve(1);
  const p2 = Promise.resolve(2);
  const p3 = Promise.resolve(3);
  const p4 = Promise.resolve(4);
  const p5 = Promise.resolve(5);
  const p = Promise.all([p5,p1,p2,p3,p4]);
  // 如果all传入的数组内的promise全部成功状态的。value 是一个数组，
  // 数组的元素即是相对应的promise返回值。
  p.then(value=>{
      console.log(value);//[1,2,3,4,5]
  }).catch(reason => {
      console.error(reason);
  })
  ```

* Promise.race方法: Promise.race( [ ] )

  * 需要一个包含n个promise的数组
  * 返回一个新的promise,
  *  第一个完成的promise的结果状态就是最终的结果状态
  * 会将数组中第一个执行完的promise 的返回值返回,不区分成功/失败的状态

  ```
  const p1 = new Promise(function (resolve) {
      setTimeout(function () {
          resolve(1);
      },200)
  })
  const p2 = new Promise(function (resolve) {
      setTimeout(function () {
          resolve(2);
      },400)
  })
  const p3 = new Promise(function (resolve,reject) {
      setTimeout(function () {
          reject(3);
      },100)
  })
  // 谁先执行完，那么得到的就是谁的结果。与状态无关。
  const p = Promise.race([p1,p2,p3]);
  p.then(value=>{
      console.log(value);
  },reason => {
      console.error(reason);//3
  })
  ```


## promise 附录

####关键问题

1.  一个promise指定多个成功/失败回调函数, 都会调用吗?

   * 当promise改变为对应状态时都会调用

2. 改变promise状态和指定回调函数谁先谁后?

   1. 都有可能, 正常情况下是先指定回调再改变状态, 但也可以先改状态再指定回调
   2. 如何先改状态再指定回调?
      * 在执行器中直接调用resolve()/reject()
      * 延迟更长时间才调用then()
   3. 什么时候才能得到数据?
      *  如果先指定的回调, 那当状态发生改变时, 回调函数就会调用, 得到数据
      *  如果先改变的状态, 那当指定回调时, 回调函数就会调用, 得到数据

3. promise.then()返回的新promise的结果状态由什么决定?

   * 简单表达: 由then()指定的回调函数执行的结果决定

   * 详细表达:

     * ① 如果抛出异常, 新promise变为rejected, reason为抛出的异常

     * ② 如果返回的是非promise的任意值, 新promise变为resolved, value为返回的值

     * 如果返回的是另一个新promise, 此promise的结果就会成为新promise的结果

4. promise如何串连多个操作任务?

   * promise的then()返回一个新的promise, 可以开成then()的链式调用
   * 通过then的链式调用串连多个同步/异步任务

5. promise异常传透?

   * 当使用promise的then链式调用时, 可以在最后指定失败的回调,
   * 前面任何操作出了异常, 都会传到最后失败的回调中处理

6. 中断promise链?

   * 当使用promise的then链式调用时, 在中间中断, 不再调用后面的回调函数
   * 办法: 在回调函数中返回一个pendding状态的promise对象