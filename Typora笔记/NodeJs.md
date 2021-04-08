# Nodejs

## 介绍

Node.js 是一个基于 Chrome V8 引擎的 JavaScript 运行环境，是一个应用程序。

官方网址 <https://nodejs.org/en/>，中文站 <http://nodejs.cn/>

## 作用

- 解析运行 JS 代码 
- 操作系统资源，如内存、硬盘、网络

## 应用场景

* APP 接口服务
* 网页聊天室
* 动态网站, 个人博客, 论坛, 商城等
* 后端的Web服务，例如服务器端的请求（爬虫），代理请求（跨域）
* 前端项目打包(webpack,  gulp)

## 使用

### 下载安装

工具一定要到官方下载，历史版本下载 <https://npm.taobao.org/mirrors/node/>

![img](F:%5C%E7%AC%94%E8%AE%B0%5CTypora%E7%AC%94%E8%AE%B0%5CNodeJs.assets%5C20190329115938531-1609124321006.png)

Nodejs 的版本号奇数为开发版本，偶数为发布版本，<span style="color:red">我们选择偶数号的 LTS 版本（长期维护版本 long term service）</span>

![1572676490692](F:%5C%E7%AC%94%E8%AE%B0%5CTypora%E7%AC%94%E8%AE%B0%5CNodeJs.assets%5C1572676490692-1609124321006.png)

双击打开安装文件，一路下一步即可😎，默认的安装路径是 `C:\Program Files\nodejs`

安装完成后，在 CMD 命令行窗口下运行 `node -v`，如显示版本号则证明安装成功，反之安装失败，重新安装。 

![1572678177784](F:%5C%E7%AC%94%E8%AE%B0%5CTypora%E7%AC%94%E8%AE%B0%5CNodeJs.assets%5C1572678177784-1609124321006.png)

### 初体验

#### 交互模式

在命令行下输入命令 `node`，这时进入 nodejs 的交互模式

![1572678681282](F:%5C%E7%AC%94%E8%AE%B0%5CTypora%E7%AC%94%E8%AE%B0%5CNodeJs.assets%5C1572678681282-1609124321006.png)

#### 文件执行

创建文件 hello.js ，并写入代码 console.log('hello world')，命令行运行 `node hello.js`

快速启动命令行的方法

* 在文件夹上方的路径中，直接输入 cmd 即可
* 使用 webstorm 和 vscode 自带的命令行窗口

![1572680753835](F:%5C%E7%AC%94%E8%AE%B0%5CTypora%E7%AC%94%E8%AE%B0%5CNodeJs.assets%5C1572680753835-1609124321007.png)

#### VScode 插件运行

安装插件 『code Runner』, 文件右键 -> run code

![1593782861500](F:%5C%E7%AC%94%E8%AE%B0%5CTypora%E7%AC%94%E8%AE%B0%5CNodeJs.assets%5C1593782861500-1609124321007.png)

#### 注意

<span style="color:red">在 nodejs 环境下，不能使用 BOM 和 DOM ，也没有全局对象 window，全局对象的名字叫 global</span>



### Buffer(缓冲器)

#### 介绍

Buffer 是一个和数组类似的对象，不同是 Buffer 是专门用来保存二进制数据的

#### 特点

* 大小固定：在创建时就确定了，且无法调整
* 性能较好：直接对计算机的内存进行操作
* 每个元素大小为 1 字节（byte）

#### 操作

##### 创建 Buffer

* 直接创建 Buffer.alloc
* 不安全创建 Buffer.allocUnsafe

* 通过数组和字符串创建 Buffer.from

##### Buffer 读取和写入

可以直接通过 `[]` 的方式对数据进行处理，可以使用 toString 方法将 Buffer 输出为字符串

* ==[ ]== 对 buffer 进行读取和设置
* ==toString== 将 Buffer 转化为字符串

##### 关于溢出

溢出的高位数据会舍弃

##### 关于中文

一个 UTF-8 的中文字符大多数情况都是占 3 个字节

##### 关于单位换算

1Bit 对应的是 1 个二进制位

8 Bit = 1 字节（Byte）

1024Byte = 1KB 

1024KB = 1MB

1024MB = 1GB

1024GB = 1TB

