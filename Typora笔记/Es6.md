

## <span style='color:red;'>let 声明</span>

let 声明变量 字母数字下划线($) 首字母不能为数字, 严格区分大小写, 且不能使用关键字(保留字)

#### let  声明特点

1. 不允许重复声明 (不允许重复声明 相同的变量名,)

2. 块级作用域     包含但不限于以下几种情况 

   * if(){ }   
   * else{ }
   * else if(){ }
   * while (){ }
   * for(){ }

3. 变量提升

   使用 let 关键字声明的变量,没有变量提升    必须在声明之后再进行操作

4. 不影响作用域链

## <span style='color:red;'>const 声明常量</span>

值不能改变的变量,称之为 **常量**

1. 使用const声明的变量必须赋初始值
2. 使用const声明的常量名一般全大写(潜规则)
3. 常量的值不能修改
4. 不允许重复声明

## let 和 const 应用场景

let 用来声明基本数据类型

const 用来声明引用数据类型

## 变量解构赋值

将数组和对象中的值或属性解构出来

**数组解构**

```js
const F4 = ['宋小宝','刘能','赵四','小沈阳'];
let [song,liu,zhao,xiao] = F4;
console.log(song) //宋小宝
console.log(liu) //刘能
console.log(zhao) //赵四
console.log(xiao) //小沈阳
//通过解构赋值可以用一个变量保存数组中的值,通过变量访问
```

**对象解构**

```js
let guodegang = {
      name: '郭德纲',
      tags: ['相声','桃','小黑胖子'],
      say: function(){
           console.log('我可以说相声')
      }
};

let {name,say, tags:[xiang, tao, hei]} = guodegang;
console.log(name);//郭德纲
console.log(say);//函数体
console.log(xiang);//相声
console.log(tao);//桃
console.log(hei);//小黑胖子
console.log(tags);//报错 未定义   
//注意:在解构复杂数据类型时,例如tags(数组),在这里的数组不会被解构,而其内部的数据会被解构
```



## 模板字符串

增强版的字符串,可以只接使用换行符,可以与变量直接拼接

语法:   ``     (反引号)

拼接变量:  ${变量名}

```js
//1. 字符串中可以直接使用换行符
        let mahua = `<ul>
                        <li>沈腾</li>
                        <li>艾伦</li>
                        <li>马丽</li>
                        <li>魏翔</li>
                    </ul>`;

        //2. 字符串与变量拼接非常方便
        let star = '魏翔';
        let string = `我特别喜欢 ${star.substr(0,1)} ,他太逗了`;
```

## 简化对象写法

```js
		//声明变量
        let school = '尚硅谷';
        let change = function(){
            console.log('改变自己');
        }

        const obj = {
            //1. 直接使用变量
            school,
            change,
            //2. 方法的声明
            improve(){
                console.log('提升自己');
            }
        }
```

## <span style='color:red;'>箭头函数</span>

在ES6中允许使用箭头(=>)来定义函数,定义函数  箭头中间不能有空格

箭头函数的特点:

1. 箭头函数中的 this 是静态的, 始终指向其外部作用域的 this 的值
2. 箭头函数不能作为构造函数使用
3. 箭头函数中没有  arguments
4. 箭头函数可以简写:
   * 不写小括号,当形参只有一个时,可以不写小括号
   * 不写花括号, 当代码体只有一条语句的时候, 并且语句的执行结果为函数返回值的 (如果不写花括号的话, return 也不能写)

**箭头函数的实例**

```js
//从数组中返回偶数的元素
let arr = [1,2,3,99,55,6,7,8,8,8,6,5,4,43,3];
var result = arr.filter(item => item % 2 === 0);
//filter 方法    是数组的方法  是一个过滤器,通过传递的参数判断返回的值
//filter方法会创建一个新的数组返回, 
//参数: 以一个回调函数为参数,返回true表示该元素通过测试，保留该元素，false则不保留。
```

**<span style='color:red;'>总结</span>**

什么时候适合使用箭头函数?

