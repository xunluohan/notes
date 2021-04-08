# MongoDb 数据库

## 数据库



## 数据库 三个重要的概念

* 一个服务可以创建多个数据库
* 数据库（database） 数据库是一个仓库，在仓库中可以存放集合
* 集合（collection）    集合类似于JS中的数组，在集合中可以存放文档
* 文档（document）  文档数据库中的最小单位，类似于 JS 中的对象，在 MongoDB 中每一条数据都是一个 JS 的对象





## 基本指令

#### 数据库集合命令

* 显示所有的数据库

```
show dbs
show databases
```

* (创建) 切换到指定的数据库

```
use 数据库名
```

* 显示当前所在的数据库

```
db
```

* 删除当前数据库(先切换在删除)

```
use project_1
db.dropDatabase()
```

* 显示当前数据库中所有的集合

```
show collections  
```

* 创建集合

```
db.createCollection('users');
```

* 删除当前集合

```js
db.collection.drop()
```

* 重命名集合

```
db.collection.renameCollection('新的集合名')
```

**<span style="color:red">注 : 操作集合时,如果集合不存在则会自动创建集合</span>**



## 命令行操作数据库

#### 条件控制

##### 运算符

在 mongodb 不能 > < >=  <= !== 等运算符，需要使用替代符号

* `>`   使用 `$gt`   greater  than
* `<`   使用 `$lt`   less than
* `>=`   使用 `$gte` 
* `<=`   使用 `$lte`
* `!==`   使用 `$ne`

##### 逻辑或

`$in` 满足其中一个即可 

```
db.students.find({age:{$in:[18,24,26]}}) //  /[aedf]/  
```

`$or` 逻辑或的情况

```js
db.students.find({$or:[{age:18},{age:24}]});
```

`$and` 逻辑与的情况

```
db.students.find({$and: [{age: {$lt:20}}, {age: {$gt: 15}}]});
```

##### 正则匹配

条件中可以直接使用 JS 的正则语法

```js
db.students.find({name:/imissyou/});
```



#### 增

* 添加 一条数据 `insert()` 

```
db.集合名.insert() //用来增加一条文档  参数为一个json对象
```

* 导入 多条数据

```
mongoimport --db user --collection scoreList --file D:\201022\Lession03\data --drop
--db:指定数据库
--collection:指定集合
--file:指定导入的文件
--drop:可选项，如果使用该项则先删除集合然后再导入，如果不使用该项则会追加
```

****

#### 删

* 删除当前库 指定集合

```js
db.集合名.drop()
```

* **<span style="color:red">删除数据库   注意:在操作前要将数据库进行备份</span>**

```
db.dropDatabase()// 删除当前数据库
```

* remove()  删除数据

```
// 可以根据条件删除数据
传参方式 和 find 一样
```



****

#### 改

* 修改数据 update()
  * 需要两个参数
    * 参数1:  查找数据的条件
    * 参数2:  指定修改的内容
    * 参数3:   是否允许修改多条  或  找不到内容时向集合中添加

```
// 将该数据,完全替换
update({userName:"张三"},{age:1344})

// 修改指定属性
update({userName:'张三'},{$set:{age:12}})//$set 表示只修改指定的属性

// 是否允许修改多条 multi  默认值为false只修改一条 , 值为true 则修改多条  
update({userName:'张三'},{$set:{age:12},{multi:true}})

// 当找不到数据时,将更新的内容作为添加的内容 
.update( {userName:"钱九"} , {age:90} , {upsert:true} )

// 递增 和 递减  $inc 设置指定属性可以进行 递增(+10) 或 递减(-10)
update({userName:"张三"},{$inc:{age:-10}})
```

****

#### 查

* 查看当前数据库下的 指定集合下的 所有数据  find()

``` 
db.集合名.find()
```

* 统计 集合中 文档的数量(条数) **<span style="color:red"> 也可以传递参数,返回满足条件的数据条数</span>**

```
db.集合名.count()
```

* **<span style="color:red">根据条件查找文档</span>**

