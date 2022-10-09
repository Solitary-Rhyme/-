# HTML

[toc]

## 1. HTML文件的基础结构

```html
<!--约束，声明-->
<!DOCTYPE html>
<!--html标签表示html的开始 lang表示所使用的语言-->
<!--一般包含两部分，head和body-->
<html lang="en">
<!--head标签表示头部信息-->
<!--一般包含三部分，title标签,css样式,js代码-->
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<!--body标签是整个html页面显示的主体内容-->
<body>
    hello
</body>
</html>
```

## 2. HTML标签

### 2.1 标签基础

1. 标签的格式: `<标签名>封装的数据</标签名>`

2. 标签名大小写不敏感

3. 标签拥有自己的属性
   - 基本属性：`bgcolor="red"`，基本属性可以修改简单的样式效果，有些属性有属性名，有些没有
   - 事件属性：`onclick="alert('hello');"`，事件属性可以直接设置事件响应后的代码

4. 标签根据格式又分为两种
   - 单标签格式：`<标签名/>`
   - 双标签格式：`<标签名> ...... </标签名>`

5. **块元素与行内元素**

   在页面中独占一行的元素称为块元素(block element)；在网页中一般通过块元素对页面进行布局

   - 块元素会在页面中独占一行
   - 默认宽度是父元素的全部
   - 默认高度是内容的高度

   在页面中不会独占一行的称为行内元素(inline element)；行内元素主要用来包裹文字

   - 行内元素不会独占页面的一行，只占自身的大小
   - 行内元素在页面中自左向右水平排列
   - 行内元素的默认宽度和高度都是内容的宽度和高度

   注意：

   - 一般情况下会在块元素中放行内元素，而不会在行内元素中放块元素
   - 块元素基本上什么都能放
   - p元素中不能放任何的块元素

### 2.2 常用标签

#### meta标签

- meta主要用于设置网页中的一些元数据，元数据不是给用户看的

- **属性**

  - charset 指定网页的字符集
  - name 指定的数据名称
  - content 指定的数据内容
  - http-equiv 将页面重定向到另一个网站

  ```html
  <head>
  	<meta charset="UTF-8">
  	<meta name="keywords" content="HTML5,CSS3">
      <meta name="description" content="这是一个网站">
      <!--三秒后跳转到百度-->
      <meta http-equiv="refresh" content="3;url=https://www.baidu.com">
  </head>
  ```

- 常见的meta的name属性

  - keywords 表示网站的关键字，可以同时指定多个关键字，关键字之间使用逗号隔开
  - description用于指定网站的描述

#### 实体(转义字符)

- 就像在Java中输出`\n`需要使用`\\n`一样，HTML也有类似的需求。需要输出特殊符号的时候就要使用特殊字符

  常用的特殊字符：`< : &lt`，`> : &gt`，`空格 : &nbsp`

  ```html
  我是&lt;br&gt;标签<br/>
  ```

#### 标题标签

- 标题标签是块元素

  h1到h6都是标题标签。从h1-h6重要性依次递减，h1在网页中的重要性仅次于title标签，一般情况下一个页面中只有一个h1标签。一般情况下标题标签只会使用到h1-h3

- hgroup标签用来为标题分组，可以将一组相关的标题同时放入到hgroup

  ```html
  <hgroup>
  <h1 align="left">标题1</h1>
  <h2 align="center">标题2</h2>
  </hgroup>
  <h3 align="right">标题3</h3>
  
  <!--align属性是对齐属性，left左对齐(默认)，center居中，right右对齐-->
  ```

#### 超链接标签

- 超链接标签是一个行内元素。但在标签中可以嵌套除它自身外的任何元素。

- href属性设置连接的地址，属性值可以是一个外部网站的地址，也可以是内部页面的地址

  将href属性设置为`#(目标元素的id属性值)`可以跳转到页面的指定位置，如果只有一个`#`则默认返回页面顶部

- target属性设置哪个目标进行跳转。`_self`表示当前页面（默认值），`_blank`表示打开新页面来进行跳转

  ```html
  <a href = "https://www.baidu.com/" target="_self">百度a</a><br/>
  <a href = "https://www.baidu.com/" target="_blank">百度b</a>
  ```