与this无关的回调设置,可以使用箭头函数,例如:数组方法的回调  定时器  适合使用箭头函数

与this有关的回调设置,不建议使用箭头函数.  例如:  事件的回调  对象中的方法等

## 参数默认值

在ES6 中允许给函数的形参赋初始值

1. 参数直接设置默认值,具有默认值得参数一般放在最后面(非强制)
2. 与解构赋值结合使用   解构赋值的形式先后顺序不受影响

```js
function connect({host='127.0.0.1', port, dbname, user}){
    console.log(host)
    console.log(port)
    console.log(dbname)
    console.log(user)
}
connect({
    port: 27017,
    dbname: 'project',
    user: 'root'
})
```

## rest 参数

这里的rest 参数指的是    ...变量名

rest参数是用来代替  arguments的  和arguments 不同的是rest参数是一个真正的数组

将函数调用时传递的多个实参保存在一个数组当中

```js
function fn(...args){
    console.log(args);
    console.log(arguments);
}
fn(1,2,3,4,5)
//args会输出一个真正的数组,[1,2,3,4,5]
//arguments 输出的是一个伪数组
```

如果有多个形参, rest 参数必须放置在最后

```js
function fn(a,...args){
    console.log(a);//输出1
    console.log(args);// 输出 [2,3,4,5]
}
fn(1,2,3,4,5)
```

## spread 扩展运算符

spread扩展运算符  其实就是  rest  参数的逆运算

将数组或者对象 展开

**数组展开**

```js
let arr = [1,2,3,4,5];
function fn(){
    console.log(arguments);
}
fn(...arr);
//将arr数组中的所有元素,进行展开拆分,作为函数的实参进行传递
```

**对象展开**

```js
const skillOne = {
    q: '天音波',
    Q: '回音击'
};
const skillTwo = {
    w: '金钟罩'
};
const skillThree = {
    e: '天雷破'
};
const skillFour = {
    r: '猛龙摆尾',
    q: 'xxx'
}

const hero = {...skillOne, ...skillTwo, ...skillThree, ...skillFour}; //   ...{q:'天音波',Q: '回音击'}   => q:'天音波', Q: '回音击'
```

**扩展运算符应用**

1. 可以将数组进行合并

   ```js
   let arr1 = [1,2,3];
   let arr2 = [4,5,6];
   let arr3 = [...arr1,...arr2];
   console.log(arr3);
   //输出结果为[1,2,3,4,5,6];
   ```

   

2. 新数组克隆

   ```js
   let arr = [1,2,3,4,5];
   let arr2 = [...arr];
   console.log(arr2);
   //输出结果为 [1,2,3,4,5];
   ```

3. 将伪数组转换为真数组

   ```js
   function fn(){
       console.log([...arguments]);
   }
   fn(1,2,3,4,5);
   ///输出结果为一个真数组[1,2,3,4,5];
   
   //也可以将js中获取到的DOM对象集合转换为真数组
   let boxs = document.querySelectorAll('div');
   console.log([...boxs];
   //输出结果为一个真数组
   ```

## Symbol 

ES6中引入了一个新的原始数据类型 Symbol  用来表示一个独一无二的值

<span style='color:red;'>并且无法与任何值做运算</span>

**创建Symbol**

```js
let s1 = Symbol();
let s2 = Symbol('abc');
let s3 = Symbol('abc');
//注意 s2 和 s3 是两个不同的值,及时长得一模一样
```

**<span style='color:red;'>Symbol 的作用</span>**

用来创建对象独一无二的属性和方法

```js
//创建一个独一无二的key
let upKey = Symbol('upup');
let downKey = Symbol('downdown');
let game = {
    name:'俄罗斯方块',
    //第二种  对象内部添加 Symbol类型的属性
    [upKey]: function(){
        console.log('向上 向上');
    },
    [downKey]: function(){
        console.log('向下 向下');
    }
};
console.log(game);

```

### Symbol 内置属性

Symbol.replace   在对象被替换时自动触发

除了定义自己使用的 Symbol 值以外，ES6 还提供了11个内置的Symbol值，指向语言内部使用的方法。

