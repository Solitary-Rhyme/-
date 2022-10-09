# 02 - JavaScript

[toc]

## JavaScript和html代码的结合方式

**【方式一】**

- 在head标签或body标签中，使用script标签来书写JavaScript代码

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
      <script type = "text/javascript">
          //alert是JS语言提供的一个警告框函数
          //它可以接收任意类型的参数，这个参数就是警告框的提示信息
          alert("hello javaScript");
      </script>
  </head>
  <body>
  </body>
  </html>
  ```

**【方式二】**

- 使用script标签引入单独的JavaScript代码文件

  ```js
  //js文件内
  alert("hello javaScript");
  ```

  ```html
  <!--html文件内-->
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
      <!--script标签可以用来定义代码，也可以用来引入js文件
  		但是两个功能只能二选一，不能同时使用两个功能-->
      <script type = "text/javascript" src="test.js"></script>
      <!--可用通过分两次执行来规避这条限制-->
      <script type = "text/javascript">
      	alert("hiiiii");
      </script>
  </head>
  <body>
  </body>
  </html>
  ```

  

## JavaScript基础

### 1. 变量

#### 1.1 变量基础

- JS是一门弱类型语言，这意味变量的类型不是固定的，变量可以随时从一种类型转换为另一种类型

  ```js
  var i = 1;
  i = "abc";
  alert(i); //输出结果为abc
  ```

- JS有五种类型的变量：

  - number 数值类型(包含了Java中int,float,double等数值类型)
  - string 字符串类型
  - object 对象类型
  - boolean 布尔类型
  - function 函数类型

- JS中特殊的值

  - undefined 未定义：所有js变量未赋予初始值的时候，默认值都是undefined
  - null 空值
  - NaN 非数字，非数值
  
- typeof() 可以用来检查一个变量的类型



#### 1.2 强制类型转换

- **将其它数据类型转换为String**

  方式一：

  - 调用被转换数据类型的toString()方法
  - 该方法不会影响到原变量，它会将转换的结果返回
  - null和undefined这两个值没有toString()方法

  方式二：

  - 调用String()函数，并将被转换的数据作为参数传递给函数
  - 使用String()函数强制类型转换时
    - 对于Number和Boolean实际上就是调用的toString()方法
    - 对于null和undefined，会将null和undefined转换为字符串形式

- **将其它数据类型转换为Number**

  方式一：

  - 使用Number()函数
    - 如果是纯数字的字符串，则直接将其转换为数字
    - 如果字符串中有非数字的内容，则转换为NaN
    - 如果字符串是一个空串或者一个全是空格的字符串转换为0
    - 对于布尔值true转成1，false转成0
    - null转换成0，undefined转换为NaN

  方式二：

  - 这种方式只适用于字符串
  - parseInt() 把一个字符串转换为一个整数
  - parseFloat() 把一个字符串转换为一个浮点数



#### 1.3 关系运算

- `== ` （等于）：等于只会做简单的字面值的比较，比如："12" == 12，返回值为true

  `===` （全等于）：除了做字面值的比较之外，还会比较两个变量的数据类型

  - NaN永远不会等于任何数，即便是它自己。如果要判断NaN可以使用isNaN()函数

- &&（且运算）：当表达式全为真的时候，返回最后一个表达式的值；当表达式中有一个为假的时候，返回第一个为假的表达式的值

  ||（或运算）：当表达式全为假时，返回最后一个表达式的值；只要有一个表达式为真，就会返回第一个为真的表达式的值

  JS中的逻辑运算符也具有短路特性



### 2. 数组

- JS中数组在细节上与Java的有许多不同，比起数组他更像是数组列表或链表

- 数组也是一个对象，用于储存一些值并使用数字来作为索引操作元素

  ```js
  //数组的定义
  var arr_name = []; //空数组
  var arr_name1 = [1,'abc',true]; //显式初始化数组,一个数组中可以同时储存多种类型的变量
  
  //向数组中添加元素
  arr[0] = 10;
  arr[1] = 20;
  
  //调用数组元素
  //调用超出索引范围的元素/未定义的元素并不会报错，只会显示undefined
  console.log(arr[0]);
  
  //数组的扩容
  arr_name[2] = "abc";
  //只要通过数组下标赋值，那么数组就会根据最大的下标值自动地扩容
  //扩容后未定义的元素用undefined初始化
  alert(arr_name.length);	
  ```
  
- **[数组方法](https://www.w3school.com.cn/jsref/jsref_obj_array.asp)**



### 3. 函数

**函数的定义**

```js
//无参函数
function fun(){
	alert("无参函数fun()被调用了");
}
fun();	//调用函数