#### 列表标签

- ul是无序列表，ol是有序列表，列表间可以相互嵌套

  li是列表项，type属性可以修改列表项前面的符号
  
  ```html
  <ul type="none">
      <li>AAA</li>
      <li>BBB</li>
      <li>CCC</li>
  </ul>
  ```

#### img标签

- img标签是属于替换元素(介于块元素和行内元素之间)，用来显示图片

  - src属性可以设置图片的路径
  - width属性设置图片的宽度
  - height属性设置图片的高度
  - border属性设置图片边框大小
  - alt属性设置当指定路径找不到图片时，用来替代显示的文本内容

- web中路径的书写格式和JavaSE有所不同：

  - 相对路径：`.`表示当前文件所在的目录；`..`表示当前文件所在的上一级目录；`文件名`表示当前所在目录的文件
  - 绝对路径：`http://ip:port/工程名/资源路径`

  ```html
  <img src="../imgs/1.jpg" width="200" height="260" border="1" alt="找不到图片"/>
  ```
  
- 图片常见格式

  - **jpeg(jpg)**：支持的颜色比较丰富，不支持透明效果，不支持动图
  - **gif**：支持的颜色比较少，支持简单透明，支持动图
  - **png**：支持的颜色丰富，支持复杂透明，不支持动图
  - **webp**：具备其它图片格式的所有优点，文件小。但是对除Chrome外游览器兼容性较差
  - **base64**：将图片使用base64编码转换为字符，通过字读的形式引入图片。一般为需要和网页一起加载的图片才会使用base64


#### 音频标签

- audio标签用来向页面中引入一个外部的音频文件，video标签用来向页面中引入一个视频

  - controls属性设置是否允许用户控制播放
  - loop属性设置音乐是否循环播放
  - src属性可以设置音频的路径
  - source属性和src属性基本相同，但功能更多（见下）

  ```html
  <audio src="..." controls autoplay loop></audio>
  <video controls>
  	<source src="aaa.webm">
      <source src="aaa.webm">
      <source src="aaa.webm">
      该游览器不支持播放此视频！
  </video>
  <!--使用多个source引入资源的时候，会从上至下尝试播放资源。若选中资源无法播放，则尝试播放下一个。若所有资源都无法播放，则显示文字字段-->
  ```


#### 表格标签

- table标签是表格标签（下列为针对表格操作）

  - border设置表格边框
  - width设置表格宽度
  - height设置表格高度
  - align设置表格相对于页面的对齐方式
  - cellspacing设置单元格间距

- （下列为针对表格内文本操作）

  - tr是行标签
  - th是表头标签
  - td是单元格标签
  - align设置单元格文本对齐方式
  - b是加粗标签

  ```html
  <table align="center" border="1" width="300" height="300" cellspacing="0">
      <tr>
          <th>1.1</th>
          <th>1.2</th>
      </tr>
      <tr>
          <th>2.1</th>
          <th>2.2</th>
      </tr>
  </table>
  ```

- colspan 属性设置跨列

  rowspan 属性设置跨行

  ```html
  <table align="center" border="1" width="300" height="300" cellspacing="0">
      <tr>
          <th colspan="2">1.1</th>
          <th>1.3</th>
      </tr>
      <tr>
          <th rowspan="2">2.1</th>
          <th>2.2</th>
          <th>2.3</th>
      </tr>
      <tr>
          <th>3.2</th>
          <th>3.3</th>
      </tr>
  </table>
  ```

#### iframe框架标签

- iframe标签可以在页面上开辟一个小区域显示一个单独的页面；iframe一般配合超链接使用

  ```html
  <iframe src="XXX" width="500" height="400" name="abc"></iframe>
  <a href = "https://www.baidu.com/" target="abc">Baidu</a>
  <a href = "https://cn.bing.com/" target="abc">Bing</a>
  ```


####   表单标签

