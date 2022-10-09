# 07 - EL表达式&JSTL标签库

[toc]

## EL表达式

- EL表达式(Expression Language)是表达式语言。EL表达式的作用主要是替代jsp页面中的表达式脚本，在jsp页面中进行数据的输出。EL表达式在输出数据的时候，比jsp打的表达式脚本简洁很多

### 1. EL表达式的输出

#### 1.1 基本输出

- EL表达式的格式是：`${表达式}`

- jsp表达式脚本输出null的时候，输出的是null字符

  EL表达式在输出null值的时候，输出的是空串

  ```jsp
  <body>
  	<%
      	request.setAttribute("key","值");
      %>
      
      表达式脚本输出key值：
      <!--原jsp表达式-->
      <%=request.getAttribute("key1")%>
      <!--EL表达式-->
      ${key1}
  </body>
  ```

  

#### 1.2 EL表达式搜索域输出的顺序

- EL表达式主要用于输出域对象中的数据。当四个域都有相同的key的数据的时候，EL表达式就会按照四个域的从小到大的顺序去进行搜索，找到就输出。

  ```jsp
  <body>
  	<%
      	request.setAttribute("key","request");
      	session.setAttribute("key","session");
      	application.setAttribute("key","application");
      	pageContext.setAttribute("key","pageContext");
      %>
      ${key}
  </body>
  ```

  

#### 1.3 输出Bean对象的属性

- 输出Person类中的普通属性，数组属性，list集合属性和map集合属性

  EL表达式获取属性的本质并不是直接访问Bean对象中的属性，而是访问get方法，只要get方法存在，即便属性不存在，也能获取到"属性"，反之则不行

  ```jsp
  <body>
  	<%
      	//Person的创建和设置过程略
      	pageContext.setAttribute("person1",person);
      %>
      
      输出Person:${p} <br/>
      输出Person的name属性:${p.name}<br/>
      输出Person的phone数组属性值:${p.phone[2]} <br/>
      输出Person的List集合中的个别元素值:${p.cities[2]} <br/>
      输出Person的Map集合:${p.map} <br/>
      输出Person的Map集合某个key的值:${p.map.key3} <br/>
  </body>
  ```

  

### 2. EL表达式的11个隐含对象

- EL表达式中的11个隐含对象，是EL表达式中自己定义的，可以直接使用

  |       变量       |         类型         |                        作用                        |
  | :--------------: | :------------------: | :------------------------------------------------: |
  |   pageContext    |   PageContextImpl    |            可以获取jsp中的九大内置对象             |
  |    pageScope     |  Map<String,Object>  |           可以获取pageContext域中的数据            |
  |   requestScope   |  Map<String,Object>  |             可以获取Request域中的数据              |
  |   sessionScope   |  Map<String,Object>  |             可以获取Session域中的数据              |
  | applicationScope | Map<String,String[]> |          可以获取ServletContext域中的数据          |
  |      param       |  Map<String,String>  |                可以获取请求参数的值                |
  |   paramValues    | Map<String,String[]> |             可以获取[多个]请求参数的值             |
  |      header      |  Map<String,String>  |                可以获取请求头的信息                |
  |   headerValues   | Map<String,String[]> |             可以获取请求头的[多个]信息             |
  |      cookie      |  map<String,Cookie>  |            可以获取当前请求的Cookie信息            |
  |    initParam     |  Map<String,String>  | 可以获取在web.xml中配置的<context-param>上下文参数 |



#### 2.1 EL表达式获取四个特定域中的属性

- 使用XxxScope对象，可以有选择性的获取指定域对象中的值

  ```jsp
  <body>
  	<%
      	pageContext.setAttribute("key1","pageContext");
      	request.setAttribute("key1","request");
      	session.setAttribute("key1","session");
      	application.setAttribute("key1","application");
      %>
      
      <!--在之前的jsp程序中,由于优先级的关系,下面的表达式只能获取pageContext的值-->
      ${key1}
      
      <!--使用Scope对象之后就可以指定获取-->
      ${sessionScope.key1}
      ${requestScope.key1}
      ${applicationScope.key1}
  </body>
  ```

  