| Symbol.hasInstance        | 当其他对象使用instanceof运算符，判断是否为该对象的实例时，会调用这个方法 |
| ------------------------- | ------------------------------------------------------------ |
| Symbol.isConcatSpreadable | 对象的Symbol.isConcatSpreadable属性等于的是一个布尔值，表示该对象用于Array.prototype.concat()时，是否可以展开。 |
| Symbol.unscopables        | 该对象指定了使用with关键字时，哪些属性会被with环境排除。     |
| Symbol.match              | 当执行str.match(myObject) 时，如果该属性存在，会调用它，返回该方法的返回值。 |
| Symbol.replace            | 当该对象被str.replace(myObject)方法调用时，会返回该方法的返回值。 |
| Symbol.search             | 当该对象被str. search (myObject)方法调用时，会返回该方法的返回值。 |
| Symbol.split              | 当该对象被str. split (myObject)方法调用时，会返回该方法的返回值。 |
| Symbol.iterator           | 对象进行for...of循环时，会调用Symbol.iterator方法，返回该对象的默认遍历器 |
| Symbol.toPrimitive        | 该对象被转为原始类型的值时，会调用这个方法，返回该对象对应的原始类型值。 |
| Symbol. toStringTag       | 在该对象上面调用toString方法时，返回该方法的返回值           |
| Symbol.species            | 创建衍生对象时，会使用该属性                                 |

## 迭代器

迭代器（Iterator）是一种接口，为各种不同的数据结构提供统一的访问机制。任何数据结构只要部署 Iterator 接口，就可以完成遍历操作。

1. ES6创造了一种新的遍历命令for...of循环，Iterator接口主要供for...of消费

2. 原生具备iterator接口的数据(可用for of遍历)

   原生具有iterator接口的数据类型  Array  Arguments  Set  Map  String  TypedArray  NodeList

3. 工作原理

   * 创建一个指针对象，指向当前数据结构的起始位置
   * 第一次调用对象的next方法，指针自动指向数据结构的第一个成员
   * 接下来不断调用next方法，指针一直往后移动，直到指向最后一个成员
   * 每调用next方法返回一个包含value和done属性的对象

4. <span style='color:red;'>注:需要自定义遍历数据的时候,要想到迭代器</span>

**自定义遍历数据**

在要自定义遍历数据的时候,如果该数据类型没有原生Iterator接口,则需要自己添加一个

```js
<script>
    const team = {
        name: '终极一班',
        members: [
            'xiaoming',
            'xiaoning',
            'xiaotian',
            'knight'
        ],
        //添加迭代器方法
        [Symbol.iterator]: function(){
            //声明一个变量 index
            let index = 0;
            return {
                next: () => {
                    //value 属性的设置
                    let result =  {value: this.members[index]};
                    //done属性的设置
                    if(index < this.members.length-1){
                        result.done = false;
                    }else{
                        result.done = true;
                    }
                    //自增下标
                    index++;
                    //返回最终的对象
                    return result;
                }
            };
        }
    }
//需求 使用 for...of 遍历 team 这个对象, 并且遍历的数据是 members 中的元素
for(let v of team){
    console.log(v);
}
</script>
```

### for ... of  遍历

所有具有Iterator接口的数据都可以使用  for ...of 来进行遍历,

与fon  ...in不同的是  for...of遍历的是值   for ...in遍历的是属性名(索引)

## <span style='color:red;'>Set</span>

ES6 提供了新的数据结构 Set（集合）。它类似于数组，但成员的值都是唯一的，集合实现了iterator接口，所以可以使用『扩展运算符』和『for…of…』进行遍历，

**<span style='color:red;'>注意:</span>**

1. Set的数据类型是"Object"
2. 声明集合时,不能传递     数组 字符串 null  undefined 以外的  数据类型
   * 参数为 数组  会返回一个类似于数组的集合,但数据类型是"Object"
   * 参数为 字符串  会将字符串进行切割,添加到集合中
   * 参数为 null  返回一个空对象
   * 参数为 undefined  返回一个空对象

