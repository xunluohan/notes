# Es5 补充  严格模式

## 介绍

ES5 除了正常运行模式（又称为混杂模式），还添加了第二种运行模式："[严格模式](<https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Strict_mode>)"（strict mode）。

严格模式顾名思义，就是使 JavaScript 在更严格的语法条件下运行。

## 严格模式  作用

1.  消除 JavaScript 语法的一些不合理、不严谨之处，减少一些怪异行为
2.  消除代码运行的一些不安全之处，保证代码运行的安全
3.  为未来新版本的 JavaScript 做好铺垫

## 严格模式声明

1. 全局

   'use starct'   //use  使用        starct  严格

   <span style = "color:red;">在全局编写    'use starct'    代码立即进入严格模式</span>

2. 函数内部

   <span style = "color:red;">在函数内部首行编写  'set starct'   代码,在当前函数内部立即进入严格模式</span>

## 严格模式的特性

1. 不允许使用未声明的变量

2. 函数内部的  this  不指向  window  ,this为undefined

3. eval 作用域 

   eval  是一个函数  接收字符串类型参数, 对字符串内容进行JS语法解析并执行
   
   ```javascript
           eval('alert("圣诞节快乐")');
           eval('var a = 100;var b = 200;console.log(a)');
   		//在eval函数外部无法访问,其内部的变量
           console.log(a, b);
   
   ```
   
4. 对象不能有重复的属性

5. 严格模式下  函数不允许有同名的形参

6. 新增一些保留字    

   * private   私有的
   * protected   受保护的
   * implements  实现

## 严格模式  语法和行为改变

* 必须用 var 声明变量，不允许使用未声明的变量
* 禁止自定义的函数中的 this 指向 window
* 创建 eval 作用域
* 对象不能有重名的属性（Chrome 已经修复了这个 Bug，IE 还会出现）
* 函数不能有重复的形参
* 新增一些保留字, 如: implements interface private protected public

# Object 扩展方法

## <span style='color:red;'>Object.ccreate   以指定对象为原型  创建新对象</span>

Object.create 方法可以以指定对象为原型创建新的对象，同时可以为新的对象设置属性, 并对属性进行描述

* value : 指定值
* writable : 标识当前属性值是否是可修改的, 默认为 false
* configurable：标识当前属性是否可以被删除 默认为 false
* enumerable：标识当前属性是否能用for in 枚举 默认为 false
* get:   当获取当前属性时的回调函数
* set:   当设置当前属性时

```javascript
//以指定对象为原型 创建新对象
        var car = {
            name: '汽车'
        };
        var dazhong = Object.create(car, {
            //品牌属性   属性值必须是对象
            brand: {
                //设置该属性的属性值
                value: '大众',
                //设置该属性是否可以修改
                writable: false,
                //设置该属性是否可以删除
                configurable: false,
                //设置该属性是否可以枚举(遍历)
                enumerable: false
            },
            //价格属性
            price: {
                //getter 函数
                // 当获取对象中的 price 属性的时候, get 方法将会自动执行, 并将 return 的值作为 price 的属性值
                get: function(){
                    // console.log('price属性被获取了')
                    return 280000;
                },
                //setter 函数
                //当对 price 属性进行赋值的时候, 会自动执行
                set: function(v){
                    console.log('price 属性被修改了, 新的值为'+v);
                }
            }
        });
```



## Object.defineProperties(object, descriptors)

直接在一个对象上定义新的属性或修改现有属性，并返回该对象。

* object     要操作的对象
* descriptors     属性描述
  * get  作为该属性的 getter 函数，如果没有 getter 则为undefined。函数返回值将被用作属性的值。
  * set  作为属性的 setter 函数，如果没有 setter 则为undefined。函数将仅接受参数赋值给该属性的新值。

```javascript
var car = {
            brand: '五菱宏光'
        }

        // car.price = 50000;
        Object.defineProperties(car, {
            price: {
                value: 60000
            },
            xiaoliang: {
                get: function(){
                    //this的值 指向要添加属性的这个对象
                    console.log(this);
                    return 50000;
                },
                set: function(){

                }
            }
        });
```

#### <span style='color:red;'>this 指向 要添加属性的这个对象</span>

## 改变this指向       call  -  apply  -  bind

* call 方法使用一个指定的 this 值和单独给出的一个或多个参数来调用一个函数

* apply 方法调用一个具有给定 this 值的函数，以及作为一个数组（或类似数组对象）提供的参数

* bind 同 call 相似，不过该方法会返回一个新的函数，而不会立即执行

### call  方法

用来修改  this  指向

> 语法:    对象(函数).call(第一个参数:this要指向的对象(函数)  ,  后面的参数作为实参传递,多个参数使用逗号隔开)

**注:调用 call 方法  会立即执行函数**

### apply  方法

用来修改  this  指向

>语法和call 方法一样 , 只不过传递参数是需要使用数组的方式传递实参

###bind  方法

用来修改  this  指向

bind 也是一个方法,与coll和apply不同的是  

调用 bind 方法不会立即执行,而是会返回一个函数,  通过调用bind的返回值来执行代码