```
// 参数为空对象 或 不传参 为无条件搜索全部数据
find({})

// 根据指定属性 查找数据 //参数为一个对象
find({属性名:属性值})

// 查找符合条件的多条数据
find({属性名:属性值})// 注意: 如果属性名中包含了特殊字符`.`,则属性名需要加引号

// 根据正则匹配符合条件的多条数据
find({属性名:/正则表达式/})

// 根据多个条件进行搜索
find({属性名:属性值,属性名:属性值})// 将多个条件使用逗号隔开

// 判断条件  进行搜索
/*
	参数为一个对象, 对象中 判断属性值  进行搜索
	值为对象形式
	同一属性名下,传递多个条件时,需要使用逗号隔开  
	例如: 大于10小于20  find({age:{$gt:12,$lt:50}})
	$gt:  大于
	$lt:  小于
	$gte: 大于等于
	$lte: 小于等于
	$ne:  不等于
	$in:  等于  // 可以传一个数组,搜索满足多个条件的数据
			 find({age:{$in:[12,32,21]}})
	$or:  或 find({$or:[{age:32},{sex:"男"}]})
	$and: 与 find({$and:[{age:32},{sex:"男"}]})
*/
find({
	age:{$gt:10,$lt:20}
})
```

* 查找满足条件的一条记录 如果满足条件的有多条,则返回第一个满足条件的数据

```
findOne();
```

* 投影  将满足条件的数据 的 指定属性显示出来

```
find({sex:"男"},{age:1,_id:false})
/*
	参数1: 一个对象,搜索条件, 根据条件进行搜索数据
	参数2: 一个对象,根据对象中属性名的值来判断是否显示该属性
			值为true 只显示该属性
			值为false 不显示该属性
*/
```

* 根据 js 函数 进行判断     通过$where进行条件搜索

```
// 通过$where进行条件搜索
find({
	$where:function(){
		return this.age===32 && this.sex==="男"
	}
})
```

* 排序

```
// 将数据进行排序, 参数为一个对象,根据执行属性名排序,值来指定排序的方向
// 也可以同时指定多个属性,使用逗号隔开
	当第一个属性排序后,值相同,则根据第二个属性进行排序,以此类推
find().sort({
	age:1   // 1为升序从小到大   -1 为降序从大到小
})
```

* 截取数据  截取指定的数据条数

```
find().limit()  值为数字,0表示不截取,
```

* 跳过数据  跳过指定数据的条数

```
find().skip()   值为数字,0表示不跳过
```

* find  sort  limit  skip  结合使用  **<span style="color:red">实现分页功能</span>**

```
//当结合使用时,执行顺序永远为,
find ==> sort ==> limit ==> skip
查找 ==> 排序 ==> 跳过 ==> 截取
不以书写顺序执行
```

****

#### 其他

* 挂载数据库

```
mongod --dbpath 文件绝对路径
mongod --dbpath E:/mongodb
```

* 进入mongo环境

```js
mongo
```

* 查看数据库列表

```
show dbs
```

* 切换 数据库  即使没有该数据库也可以进入,当添加数据时,会自动创建

```
use 数据库名
```

* 查看当前数据库中 集合列表

```
show collections   // collection 集合的意思
```

* 查看当前所在的 数据库

```
db
```

* 创建集合

```
db.createCollection('集合名')  //参数为字符串
```

********

## Mongoose 操作数据库

### mongoose 介绍

Mongoose 是一个对象文档模型（ODM）库，它对Node原生的MongoDB模块进行了进一步的优化封装，并提供了更多的功能。 官网 <http://www.mongoosejs.net/>

### 作用

使用代码操作 mongodb 数据库

### 使用流程

一、安装 mongoose

在命令行下使用 npm 或者其他包管理工具安装（cnpm  yarn）

```sh
npm install mongoose --save     -S
```

二、引入包

在运行文件中引入 mongoose

```js
var mongoose = require('mongoose');
```

三、连接数据库

```js
mongoose.connect('mongodb://127.0.0.1/data');

//如果启动时遇到警告提醒， 则按照提示增加选项即可
mongoose.connect('mongodb://127.0.0.1/data', {useNewUrlParser: true, useUnifiedTopology: true});
```

