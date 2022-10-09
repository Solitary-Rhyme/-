# 02 - JavaScript

[toc]

### 1. DOM模型

- DOM（文档对象模型）就是把文档中的标签，属性，文本转换成对象来管理（类似于Java中的对象）

  - document管理了所有的HTML文档内容
  - document是一种树结构的文档，有层级关系
  - document让我们把所有的标签都对象化
  - 我们可以通过document访问所有的标签对象

- 对于一段代码来说，它的Document文档树内存结构可能如下：

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <title>Title</title>
  </head>
  <body>
  <a href="https://www.baidu.com/">我的连接</a>
  <h1>我的标题</h1>>
  </body>
  </html>
  
  <!--
  根元素:<html>
  	元素:<head>
  		元素:<title>
  			文本:"文档标题"
  	元素:<body>
  		元素:<a>
  			属性:href
  			文本:"我的连接"
  		元素:<h1>
  			文本:"我的标题"
  -->
  ```

- **Document对象中的四大方法**

  - **document.getElementById(elementId)：**通过标签的id属性查找标签dom对象，elementId是标签的id属性值
  - **document.getElementsByName(elementName)：**通过标签的name属性查找标签dom对象，elementName是标签的name属性值
  - **document.getElementsByTagName(tagName)：**通过标签名查找标签dom对象，tagname是标签名
  - **document.createElement(tagName)：**通过给定的标签名，创建一个标签对象。tagName是要创建的标签名

#### 1.1 文档的加载

- 游览器在加载一个页面时，是按照自上而下的顺序加载的，读取到一行就运行一行

  这就是为什么不能将script标签直接写到head部分，因为在代码执行时，页面和DOM对象都没有加载

- 根据书写位置，script标签可以分为两种形式

  **【方式一】**

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <script>
      	/*onload事件会在整个页面加载完成后才触发
  		为window绑定一个onload事件，该事件对应的响应函数将会在页面加载完成后执行
  		这样就可以确保我们的代码执行时所有的DOM对象已经加载完毕了*/
          window.onload = function(){
              //.....
          }
      </script>
  </head>
  <body>
  </body>
  </html>
  ```

  **【方式二】**

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
  </head>
  <body>
      <script>
          //body内随便写
      </script>
  </body>
  </html>
  ```

#### 1.1 document.getElementById(elementId)

- getElementById在前面已经使用过了，这里以一个实际实现来拓展一下

  ```html
  <!--
  实现要求:用户输入用户名，用户名必须由字符数字下划线组成，长度是5-12位。校验按钮判断用户名是否合法
  -->
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
      <script type="text/javascript">
          function onclickFun(){
              var usernameObj = document.getElementById("username");
              var usernameText = usernameObj.value;
              var usernameSpanObj = document.getElementById("usernameSpan");
  
              //字符串必须由字符数字下划线组成，长度是5-12位
              var patt = /^\w{5,12}$/;
              if(patt.test(usernameText)){
                  //设置span字段所显示的信息
                  usernameSpanObj.innerHTML = "用户名合法";
              }else{
                  usernameSpanObj.innerHTML = "用户名不合法";
              }
          }
      </script>
  </head>
  <body>
  用户名：<input type="text" id="username" />
  <span id="usernameSpan"></span>
  <button onclick="onclickFun()">校验</button>
  </body>
  </html>
  ```

#### 1.2 document.getElementsByName(elementName)

- document.getElementsByName()是根据指定的name属性查询返回多个标签对象集合。这个集合和数组一样，集合中每个元素dom对象，集合中元素的顺序是他们在html页面中从上到下的顺序

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
      <script type="text/javascript">
          function checkAll(){
              var checkObj = document.getElementsByName("hobby");
              for(var i = 0;i < checkObj.length;i++){
                  checkObj[i].checked = true;
              }
          }
          function checkNot(){
              var checkObj = document.getElementsByName("hobby");
              for(var i = 0;i < checkObj.length;i++){
                  checkObj[i].checked = false;
              }
          }
          function checkReverse(){
              var checkObj = document.getElementsByName("hobby");
              for(var i = 0;i < checkObj.length;i++){
                  if(checkObj[i].checked == true){
                      checkObj[i].checked = false;
                  } else{
                      checkObj[i].checked = true;
                  }
              }
          }
      </script>
  
  </head>
  <body>
      兴趣爱好:
      <input type="checkbox" name="hobby" value="cpp">C++
      <input type="checkbox" name="hobby" value="java">Java
      <input type="checkbox" name="hobby" value="js">JavaScript
      <br/>
      <button onclick="checkAll()">全选</button>
      <button onclick="checkNot()">全不选</button>
      <button onclick="checkReverse()">反选</button>
  </body>
  </html>
  ```

#### 1.3 document.getElementsByTagName(tagName)

- document.getElementsByTagName()是根据指定标签名来进行查询并返回集合；其余的属性和document.getElementsByName差不多，具体使用可以参考上例

- **【注意事项】**

  document对象的三个查询方法，如果有id属性，优先使用getElementById方法来进行查询。如果没有id属性，则优先使用getElementsByName方法来进行查询。如果id属性和name属性都没有最后再按标签名查getElementsByTagName。

  以上三个方法，一定要在页面加载完成后执行，才能查询到标签对象

#### 1.4 节点的常用属性和方法

- 粗略的讲，节点就是标签对象

- **方法：**通过具体的元素节点调用

  - getElementsByTagName()：获取当前节点的指定标签名
  - appendChild(oChildNode)：可以添加一个子节点，oChildNode是要添加的孩子节点