//带参函数
function fun2(a,b){
	alert("有参函数fun"+(a+b));
}
fun2(1,1);

//带返回值函数
function sum(num1,num2){
	return num1+num2;
}
alert(sum(1,1));
```

注意：Js中不允许重载函数，一旦函数重名，后续书写的函数会直接覆盖掉前面的同名函数



**隐形参数 arguments**

在function函数中有一种不需要定义的，但却可以直接用来获取所有参数的变量，我们管他叫做隐形参数。

隐形参数类似于java的可变参数，隐形参数本身也是一个数组

```js
function fun(){
	alert(arguments.length);
}
fun(a,b,c);
//就算fun是无参函数，arguments依旧会接收从外界传入的参数并存储，只是不会在函数内使用罢了

function sum(num1,num2){
    var result = 0;
    for(var i = 0;i < arguments.length;i++){
        result += arguments[i];
    }
    return result;
}
alert(sum(100,200,300,400));
//最终输出结果为1000，可以看到即便不按照形参输入arguments也会照常计算，这样很危险
```



### 4. 对象

#### 4.1 对象基础

```js
//【方式一】

var obj = new Object();
//定义&修改属性
obj.name = "Jack";
obj.age = 18;
//定义函数
obj.fun = function(){
    alert("姓名:" + this.name + ",年龄:" + this.age);
}

//调用属性
alert(obj.age);	
//调用函数
obj.fun();
//删除属性
delete obj.name;

//【方式二】
var obj = {
    name:"Jack",
    age:18,
    fun:function(){
        alert("姓名:" + this.name + ",年龄:" + this.age);
    }
};