- form标签表示表单

  | input type | 效果       | 设置                                                         |
  | ---------- | ---------- | ------------------------------------------------------------ |
  | text       | 文件输入框 | value设置默认显示内容                                        |
  | password   | 密码输入框 | value设置默认显示内容                                        |
  | radio      | 单选框     | name属性可以对其进行分组；checked="checked"表示默认选中      |
  | checkbox   | 复选框     | checked="checked"表示默认选中                                |
  | reset      | 重置按钮   | value属性修改按钮上的文本                                    |
  | submit     | 提交按钮   | value属性修改按钮上的文本                                    |
  | button     | (普通)按钮 | value属性修改按钮上的文本                                    |
  | file       | 文件上传域 |                                                              |
  | hidden     | 隐藏域     | 当我们要发送某些信息，而这些信息，不需要用户参与，就可以使用隐藏域（信息会在提交的时候同时发给服务器） |

  ```html
  <form>
      用户名称：<input type="text" value="默认值"/><br/>
      用户密码：<input type="password" value="abc"/><br/>
      性别：<input type="radio" name="sex"/>男<input type="radio" name="sex" checked="checked"/>女<br/>
      <input type="reset" />
  </form>
  ```

  select 标签是下拉列表框

  option标签是下拉列表框中的选项，selected = "selected"设置默认选中

  textarea表示多行文本输入框（起始标签和结束标签中的内容是默认值）

  - rows属性设置可以显示几行的高度
  - cols属性设置每行可以显示几个字符宽度

  ```html
  国籍：<select>
      	<option>--请选择国籍--</option>
      	<option>中国</option>
      	<option>美国</option>
      </select><br/>
  自我评价：<textarea rows="10" cols="20"></textarea><br/>
  ```

#### 表单的格式化

- 结合表格可以让表单更加美观

  ```html
  <form>
      <table align="center">
          <tr>
              <td>用户名称：</td>
              <td><input type="text" value="默认值"/></td>
          </tr>
          <tr>
              <td>用户密码：</td>
              <td><input type="password" value="abc"/></td>
          </tr>
          <tr>
              <td>性别：</td>
              <td><input type="radio" name="sex"/>男<input type="radio" name="sex" checked="checked"/>女</td>
          </tr>
          <tr>
              <td>国籍：</td>
              <td><select>
                  <option>--请选择国籍--</option>
                  <option>中国</option>
                  <option>美国</option>
              </select></td>
          </tr>
          <tr>
              <td>自我评价：</td>
              <td><textarea rows="10" cols="20"></textarea></td>
          </tr>
          <tr>
              <td><input type="reset"/></td>>
              <td><input type="submit"/></td>>
          </tr>
      </table>
  </form>
  ```

#### 表单的提交

- 对于`input type="submit"`，它可以配合事件属性和超链接将数据提交到服务器，有get和post两种请求方式

  ```html
  <form action="https://www.baidu.com/" method="get">
      <input type="text" name="username" value="123"/>
      <input type="submit"/>
  </form>
  ```

- 表单提交时，数据没有发送给服务器的三种情况:

  1. 表单项中没有设置name属性值
  2. 单选，复选，下拉列表中的option标签都需要添加value属性，以便发送给服务器
  3. 表单项不在提交的form标签中

- GET请求的特点是：

  1. 游览器地址栏中的地址是：action属性+?+请求参数
  2. 不安全
  3. 有数据长度的限制

- POST请求的特点是：

  1. 游览器地址栏中只有action属性值
  2. 相对于GET请求安全
  3. 理论上没有数据长度的限制

#### 其他标签

- div标签：块元素。没有语义，用来表示一个区块。是目前主要的布局元素

  span标签：行内元素。没有语义，一般用于在网页中选中元素

  p段落标签：默认会在段落的上方或下方各空出一行来（如果已有就不再空）

  字体标签：可以用来修改文本的字体，颜色，大小。不建议使用，所有关于字体的设定应该交由CSS负责
  
  ```html
  <div>div标签</div>
  <span>span标签</span>
  <p>p段落标签</p>
  <font color="red" face="宋体" size="7">我是字体标签</font>
  ```

- **使用一个标签应该重点关注其语义，而非表现形式。应为所有的表现形式都可以通过CSS修改**
