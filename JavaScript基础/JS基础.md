JS基础

## 函数相关知识

实参列表 arguments

```js
function test(a){
  // 实参列表
  console.log(arguments)  // [1, 2, 3] 一个类数组
  // 形参长度
  console.log(test.length)	// 1
}
test(1, 2, 3)

```

注意：arguments 和 括号中的形参是一一映射的关系，但不是同一个值



递归

阶乘、斐波那契数列

1. 找规律
2. 找出口

好处：代码简洁

缺点：执行速度慢

```js
function mul(n){
  if(n === 1 || n === 0){
    return 1;
  }
  return n * mul(n - 1);
}
```





## 预编译 => 作用域 => 闭包

### 预编译

JS执行三部曲：

1. 语法分析（通篇扫描）
2. 预编译（预编译四部曲）
3. 解释执行（JS是解释性语言、在解释执行阶段解释一行执行一行）



声明提升

函数声明整体提升

变量声明 声明提升



`Imply global` 暗示全局变量

```js
function test(){
  var a = b = 123;
}
test()

console.log(a)	// undefined
console.log(b)	// 123
```

未经声明就赋值的变量为全局对象`window`所有

所有全局变量为`window`的属性



#### 预编译四部曲

预编译发生在函数时，预编译发生在函数前一刻；

1. 创建AO对象`Activation Object`，即执行期上下文
2. 找形参和变量声明，将变量和形参名作为AO的属性名，值为`undefined`
3. 将实参值和形参相统一
4. 在函数体里面找函数声明，值赋予函数体

```js
// 测试题
// 1.
function fn1(a){
  console.log(a);
  var a = 123;
  console.log(a);
  function a(){}
  console.log(a);
  var b = function b(){}
  console.log(b);
  function d(){}
}
fn1(1);
// 2.
function fn2(a, b){
  console.log(a); 
  c = 0;
  var c;
  a = 3;
  b = 2;
  console.log(b); 
  function b(){}
  function d(){}
  console.log(b) 
}
fn2(1)
// 3.
function fn3(a, b){
  console.log(a); // fn
  console.log(b);	// undefined
  var b = 234;
  console.log(b);	// 234
  a = 123;
  console.log(a);	// 123
  function a(){};
  var a;	
  b = 234;
  var b = function(){}
  console.log(a);	// 123
  console.log(b);	//fn
}
fn3(1);

```

预编译不仅发生在函数内，还发生在全局；

因为全局没有参数，所以全局的预编译是：

1. 创建GO对象（`window`），即执行期上下文
2. 找形参和变量声明，将变量和形参名作为AO的属性名，值为`undefined`
3. 找函数声明，值赋予函数体

```js
// 1. 自己的AO里没有的变量会到上一级找
var global = 100;

function fn(){
  console.log(global);
}

fn();

// 2. 自己的AO里有的变量则会访问自己的
global = 100;
function fn(){
  console.log(global);
  global = 200;
  console.log(global);
  var global = 300;
}

fn();
var global;

```



### 作用域、作用域链

>  当函数执行时会产生一个作用域

1. 作用域（`[[scope]]`）：每个`JavaScrip`t函数都是一个对象，对象中有些属性我们可以访问，但有些不可以，这些属性仅供`JavaScript`引擎存取，`[[scope]]`就是其中一个。`[[scope]]`就是我们所说的作用域，其中存储了执行期上下文的集合。

2. 作用域链：[[scope]]中所存储的执行期上下文的集合，这个集合呈链式链接，我们把这种链式链接叫做作用域链。
3.  执行期上下文（`AO`、`GO`）：当函数执行（前一刻）时，会创建一个称为`执行期上下文`的内部对象。一个`执行期上下文`定义了一个函数执行时的环境，函数每次执行时对应的`执行期上下文`都是独一无二的，所以多次执行一个函数会创建多个`执行期上下文`，当函数执行完毕，它所产生的执行期上下文就会被销毁。
4. 查找变量：从作用域顶端依次向下查找。(后创建的执行期上下文会被放到作用域顶端)

> 也就是说作用域里面存的是作用域链，而作用域链是由一个个执行期上下文(AO,GO)链接而成的链式结构。

原型 => 原型链 => 继承

对象数组操作方法

### 闭包

内部函数被保存到外部时，将会生成闭包。闭包会导致原有作用域链不释放，造成内存泄露。

```js
// 例子1
function a(){
  var num = 100;
  function b(){
    num ++;
    console.log(num)
  }
  
  return b;
}

var demo = a();
demo()
demo()
```

#### 作用

1. 实现共有变量

   ```js
   // 累加器
   function add(){
     var count = 0;
     function demo(){
   		count ++;
       console.log(count);
     }
     return demo;
   }
   
   var counter = add()
   counter();
   counter();
   ```

   

2. 做缓存

   ```js
   function test(){
     var num = 100;
     function a(){
   		num ++;
       console.log(num);
     }
     function b(){
   		num --;
       console.log(num);
     }
     
     return [a, b];
   }
   
   var arr = test();
   arr[0]();
   arr[1]();
   ```

   ```js
   function eater(){
     var food = "";
     var obj = {
   		eat: function(){
   			console.log("eating " + food);
         food = ""
       },
       push: function(food){
   			food = food;
       }
     }
     
     return obj;
   }
   
   var eater1 = eater();
   
   eater1.push("banana")
   eater1.eat()
   ```

   

3. 实现封装，属性私有化

4. 模块化开发，防止污染全局变量



#### 立即执行函数

此类函数没有声明，在一次执行过后即释放。适合做初始化工作。

```js
(function(a, b){
  console.log(a + b)
}(1, 2))
```





## 原型 => 原型链 => 继承

### 原型

1. 定义：原型是`function`对象的一个属性，它定义了构造函数制造出的对象的公共祖先。通过构造函数产生的对象，可以继承该原型的属性和方法。原型也是对象。

2. 利用原型的特点和概念，可以提取共有属性。

   ```js
   // 提取共有属性
   Car.prototype.height = 1400;
   Car.prototype.width = 4900;
   Car.prototypr.carName = "BMW";
   
   function Car(color, owner){
     this.color = color;
     this.owner = owner;
   }
   
   var car1 = new Car()
   var car2 = new Car()
   
   // Car.prototype 是一个对象
   Car.prototype = {
     constructor: Car()
   }
   
   // constructor 可以修改
   ```

   > 实例不能修改原型上的属性

3. 对象如何查看原型：隐式属性`__proto__`

   ```js
   // new
   function Person(){
     // var this = {
     //  __proto__: Person.prototype
     // }
   }
   
   var person = new Person()
   person.__proto__ === Person.prototype
   ```

4. 对象如何查看对象的构造函数：`constructor`