alert(obj.age);
obj.fun();
delete obj.name;
```

#### 4.2 构造函数

- JS中没有构造器，所以要初始化一个对象需要借助构造函数。构造函数和一般函数唯一区别就是，普通函数可以直接调用，构造函数需要用new关键字调用

- 构造函数的执行流程：

  1. 立刻创建一个新的对象
  2. 将新建的对象设置为函数中this，在构造函数中可以使用this来引用新建的对象
  3. 逐行执行函数中的代码
  4. 将新建的对象作为返回值返回

- 使用同一个构造函数创建的对象，称为类对象，构造函数也是一个类

  将通过构造函数创建的对象，称为是该类的实例

  ```js
  function Person(name,age,gender){
      this.name = name;
      this.age = age;
      this.gender = gender;
      //此处的方法设计仍有缺陷，详情见"原型对象"
      this.sayName = function(){
      	alert(this.name);  
      };
  }
  
  var person = new Person("Jack","18","男");
  ```

- 单纯通过该方式创建的构造函数具有巨大缺陷，由于每个对象都是独立的，所以创建10000个对象就会创建10000个相同的，独立的属性和方法，占用很多内存。为了解决这一问题，需要使用原型函数。

#### 4.3 原型对象

- 对于所创建的每一个函数，解析器都会向函数中添加一个属性prototype，这个属性所以对应的就是原型对象

- 如果函数作为普通函数调用prototype，不会有任何作用

  但当函数以构造函数的形式调用时，它所创建的对象中都会有一个隐含的属性。这个属性指向该构造函数的原型对象，我可以通过`__proto__`来访问该属性

- 原型对象就相当于一个公共的区域，所有同一个类的实例都可以访问这个原型对象，可以将对象中共有的内容，统一设置到原型对象中

  以后创建构造函数时，可以将这些对象共有的属性和方法，统一添加到构造函数的原型对象中，这样就可以在不影响到全局作用域的情况下，使每个对象都具有这些属性和方法

  ```JS
  Person.prototype.name = "jack";
  Person.prototype.age = "18";
  Person.prototype.gender = "female";
  Person.prototype.sayName = function(){
    alert("hi," + this.name);  
  };
  ```

  

#### 4.4 其它知识点

**特殊属性值**

- 定义与调用属性，有一种更加灵活的的方法：

  - 定义：`对象["属性名"] = 属性值`

  - 调用：`对象["属性名"]`

  ```js
  var obj = new Object();
  obj["123"] = 789;
  console.log(obj["123"]);
  ```

**in 运算符**

- 通过该运算符可以检查一个对象中是否含有指定的属性，有则返回true，反之返回false

  如果对象中没有但是原型中有也会返回true

  语法：`"属性名" in 对象`
  
  ```js
  console.log("test2" in obj);
  ```

**hasOwnProperty()**

- 可以使用该方法检查对象自身中是否含有该属性，不受原型对象影响

  ```js
  console.log(mc.hasOwnProperty("age"));
  ```

**for...in 语句**

- 枚举对象中的属性，循环次数由属性数量决定，每次执行时会将对象中的一个属性的名字赋值给变量

  语法：`for(var 变量 in 对象){}`

  ```js
  for(var n in obj){
      console.log("属性名:" + n);
      console.log("属性值:" + obj[n]);
  }
  ```


**instanceof**

- 作用和java中的类似，检查一个对象是否是一个类的实例。若是返回true，反之返回false.

  语法：`对象 instanceof 构造函数`

- 所有对象都是Object后代，所以任何对象和Object使用instanceof检查时都会返回true

  ```js
  console.log(dog instanceof Person);
  ```

**toString()**

- 当直接在页面中打印一个对象时，实际上是输出对象toString()方法的返回值
- toString()方法实际存在于类的祖先原型对象中，如果要修改toString()内容，可以通过覆写该方法达成





### 5. 作用域

- JS中的作用域和Java中的略有不同

#### 5.1 全局作用域

- 直接编写在script标签中的JS代码，全都在全局作用域
- 全局作用在页面打开时创建，在页面关闭时销毁
- 在全局作用域中有一个全局对象window
  - window代表的是一个游览器的窗口，由游览器自动创建
  - 创建的变量都会作为window对象的属性保存
  - 创建的函数都会作为window对象的方法保存
- 全局作用域中的变量都是全局变量，在页面的任意部分（包括函数），都可以访问的到

#### 5.2 函数作用域

- 每次调用函数时创建函数作用域，函数执行完毕后销毁
- 函数作用域之间是相互独立的
- 在函数作用域中可以访问到全局作用域中的变量，反之则不行
- 当函数作用域操作一个变量时，它会先在自身作用域中寻找并使用，如果没有，则在上一级作用域中寻找，直到报错

#### 5.3 声明提前

- 变量的声明提前

  - 使用var关键字声明的变量，会在所有的代码执行之前被声明，但是不会被赋值

    如果声明变量时不使用var关键字，则变量不会被声明提前

- 函数的声明提前

  - 使用函数声明形式创建的函数`function 函数(){}`，会在所有的代码执行之前就被创建，所以可以在函数被定义前来调用函数
  - 使用函数表达式创建的函数，不会被声明提前，所以不能在声明前调用

```js
//变量声明提前

console.log(a); //输出undefined，意味着a已被定义尚未赋值

var a = 123;

//函数声明提前

fun1();	//正常调用函数
fun2(); //报错

function fun(){
    console.log("fun函数")
}

var fun2 = function(){
    console.log("fun2函数")
};
```