平时所说的网速 10M 20M 100M 这里指的是 Bit ，所以理论下载速度 除以 8 才是正常的理解的下载速度

### 文件系统 fs

fs 全称为 file system，是 NodeJS 中的内置模块，可以对计算机中的文件进行增删改查等操作。

##### 文件写入

* 简单写入
  * fs.writeFile(file, data, [,options], callback);
  * fs.writeFileSync(file, data);
  * options 选项
    * `encoding` **默认值:** `'utf8'`
    * `mode`**默认值:** `0o666`
    * `flag` **默认值:** `'w'`
  
  ##### 
  
  ```js
  //1.引入 fs 模块  固定写法
  const fs = require('fs');
  
  //2.调用fs的方法写入文件          回调函数会在写入文件后自动执行
  fs.writeFile('路径+新建的文件名','文件内容',function(err){
      //err 错误对象  如果写入失败 err为错误的对象结果,如果没有错误 err 的值为null
      if(err){
          console.log('写入错误');
          return ;
      }
      console.log('写入成功');
  })
  fs.writeFile('路径','文件内容',{flag:'a'},回调)
  //其中第三个参数 {falg:'a'} 配置属性 是可选, 
  //写了就是在文件原有内容基础上追加,
  //不写就是将文件内容进行覆盖
  ```
  
  
  
* 流式写入
  * fs.createWriteStream(path[, options])
    * path
    * options
      * ==flags==   **默认值:** `'w'`
      * `encoding `**默认值:** `'utf8'`
      * `mode`   **默认值:** `0o666`
    * 事件监听 open  close  eg:  ws.on('open', function(){});
  
  ```js
  //1.引入  fs 模块
  const fs = require('fs');
  
  //2.床架写入流对象
  const 对象名 = fs.creatWriteStream('路径文件名');
  
  //3.写入内容 写入流对象调用write方法 可以多次写入内容
  对象名.write('文件内容');
  对象名.write('文件内容');
  对象名.write('文件内容');
  //绑定回调  当写入流对象创建成功时触发  open写入
  对象名.on('open',function(){
      console.log('写入流常见成功')
  })
  //绑定写入流关闭事件    close 关闭
  对象名.on('close',function(){
      console.log('写入流关闭');
  })
  //4.在文件内容写入完成后  调用关闭方法
  对象名.close();
  ```
  
  

##### 文件读取

* 简单读取
  * fs.readFile(file, function(err, data){})
  * fs.readFileSync(file)

```js
1.引入 fs 模块
const fs = require('fs');
//2.调用方法      read 读取   File 文件
fs.readFile('路径',function(err,data){
    //判断
    if(err) throw err;//如果有错误,弹出错误
    console.log(data.toString());//将文件内容输出
})
```

> <span style='color:red;'>通过fs.readFild方法获取到的数据内容格式是 Buffer格式 ,需要通过toString()方法解析成字符串</span>

* 流式读取
  * fs.createReadStream();

```js
//1.引入 fs 文件
const fs = require('fs');
//2.创建读取对象
const 对象名 = fs.createReadStream('文件路径');
//3.绑定事件  data数据事件  
//当事件触发时会将读取到的文件块,传递给回调函数,在回调中操作看读取到的文件内容
对象.on('data',function(chunk){
    console.log(chunk.length);
})
// open事件   当读取开始会触发
对象.on('open',() => {
    console.log('读取流打开了')
})
//当 end事件   当读取结束会触发
对象.on('end'.() => {
    console.log('读取流结束了');
})
```

> <span style='color:red;'>通过fs.createReadStresam方法获取到的数据内容格式是 Buffer格式 ,需要通过toString()方法解析成字符串</span>

#### 读取并写入

```jhs
//复制文件
const fs = require('fs');

//创建读取流对象
const rs = fs.createReadStream('./file/刻意练习.mp3');
//创建写入流对象
const ws = fs.createWriteStream('../../致胜法宝.mp3');

//绑定事件
// rs.on('data', chunk => {
//     //将数据写入到目标文件中
//     ws.write(chunk);
// });

//简便操作  pipe 管道
rs.pipe(ws);
```



##### 文件删除

* fs.unlink('./test.log', function(err){});
* fs.unlinkSync('./move.txt');