四、监听连接事件

```js
mongoose.connection.on('open', function () {
	
	//下面编写数据库操作代码
    
    //五、创建文档结构
    var SongSchema = new mongoose.Schema({
        title: String,  //歌名
        author: String  //歌手
    });
    
    //六、创建文档模型
    var SongModel = mongoose.model('songs', SongSchema);
    
    //七、使用模型进行文档处理（这里以增加数据为例）
    SongModel.create({title:'野狼disco',author:'宝石gem'}, function(err,data){
        if(err) throw err; //这里判断错误
        
        //下面编写创建成功后的逻辑
        // ... ...
        
        //八、关闭数据库连接（可选，代码上线之后一般不加）
        mongoose.connection.close();
    });
	
});
```

### 数据类型

文档结构可选的字段类型列表

- ==String==
- ==Number==
- Date
- Buffer
- Boolean
- Mixed   任意类型（使用 mongoose.Schema.Types.Mixed 设置）
- ObjectId
- ==Array==



### CURD  增 删 改 查

#### 增

* 插入一条  **<span style="color:red">create()</span>**

```javascript
create({},collback)
// 参数1: 要插入的数据, 以对象的形式插入
// 参数2: 回调函数,  
/*
	回调函数的参数
		参数1: 错误信息
		参数2: 插入后的数据对象
*/
SongModel.create({
    title:'给我一首歌的时间',
    author: 'Jay'
}, function(err, result){
    //错误
    console.log(err);
    //插入后的数据对象
    console.log(data);
});
```

* 批量插入  **<span style="color:red">insertMany()</span>**

```js
SongModel.insertMany([
    {
        title:'给我一首歌的时间',
        author: 'Jay'
    },
    {
        title:'爱笑的眼睛',
        author: 'JJ Lin',
    },
    {
        title:'缘分一道桥',
        author: 'Leehom Wang'
    }
], function(err, data){
    console.log(err);
    console.log(data);
});
```



****

#### 删

* 删除一条数据  **<span style="color:red">deleteOne()</span>**

```js
// 参数1: 查询数据条件
// 参数2: 回调函数
model.deleteOne({_id:'6002cddaf86f2907cc097916'},function(err,result){
    if(err) console.log(111,err)
    else console.log(222,'删除成功',result)
})
```

* 删除多条数据  **<span style="color:red">deleteMany()</span>**

```js
model.deleteMany(查询数据条件,function(err,result){
    //err  错误信息
    // result  删除结果
})
```





****

#### 改

* 修改一条数据  **<span style="color:red">update()</span>**

```js
// 修改一条 修改满足条件的一条数据,  如果满足条件的数据有多个,则只修改第一个
// 参数1: 筛选条件对象
// 参数2: 修改的属性(以对象的形式传递)
// 参数3: 回调函数 参数1: 报错新信息, 参数2: 结果
model.updateOne({_id:"6002cddaf86f2907cc097916"},{
    	userName:'lala'
	},
    function(err,result){
        if(err) console.log(err)
        else console.log(result)
    })
```

* 修改多条数据  **<span style="color:red">updateMany()</span>**

```js
// 更新多条 uptateMany 方法修改满足条件的多条信息
// 参数1: 筛选条件对象
// 参数2: 修改的属性(以对象的形式传递)
// 参数3: 回调函数 参数1: 报错新信息, 参数2: 结果
model.updateMany({age:2},{age:130},function(err,result){
    if(err) console.log(111,err)
    else console.log(222,result);
})
```



****

#### 查

* 获取所有数据  以数组形式展现  **<span style="color:red">find()</span>**

```js
model.find({},function(err,result){
    //err 报错信息
    //result 所有数据的集合  是一个数组,即使数据只有一条
})
```

* 获取一条数据 一对象形式展现 **<span style="color:red">findOne()</span>**

```js
model.findOne({},function(err,result){
    console.log(result);// result 为查询到的数据
})
```

* 通过id获取一条数据  **<span style="color:red">findById()</span>**