- **属性**

  childNodes：获取当前节点的所有子节点

  - childNodes属性会获取包括文本节点在内的所有节点。也就是说标签之间的空白/换行也会算做节点
  - children属性可以获取当前元素的**所有子元素**
  
  firstChild：获取当前节点的第一个子节点
  
  - 存在和childNodes类似的问题
  - firstElementChild属性可以获取当前元素的**第一个子元素**

  lastChild：获取当前节点的最后一个子节点

  - 存在和childNodes类似的问题
  - lastElementChild属性可以获取当前元素的**最后一个子元素**
  
  parentNode：获取当前节点的父节点
  
  nextSibling：获取当前节点的下一个节点
  
  - 存在和childNodes类似的问题
  - nextElementSibling属性可以获取当前元素的**下一个兄弟元素**
  
  previousSlibling：获取当前节点的上一个节点
  
  - 存在和childNodes类似的问题
  - previousElementSibling属性可以获取当前元素的**上一个兄弟元素**
  
  InnerHTML：表示获取/设置起始标签和结束标签中的*内容*
  
  - 对于自结束标签，这个属性没有意义。如果需要读取元素节点属性，直接使用`元素.属性名`
  - 特殊的，对于class属性不能使用`元素.class`，而是因该使用`元素.className`
  
  InnerText：表示获取/设置起始标签和结束标签中的*文本*，即不会返回html格式
  
  querySlector
  
  body：获取body标签
  
  html：获取html根标签
  
  all：获取所有标签

#### 1.5 document.createElement(tagName)

- document.createElement()通过给定的标签名，创建一个标签对象。tagName是要创建的标签名

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
      <script type="text/javascript">
          //必须要放在加载完成事件内，否则会在加载完成前执行语句，就会无法读取到对象对象标签
          window.onload = function(){
              //创建div标签对象
              var divObj = document.createElement("div");
              //设置标签内容
              divObj.innerHTML = "测试"
              //添加对象到body内
              document.body.appendChild(divObj);
          }
      </script>
  </head>
  <body>
  </body>
  </html>
  ```

  

### 2.事件

- **事件**是电脑输入设备与页面进行交互的响应。

  常见的事件：

  - onload 加载完成事件：页面加载完成后，对页面js代码进行初始化操作
  - onclick 单击事件：用于按钮的点击响应操作
  - onblur 失去焦点事件：用于输入框失去焦点后验证其输入内容是否合法
  - onchange 内容发生改变事件：用于下拉列表和输入框内容发生改变后的操作
  - onsubmit 表单提交事件：用于表单提交前，验证所有表单项是否合法

- **注册**就是告诉游览器，当事件响应后要执行哪些操作代码，这一过程叫做事件注册或事件绑定

  注册也分为两种：

  - 静态注册事件：通过html标签的事件属性直接赋予事件响应后的代码，这种方式叫静态注册
  - 动态注册事件：先通过js代码得到标签的dom对象，然后再通过dom对象.事件名 = function(){}这种形式赋予事件响应后的代码。这种方式叫动态注册

#### 2.1 onload事件

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type = "text/javascript">
        function onloadFun(){
            alert("静态注册onload事件")
        }
        window.onload = function(){
            alert("动态注册onload事件")
        }
    </script>
</head>
<body onload="onloadFun();">
</body>
</html>

<!--
由运行结果可以看出，静态注册的事件需要在body内显式调用，而动态注册的不用
同时，onload事件只能从静态和动态注册中二选一
-->
```

#### 2.2 onclick事件

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type = "text/javascript">
        function onclickFun(){
            alert("静态注册onclick事件");
        }

        window.onload = function(){
            //根据id属性获取一个标签对象
            var btnObj = document.getElementById("btn01");
            //由标签对象调用函数
            btnObj.onclick = function(){
                alert("动态注册onclick事件");
            }
        }
    </script>
</head>
<body>
<button onclick="onclickFun()">静态注册</button>
<button id="btn01">动态注册</button>
</body>
</html>
```

#### 2.3 onblur事件

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type = "text/javascript">
        function onblurFun(){
            alert("静态注册onblur事件");
        }

        window.onload = function(){
            var passwordObj = document.getElementById("password");
            passwordObj.onblur = function(){
                alert("动态注册onblur事件");
            }
        }
    </script>
</head>
<body>
用户名:<input type="text" onblur="onblurFun();"><br/>
密码:<input id="password" type="text"><br/>
</body>
</html>
```

#### 2.4 onchange事件

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type = "text/javascript">
        function onchangeFun(){
            alert("静态注册onchange事件");
        }

        window.onchange = function(){
            var listObj = document.getElementById("list");
            listObj.onchange = function(){
                alert("动态注册onchange事件");
            }
        }
    </script>
</head>
<body>
选择列表A
<select onchange="onchangeFun()">
    <option>--请选择--</option>
    <option>AAA</option>
    <option>BBB</option>
    <option>CCC</option>
</select><br/>
选择列表B
<select id="list">
    <option>--请选择--</option>
    <option>AAA</option>
    <option>BBB</option>
    <option>CCC</option>
</select>
</body>
</html>
```

#### 2.5 onsubmit事件

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type = "text/javascript">
        function onsubmitFun(){
            alert("静态注册onsubmit事件");
            return false;
        }

        window.onsubmit= function(){
            var formObj = document.getElemntById("form01");
            formObj.onsubmit = function(){
                alert("动态注册onsubmit事件");
                return false;
            }
        }
    </script>
</head>
<body>
<form action="https://www.baidu.com/" method="get" onsubmit="return onsubmitFun()">
    <input type="submit" value="静态注册"/>
</form>
<form action="https://www.baidu.com/" id="form01">
    <input type="submit" value="动态注册"/>
</form>
</body>
</html>
```