```js
//1.引入 fs 模块
const fs = require('fs');
//2.调用方法 删除文件   异步操作
fs.unlink('路径+文件名',function(err){
    if(err) throw err;
    console.log('删除成功')
})
// 同步操作
fs.unlinkSync('路径+文件名')
```



##### 移动文件 + 重命名

* fs.rename('./1.log', '2.log', function(err){})
* fs.renameSync('1.log','2.log')

```js
//1. 引入 fs 模块
const fs = require('fs');
//2. 调用方法移动文件  异步操作
fs.rename('目标文件路径','目的地路径+文件名',function(err){});
//同步移动文件
fs.renameSync('目标文件路径','目的地路径+文件名')
```



##### 文件夹操作

* mkdir  创建文件夹
  * path
  * options 
    * recursive 是否递归调用
    * mode  权限 默认为 0o777
  * callback 

```js
const fs = require('fs');
//创建文件夹   异步操作
fs.mkdir('路径/文件夹名',function(err){
    if(err) throw err;
    console.log('创建成功');
})

// 同步创建
fs.mkdirSync('路径/文件名')

//递归式创建文件夹,及内部文件或文件夹
fs.mkdit('D:/a/b/c',{recursive:true},function(err){
    if(err) throw err;
    console.log('创建成功')
})
```



* rmdir 删除文件夹

```js
const fs = require('fs');
// 配置属性:当要删除的目标文件夹不为空时,需要配置该参数  
// {recursive: ture} 递归式删除文件夹及其内部的文件.  默认值为false
fs.rmdir('路径+文件名',[配置属性],function(err){
    if(err) throw err;
    console.log('删除成功');
})
```



* readdir  读取文件夹

```js
//1.引入 fs 模块
const fs = require('fs');
//读取文件  配置属性 {withFileTypes:true} 默认值为false不读取文件类型
fs.readdir('路径+文件名',[配置属性],function(err,data){
    if(err) throw err;
    console.log(data)
})
```

#### 查看目标资源的状态

```js
const fs = require('fs');  //stat   status 缩写
//fs.stat 查看目标资源的状态
fs.stat(__dirname + '/A', (err, stats) => {
    if(err) throw err;
    //打印状态结果
    // console.log(stats);//stats 获取是一个对象,内部保存着各种文件的状态
    //状态  文件的大小, 相关时间
    //类型
    if(stats.isFile()){
        console.log('是一个文件');
    }
    if(stats.isDirectory()){
        console.log('是一个文件夹');
    }
});
```



## nodejs 中路径问题

* 绝对路径(不受 js 文件的存放位置)
* 相对路径   (参照的是执行命令所在的文件夹)
* 解决路径问题
  * __dirname+'文件名'
  * __dirname 是一个特殊的变量,保存的值是当前文件所在目录的绝对路径,不会受到执行命令位置的影响

## nodejs 中的两种操作 (同步和异步)

* Sync  (同步)  方法名后加 Sync就是同步操作
* 写测试demo, 本地的一些文件操作, 可以使用同步 API

* 做服务功能的时候, 需要使用异步 API

## 附录

### unicode 字符集

* https://www.tamasoft.co.jp/en/general-info/unicode.html

* https://www.cnblogs.com/whiteyun/archive/2010/07/06/1772218.html    

### 读取与写入

写入文件场景

1. 下载文件

2. 安装文件

3. 日志(程序日记) 如 Git

4. 数据库

5. 网盘

6. 编辑器保存文件

7. 视频录制

读取文件场景

1. 下载文件

2. 程序运行

3. 数据读取(数据库)

4. 日志 (git log)

5. 编辑

### 写入文件的两种方式选择

\* writeFile       简单文件写入, 写入的次数比较少

\* createWriteStream   多次内容写入, 可以使用createWriteStream

### 读取文件的两种方式选择

* readFile     小文件读取

* createReadStream 大文件读取

### 关于路径分割符

* windows \  

* linux  /

> 编程需要设置路径的时候, 都可以使用 `/`



### vscode 两个设置

* 左侧文件树的缩进 左下角 -> 齿轮 -> 设置 -> 搜索『缩进』 -> 设置值即可

* 设置文件夹的显示 左下角 -> 齿轮 -> 设置 -> 搜索『compact』 -> 取消勾选