# 10 - Filter过滤器&JSON&AJAX

[toc]

## 1. Filter过滤器

### 1.1. 基本概念

- Filter过滤器是JavaWeb的三大组件之一，是一种规范（接口）。过滤器的主要作用是拦截请求，过滤响应。

  拦截请求常见的应用场景有：权限检查，日记操作，事务管理等

- Filter过滤器的使用步骤：

  1. 编写一个类去实现Filter接口
  2. 实现过滤方法doFilter()
  3. 到web.xml中去配置Filter的拦截路径

### 1.2. Filter的生命周期

- Filter的生命周期包含以下几个方法：
  1. 构造器方法
  2. init初始化方法
     - 第1，2步，在web工程启动的时候执行（Filter已经创建）
  3. doFilter过滤方法
     - 每次拦截到请求，就会执行
  4. destroy销毁
     - 停止web工程的时候就会执行

### 1.3 FilterChain过滤器链

- 过滤器链表示多个过滤器一起工作，主要依靠`FilterChain.doFilter()`方法运作

  `FilterChain.doFilter()`的作用：

  1. 执行下一个Filter过滤器（如果有Filter的话）
  2. 执行目标资源（如果没有Filter的话）

- 过滤器链的运作方式和递归有些类似：

  假设有两个过滤器Filter1和Filter2，两个过滤器在`FilterChain.doFilter()`方法前后都有代码

  - 开始执行 -> 执行Filter1中的前置代码 -> 执行Filter1的doFilter方法 -> 执行Filter2的前置代码 -> 执行Filter2的doFilter方法 -> 执行目标资源 -> 执行Filter2的后置代码 -> 执行Filter1的后置代码

- 在多个Filter过滤器执行的时候，它们执行的顺序是由它们在web.xml中从上到下的配置顺序决定的

- 多个Filter过滤器执行的特点：

  1. 所有Filter和目标资源默认都执行在同一个线程中
  2. 多个Filter共同执行的时候，它们都使用同一个Request对象

### 1.4 Filter的拦截路径

- **精确匹配**

  `<url-pattern>/target.jsp</url-pattern>`

  以上配置的路径，表示请求地址必须为：`http://ip:port/工程路径/target.jsp`

- **目录匹配**

  `<url-pattern>/admin/*</url-pattern>`

  以上配置的路径，表示请求地址必须为：`http://ip:port/工程路径/admin/*`

- **后缀名匹配**

  `<url-pattern>*.html</url-pattern>`

  以上配置的路径，表示请求地址必须以`.html`结尾

## 2. JSON

- JSON（JavaScript Object Notation）是一种轻量级的数据交换格式，是一种理想的数据交换语言。数据交换指的是客户端和服务器之间业务数据的传递格式

### 2.1 json的定义和访问

- json由键值对组成，并且由花括号包围。每个键由引号引起来，键和值之间使用冒号进行分割，多组键值对之间由逗号进行分隔

- json本身是一个对象，json中的key可以理解为是对象中的一个属性，json中的key访问就跟访问对象的属性一样，`json对象.key`

  ```json
  var jsonObj = {
      "key1":12,
  	"key2":"abc",
  	"key3":[11,"arr",false],
  	"key4":{
      "key4_1":551,
      "key4_2":"key5_2_value"
  	}
  }
  
  alert(jsonObj.key1);
  alert(jsonObj.key2);
  alert(jsonObj.key3[0]);
  alert(jsonObj.key4.key4_1);
  ```

### 2.2 json的转换

#### 2.2.1 json对象与字符串的转换

- Json的存在有两种形式：

  - 对象的形式，称为json对象
  - 字符串的形式，称为json字符串

  一般要操作json中数据的时候，使用json对象的格式；在客户端和服务器之间进行数据交换的时候，使用json字符串

  ```json
  //把json对象转换成为json字符串
  var jsonObjString = JSON.stringify(jsonObj);
  //把json字符串，转换成为json对象
  var jsonObj2 = JSON.parse(jsonObjString);
  ```

#### 2.2.2 json对象和javaBean的转换

```java
public void test1(){
    Person person = new Person(1,"AAA");
    //创建Gson对象实例
    Gson gson = new Gson();
    //toJson方法可以把java对象转换称为json字符串
    String personJsonString = gson.toJson(person);
    System.out.println(personJsonString);
    //fromJson把json字符串转换会Java对象
    Person person1 = gson.fromJson(personJsonString,Person.class);
}
```

#### 2.2.3 List/Map和json的转换

```java
public void test2(){
    List<Person> personList = new ArrayList<>();
    
    personList.add(new Person(1,"AAA"));
    personList.add(new Person(2,"BBB"));
    
    Gson gson = new Gson();
    
    //把List转换为json字符串
    String personListJsonString = gson.toJson(personList);
    System.out.println(personListJsonString);
    
    List<Person> list = gson.fromJson(personListJsonString,new PersonListType().getType());
    System.out.println(list);
    Person person = list.get(0);
    System.out.println(person);
}

public class PersonListType extends TypeToken<ArrayList<Person>>{
}
```

- 其实可以使用匿名内部类避免单独实现接口，下面以Map和json的转换举例

```java
public voi test3(){
    Map<Integer,Person> personMap = new HashMap<>();
    
    personMap.put(1,new Person(1,"AAA"));
    personMap.put(2,new Person(2,"BBB"));
    
    Gson gson = new Gson();
    
    //将map集合转换成为json字符串
    String personMapJsonString = gson.toJson(personMap);
    System.out.println(personMapJsonString);
    
    Map<Integer,Person> personMap2 = gson.fromJson(personMapJsonString, new TypeToken<HashMap<Integer,Person>>(){}.getType());
    System.out.println(personMap2);
    Personp p = personMap2.get(1);
    
}
```



## 3. AJAX

