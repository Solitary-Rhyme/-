# 03 - jQuery

[toc]

### 1.jQuery基础概念

- jQuery是辅助JavaScript开发的js类库，它能大幅简化js中的操作量

- 简单的jQuery程序

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
      <script type="text/javascript" src="jquery-3.5.1.js"></script>
      <script type="text/javascript">
          $(function(){	//表示页面加载完成后,相当于windows.onlaod
              var $btnObj = $("#btnId");	//表示按id查询标签对象
              $btnObj.click(function () {	//绑定单击事件
                  alert("jQuery的单击事件");
              });
          });
      </script>
  </head>
  <body>
  <button id="btnId">SayHello</button>
  </body>
  </html>
  ```



### 2. jQuery核心函数$

- `$`是jQuery的核心函数，通过`$()`调用。根据传入参数的不同，`$`函数会作出不同的反应
  1. 传入参数为 [函数] 时：表示页面加载完成之后，相当于`window.onload=function(){}`
  2. 传入参数为 [HTML字符串] 时：会为我们创建这个html对象
  3. 传入参数为 [选择器字符串] 时：
     - `$("#id属性值");`：id选择器，根据id查询标签对象
     - `$("标签名");`：标签名选择器，根据指定的标签名查询标签对象
     - `$(".class属性值");`：类型选择器，可以根据class属性查询标签对象
  4. 传入参数为 [dom对象] 时：将dom对象包装为jQuery对象返回
- **jQuery对象和DOM对象**
  - jQuery对象本质是dom对象的数组，jQuery额外提供了一系列功能函数
  - jQuery对象和DOM对象互相不能调用彼此的属性和方法
  - DOM对象可以通过`$(DOM对象)`转换为jQuery对象；jQuery对象可以通过`jQuery对象[下标]`转换为DOM对象



### 3. 选择器

- **选择器的分类：**
  - 基础选择器
  - 层级选择器
  - 基本过滤选择器
  - 内容过滤选择器
  - 属性过滤选择器
  - 表单过滤选择器
  - 元素筛选
- 关于选择器的具体使用和练习实例，请翻阅API和"选择器练习"文件夹



### 4. html(),text(),val()

- **html()**

  可以设置和获取起始标签和结束标签中的**内容**，跟dom属性innerHTML一样

  ```js
  $(function(){
  	//空参表示获取
  	alert($("div").html());
  	//有参表示修改
  	$("div").html("<h1>我是新修改的h1标签</h1>");
  });
  ```

  

- **text()**

  它可以设置和获取起始标签和结束标签中的**文本**，跟dom属性的innerText一样

  ```js
  $(function(){
  	//空参表示获取
  	alert($("div").text());
  	//有参表示修改
  	$("div").text("<h1>我是新修改的h1标签</h1>");
  });
  ```

  **在获取方面：**html会将包括关键字在内的信息都显示出来，text只会获取基本的文本信息

  **在修改方面：**html会将设置内容中的关键字自动替换为相应的格式，text则不会

- **val()**

  它可以设置和获取**表单项**的value属性值，跟dom属性value一样

  ```js
  $(function(){
      //批量选择单选(radio)，这种操作同样适用于checkbox,multiple,single
      $(":radio").val(["radio2","radio1","radio3"]);
      
      //批量选择多种类型
      $(":radio,:checkbox,:single").val(["radio2","checkbox1","checkbox3","sin3"]);
  })
  ```

  

### 5. attr(),prop()

- **attr()**

  可以设置和获取属性的值，但不推荐操作checked，readOnly，selected，disabled等等。attr方法还可以操作非标准的属性，比如自定义属性

  ```js
  $(function(){
      //空参表示获取
      alert($(":checkbox:first").attr("name"));
      //有参表示设置
      $(":checkbox:first").attr("name","abc");
  });
  ```

  

- **prop()**

  可以设置和获取属性的值，只推荐操作checked，readOnly，selected，disabled等等

  ```js
  $(function(){
      $(":checkbox:first").prop("name",false);
  });
  ```

  

### 6. DOM的增删改

- **内部插入**

  appendTo()：`a.appendTo(b)`把a插入到b**子元素**末尾，成为最后一个子元素

  prependTo()：`a.prependTo(b)`把a插入到b**子元素**前面，成为第一个子元素

  ```js
  $("<h1>标题</h1>").prependTo($("div"));
  ```

  

- **外部插入**

  insertAfter()： `a.insertAfter(b)`得到ba

  insertBefore()：`a.insertBefore(b)`得到ab

  ```js
  $("<h1>标题</h1>").insertBefore($("div"));
  ```



- **替换**

  replaceWith()：`a.replaceWith(b)`用b替换掉a

  replaceAll()：`a.replaceAll(b)`用a替换掉所有b

  ```js
  $("div").replaceWith($("<h1>标题</h1>"));
  $("div").replaceAll($("<h1>标题</h1>"));
  ```



- **删除**

  remove()：`a.remove()`删除a标签

  empty()：`a.empty()` 清空a标签内的内容

  ```js
  $("div").empty();
  $("div").remove();
  ```

  

### 7. css样式操作

- **addClass()**：添加样式

  **removeClass)()**：删除样式

  **toggleClass()**：有就删除，没有就添加样式

  **offset()**：获取和设置元素的坐标

  ```js
  $(function(){
      var $divEle = $('div:first');
      
      $('#btn01').click(function(){
          $divEle.addClass('redDiv blueBorder'); //单引号内为事先定义好的css样式
      });
      
      $('#btn02').click(function(){
          $divEle.removeClass('redDiv'); 
      });
      
      $('#btn03').click(function(){
          $divEle.toggleClass('redDiv');
      });
      
      $('#btn04').click(function(){
          alert($divEle.offset());
          
          $divEle.offset({
              top:100,
              left:50
          });
      });
  });
  ```



### 8. jQuery动画

- **基本动画**

  **show()**：将隐藏的元素显示

  **hide()**：将可见的元素隐藏

  **toggle()**：可见就隐藏，不可见就展示

- 以上方法都可以添加参数：

  1. 第一个参数是动画执行的时长，以毫秒为单位
  2. 第二个参数是动画的回调参数（动画完成后自动调用的函数）

  ```js
  $(function(){
      $("#div1").show(2000,function(){
          alert("hi");
      });
  });
  
  $(function(){
      $("#div2").hide();
  });
  
  $(function(){
      $("#div3").toggle();
  });
  ```

  

- **淡入淡出动画**

  **fadeIn()**：淡入

  **fadeOut()**：淡出

  **fadeTo()**：在指定时长内慢慢的将透明度修改到指定的值。(0透明,0.5半透明,1完全可见)

  **fadeToggle()**：淡入/淡出切换

  - 使用方法和基本动画差不多，在此不赘述

  

### 9. jQuery事件操作

#### 9.1 原生js和jQuery页面加载的区别

- **触发时间**：
  - JQuery：当游览器内核解析完页面的标签，创建好DOM对象之后，JQuery的页面加载就会马上执行
  - JS：除了要等游览器内核解析完标签，创建好DOM对象以外，还需要等标签显示需要的内容加载完成后才会执行
- **触发顺序**：
  - JQuery页面加载完成后先执行，原生JS后执行
- **执行次数**：
  - JQuery的页面加载完成之后会把全部注册的function函数，按顺序执行一遍
  - 原生js的页面加载完成之后，只会执行最后一次的赋值函数

#### 9.2 jQuery中常用的事件处理方法

- **click()**：可以绑定以及触发单击事件

  **mouseover()**：鼠标移入事件

  **mouseout()**：鼠标移出事件

  **bind()**：可以给元素一次性绑定一个或多个事件

  **one()**：使用上跟bind一样。但是one方法绑定的事件只会响应一次

  **unbind()**：跟bind方法相反的操作，解除事件的绑定

  **live()**：可以用来绑定，选择器匹配的，所有元素的事件。即便这个元素时动态创建出来的也可以自动绑定

  ```js
  $(function(){
     $("h5").click(function(){
         alert("test");
     });
      
      $("h5").mouseover(function(){
         alert("test");
     });
      
      $("h5").mouseout(function(){
         alert("test");
     });
      
      $("h5").bind("click mouseover mouseout",function(){
          alert("test");
      });
      
      $("h5").one("click mouseover mouseout",function(){
          alert("test");
      });
      
      //如果不指定unbind的参数,那么unbind会删除掉目标所有绑定的事件
      $("h5").unbind("mouseout",function(){
          alert("test");
      });
      
      //针对静态的数据，live创建的click事件和普通的click事件并没有区别
      $("h5").live("click",function(){
         alert("test");
     });
      //但是对于如下动态创建的数据，click事件并不会绑定，但是live可以自动绑定
      $('<h5 class="head">TEEEEST</h5>').appendTo($("#panel"));
  });
  ```

  

#### 9.3 事件冒泡

- 事件冒泡是指，父子元素同时监听一个事件，当触发子元素事件的时候，同一个事件也被传递到了父元素的事件里去响应。通常在编程中希望取消事件冒泡这一特性，此时只需要在事件函数体内return false即可阻止事件的冒泡传递

  ```html
  <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
  <html>
  <head>
  <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
  <title>title</title>
  
  <script type="text/javascript" src="../script/jquery-1.7.2.js"></script>
  <script type="text/javascript">
  	$(function(){
  		$("#content").click(function () {
  			alert("我是div");
  		});
  
  		$("span").click(function () {
  			alert("我是span");
              return false;
  		});
  	});
  </script>
  </head>
  <body>
  <div id="content">
  	外层div元素
  	<span>内层span元素</span>
  	外层div元素
  </div>
  </body>
  </html>
  ```



#### 9.4 事件对象

- 事件对象是封装有触发的事件信息的一个javascript对象。在给元素绑定事件的时候，在事件的function()参数列表中添加一个参数(一般习惯填event)。这个event就是js传递事件处理函数的事件对象

  ```js
  //有了event，就可以在bind绑定多个事件的时候，获取当前发生的事件并针对性操作
  $("#areaDiv").bind("mouseover mouseout",function(event){
     if(event.type == "mouseover"){
         console.log("鼠标移入");
     }else if(event.type == "mouseout"){
         console.log("鼠标移出");    
     }
  });
  ```

  