Set可以将数组去重

集合的属性和方法：

* size     属性 调用方法返回集合的元素个数
* add(新元素)     方法 调用方法增加一个新元素,返回新的集合
* delete(要删除的元素)    方法  调用方法删除集合中的元素,  集合中有该元素则返回ture 反之返回false
* has(元素)      方法   调用方法检查集合中是否包含某个元素,返回值是boolean值
* clear()    方法  调用方法清空集合  返回值为undefined

Set 实践

```js
//声明集合
let a = new Set();
let a2 = new Set([1,2,3,4,3,2,1]);
console.log(a2);//输出{1,2,3,4}

//数组去重
const arr = [1,2,3,4,5,4,3,2,1];
const result = [...new Set(arr)];
console.log(result);//输出 去重后的数组

//交集  输出两个数组中相同的值
let arr1 = [1,2,3,4,5,3,2,1];
let arr2 = [2,3,7,6,8];
//  先把arr1进行数组去重 调用过滤器方法            满足条件为ture 将item放进数组
let result = [...new Set(arr)].filter(item => new Set(arr2).has(item));

//并集  将两个数组合并,去重
//    现将两个数组展开合并成一个数组,再将数组去重,然后展开成一个新数组
let result = [...new Set([...arr, ...arr2])];

//差集  两个数组中不相同的值
let result = [...new Set(arr2)].filter(item => !new Set(arr).has(item));
```



## Map  缓存

ES6 提供了 Map 数据结构。它类似于对象，也是键值对的集合。但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键。Map也实现了iterator接口，所以可以使用『扩展运算符』和『for…of…』进行遍历

Map类似于对象,与对象不同的是,Map中的属性名可以是任意数据类型

Map 的方法与属性

* set(属性名,属性值)    该方法可以为Map的实例,添加属性和方法
* get(属性名)    该方法用来获取Map实例的属性和方法
* delete(属性名)    该方法用来删除指定的元素    该方法有返回值,  删除了指定元素返回true,实例中没有指定元素,则返回false

* has(属性名)    该方法用来检查Map实例中有没有指定的元素,有则返回true,没有则返回false
* clear()     该方法用来清空Map的实例

### <span style='color:red;'>Map的作用,在什么情境下需要使用Map</span>

Map可以用来做缓存

将服务器反馈的数据通过set 方法进行存储.通过get  方法进行访问,减少服务器的访问次数,提高用户体验

## **<span style='color:red;'>class 类</span>**

在ES6语法中定义了类的概念   不过也只是一个语法糖,在底层也是通过  构造函数  来实现的

```js
//定义 class类
class Phone{
    //在类中通过constructor来构造属性
    constructor(brand,color,price){
        this.brand = brand;
        this.color = color;
        this.price = price;
    }
    //对象方法
    call(){
        console.log('我可以打电话');
    }
}
```

**class类 的静态成员(属性)**

通过 static 关键字来为 类  添加静态成员(属性). 这些静态成员只属于这个构造函数对象,不属于其实例化对象

```js
//在class类中使用static关键字声明的属性,称之为静态属性
//因为静态属性只属于这个对象,而其实例化对象是没有的

class Phone{
    static name = '手机';
	static change = function(){
        console.log('改变得了世界')
    }
}
const mi = new Phone();
console.log(mi);//此时实例对象mi,没有name和change属性


```



**class类  继承**

子类通过 extends  关键字来继承父类

语法:  class 子类  extends  父类

```js
//定义 父类
class Phone{
    //在类中通过constructor来构造属性
    constructor(brand,price){
        this.brand = brand;
        this.price = price;
    }
    //对象方法
    call(){
        console.log('打电话');
    }
}

//定义 子类
class SmartPhone extends Phone{
    //构造方法
    constructor(brand,price,pixel){
    	//调用父类的方法
        super(brand,price);
        //初始化子类的成员(属性)
        this.pixel = pixel;
    }
    //子类 方法
    playGame(){
        console.log('可以玩游戏');
    }
}

//实例化对象
const mi = new SmartPhone('小米',1999,'8000w');

```