#### 2.2 pageContext对象

- pageContext对象可以获取jsp内置的九大对象

  ```jsp
  <body>
  	1.协议: ${pageContext.request.scheme}<br>
      2.服务器ip: ${pageContext.request.serverName}<br>
      3.服务器端口: ${pageContext.request.serverPort}<br>
      4.获取工程路径: ${pageContext.request.contextPath}<br>
      5.获取请求方法: ${pageContext.request.remoteHost}<br>
      6.获取客户端ip地址: ${pageContext.request.remoteHost}<br>
      7.获取会话的id编号: ${pageContext.session.id}<br>
  </body>
  ```

  

## JSTL标签库

- JSTL标签库(JSP Standard Tag Library)，是一个JSP标签库。EL表达式主要是为了替换jsp中的表达式脚本，而标签库则是为了替换代码脚本，使jsp页面变得更加简洁。

- JSTL标签库内部又有许多子库，子库需要在jsp标签库中使用taglib指令引入，下列列出几个常用子库

  ```jsp
  CORE 标签库
  <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
  FMT 标签库
  <%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt"%>
  SQL 标签库
  <%@ taglib prefix="fn" uri="http://java.sun.com/jsp/jstl/functions"%>
  ```

### 1. core核心库使用

#### 1.1 <c:set />

- set标签可以往域中保存数据，相当于`域对象.setAttribute(key,value);`

```jsp
<!--
    scope属性设置保存到哪个域
    var属性设置key
    value属性设置值
    -->
<c:set scope="request" var="abc" value="abcValue"/>
${requestScope.abc}
```

#### 1.2 <c:if />

- if标签用来做if判断，JSTL中不支持if-else结构，多路选择请使用choose标签

```jsp
<!--test属性表示判断的条件(使用EL表达式输出)-->
<c:if test="${12 == 12}">
    <h1>相等</h1>
</c:if>
```

#### 1.3 <c:choose > <c:when > <c:otherwise >

- 多路判断，跟switch-case-default结构比较类似

```jsp
<!--
	choose标签开始选择判断
    when标签表示每一种判断情况
    test属性表示当前判断
-->
<%
	request.setAttribute("height",178);
%>
<c:choose>
    <c:when test="${ requestScope.height > 190 }">
        <h2>高</h2>
    </c:when>
    <c:when test="${ requestScope.height > 180 }">
        <h2>中</h2>
    </c:when>
    <c:otherwise>
        <h2>低</h2>
    </c:otherwise>
</c:choose>

<!--注意标签里不能使用html注释，要使用jsp注释-->
<!--when标签的父标签一定要是choose标签-->
```

#### 1.4 <c:forEach />

- 遍历输出使用

  ```jsp
  <!--
      begin属性设置开始的索引
      end属性设置结束的索引
      var属性表示循环的变量 (当前正在遍历到的数据)
  -->
  
  <!--遍历输出1-10-->
  <c:forEach begin="1" end="10" var="i">
      <h1>${i}</h1>
  </c:forEach>
  <!--遍历也可以带格式-->
  
  <!--遍历Object数组-->
  <%
  	request.setAttribute("arr",new String[]){"1","2","3"};
  %>
  <c:forEach item="${requestScope.arr}" var="item">
      ${item}<br>
  </c:forEach>
  ```

#### 1.5 forEach标签的所有属性

- **items**：表示遍历的集合

  **var**：表示遍历到的数据

  **begin**：表示遍历的开始索引值

  **end**：表示结束的索引值

  **step**：表示遍历的步长值

  **varStatus**：表示当前遍历到的数据状态

  - **varStatus可以查询到的数据：**

    current：表示当前遍历到得到数据

    index：表示获取遍历的索引

    count：表示遍历的个数

    isFirst：表示当前遍历的数据是否是第一条

    isLast：表示当前遍历的数据是否是最后一条

    begin：获取begin属性

    end：获取end属性

    step：获取step属性

    



  