```js
// 与其他不同  第一个参数:  是字符串格式
model.findById('6002cf148c2ade2b800eb78a',function(err,result){
    console.log(result)
})
```

* 投影 将查询到的数据中的指定属性显示出来  **<span style="color:red">select()  返回的数据需要使用exec() 方法来获取</span>**

```javascript
scoreModel.find().select({age:true,_id:false}).exec(function (err,result) {
    if(err) console.log(err);
    else console.log(result);
})

```

* 排序  显示  跳过  结合使用   **<span style="color:red">limit(显示条数)  skip(跳过条数) sort(排序)</span>**

```javascript
scoreModel.find().sort({
    age:-1,
    sex:1
}).skip(1).limit(2).exec(function(err,result){
    if(err) console.log(err);
    else console.log(result)
})
```





****



## 连接数据库

#### 通过mongoose 对象的方法连接数据库

```js
// 1. 下载 mongoose 模块  npm i mongoose -S
// 2. 引入 mongoose 模块
const mongoose = require('mongoose');
// 3. 通过 mongoose 对象的 connect()  方法连接
// 需要两个参数
/**
 * 第一个参数:'mongodb://127.0.0.1:27017/数据库的名字'
 * 第二个参数: 一个对象,设置 字符串解析/监视引擎
 * 第三个参数: 回调函数 需要一个err作为参数,用来接收错误信息
 */
mongoose.connect('mongodb://127.0.0.1:27017/xunluohan',{
    useNewUrlParser: true,//使用最新的字符串解析
    useUnifiedTopology: true// 使用最新的监视引擎
},function(err){
    if(err) console.log(err);//连接失败
    else console.log('连接成功');//连接成功
})
```

#### 通过 Promise 方式连接数据库

```js
// 1. 下载 mongoose 模块  npm i mongoose -S
// 2. 引入 mongoose 模块
const mongoose = require('mongoose');
// 3. 通过 mongoose 对象的 connect()  方法的返回值进行连接
// 需要两个参数
/**
 * 第一个参数:'mongodb://127.0.0.1:27017/数据库的名字'
 * 第二个参数: 一个对象,设置 字符串解析/监视引擎
 * 第三个参数: 回调函数 需要一个err作为参数,用来接收错误信息
 */
const con = mongoose.connect('mongodb://127.0.0.1:27017/xunluohan'{
    useNewUrlParser: true,
    useUnifiedTopology: true
})
// 4. 通过con对象的 then 方法监听是否连接成功
// 4. 通过con对象的 catch 方法监听是否连接失败
con.then(()=>{
    console.log('连接成功')
}).catch(()=>{
    console.log('连接失败')
})
```

#### 通过 事件 获得结果

```js
const mongoose = require("mongoose");
// connect是异步操作。
// 连接数据库成功以后，通过事件得到结果。
mongoose.connect("mongodb://127.0.0.1:27017/userList",{
    // 使用最新的字符串解析。
    useNewUrlParser:true,
    // 服务器发现和监视引擎
    useUnifiedTopology:true
})
// 连接成功以后触发。。
mongoose.connection.on("open",function () {
    console.log("连接成功")
})
// 连接数据库失败以后触发。
mongoose.connection.on("error",function () {
    console.log("连接失败")
})

```



# 附录

## 数据库配置密码

为了隐私和安全考虑,应该给数据库进行设置密码,设置访问数据库的权限(删除等)

1. 挂载数据库

```
mongod --dbpath E:/mongodb(文件路径) --auth
```

2. 重新打开一个命令窗口  进入mongo环境

```
mongo
```

3. 创建用户,设置密码与权限

```
use admin  //进入admin库
db   // 创建用户
db.createUser({user:"用户名",pwd:"密码",roles:{"root"}}) 
// roles 用来设置权限
// root 表示根权限,所有权限
```

4. 登录用户

```
// 命令行登录
db.auth("用户名","密码") //  当登录后显示1 为登陆成功

// 服务端登录并连接数据库  authSource账号和密码所存储的库    表示存储在admin库中
mongoose.connection.on('mongodb://账号:密码@127.0.0.1:27017/数据库名?authSource=admin')

```