**get 与 set**

getter  方法

* 语法:   get  属性名(){return  值};
* 不需要自己调用,而是在读取实例对象的对应属性时,自动执行

setter  方法

* 语法  set  属性名(形参){return 值}
* 不需要调用,在设置实例对象的属性时,自动执行

```js
class Computer{
    //直接声明属性
    // price = 7000;
    
    //实例对象方法
    getInfo(){
        //获取当前对象的颜色和价格  this 是指向实例对象的
        return this.jiage + '----' + Computer.color;
    }

    //getter 方法
    get price(){
        return this.jiage
    }
    //setter 方法
    set price(v){
        //将新的属性保存在当前对象中
        this.jiage = v;
    }
    //静态成员也可以使用 getter 和 setter
    static get color(){
        // return '黑色';
        return this.yanse;
    }
    static set color(v){
        this.yanse = v;
    }
}
```



<span style='color:red;'>继承总结</span>

1. class   声明类
2. constructor  定义构造函数初始化
3. extends   继承父类
4. super  调用父类构造方法
   * super()   函数调用不传参数,只会继承父类的方法
   * super()   函数调用并传参数, 继承父类的属性和方法
5. static   关键字    定义静态方法和属性
6. 父类方法可以重写
7. get  和 set   方法

## 数值扩展

**二进制和八进制**

ES6 提供了二进制和八进制数值的新的写法，分别用前缀0b和0o表示。

**数值扩展**

* Number.isFinite()		检测一个数值是否为有限,    返回值为true和false
* Number.isNaN()          检测一个数值是否为NaN
* Number.parseInt()      将字符串转换成 整数
* Math.trunc()            将数字的小树部分抹掉
* Number.isInteger()      判断一个数是否为整数
* Math.pow(2,2)           幂运算   2的2次方

## 对象扩展

1. Object.is 比较两个值是否严格相等，与『===』行为基本一致（+0 与 NaN）
2.  Object.assign 对象的合并，将源对象的所有可枚举属性，复制到目标对象
3.  __ proto __、setPrototypeOf、 setPrototypeOf可以直接设置对象的原型

##  浅拷贝

拷贝就是复制   对象和数组

浅拷贝就是指,修改拷贝后的数组或对象,原数组和对象,会收到影响(主要指数组和对象内的复杂数据类型)

**数组浅拷贝**

1. 直接赋值  
2. concat()   方法        [ ].concat(arr)
3. slice ()    方法          arr.slice(0)
4. 扩展运算符              [...arr]

**对象浅拷贝**

1. Object.assign({ } , 原对象)

## 深拷贝

深拷贝,就是修改拷贝后的数组和对象,对原数组和对象,没有影响

<span style='color:red;'>通过JSON 进行深拷贝,将对象转化为 JSON 格式的字符串(JSON.stringify) 再将字符串转换为对象(JSON.parse)</span>

## 递归实现深拷贝

```js
<script>
    // 深拷贝
    function deepClone(data){
    // 判断数据类型
    let type = getTargetType(data);
    // 初始化
    let container;
    if(type === 'Object'){
        container = {}
    }
    if(type === 'Array'){
        container = []
    }

    // 进行复制
    for(let i in data){
        // 判断数据类型
        let type = getTargetType(data[i]);
        if(type === 'Object'||type === 'Array'){
            container[i] = deepClone(data[i]);
        }else{
            container[i] = data[i];
        }
    }
    return container;
}

//目标数据
const school = {
    name: '尚硅谷',
    pos: [{
        name: '北京'
    }, {
        name: '上海'
    }, '深圳', '武汉'],
    founder: {
        name: '刚哥',
        age: 45
    },
    //缺陷 不能复制方法
    improve() {
        console.log('提升自己');
    }
};
const obj = deepClone(school);
console.log(obj);
// 判断目标类型
function getTargetType(data){
    return Object.prototype.toString.call(data).slice(8,-1);
}
</script>
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



