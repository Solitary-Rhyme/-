# CSS

[toc]

### 1. CSS和HTML的结合方式

- **内联样式/行内样式**

  在标签内部，通过style属性设置元素的样式`key:value value;`

  （由于内联样式的复用性非常差，所以开发中基本不使用内联样式）

  ```html
  <div style="border: 1px solid red;">div标签</div>
  ```

- **内部样式表**

  将样式编写到head中的style标签中，然后通过CSS选择器来选中元素并为其设置各种样式

  ```html
  <!DOCTYPE html>
  <html>
  <head>
      <style>
          /*注意css内的注释方式与html不同*/
          div{
              border: 1px solid red;
          }
          span{
              border: 2px solid yellow;
          }
      </style>
  </head>
  </html>
  ```
  
- **外部样式表**

  把css样式编写到一个外部的CSS文件中，然后通过link标签来引入外部的CSS文件

  样式可以在不同页面之间进行复用，同时能利用缓存机制加快网页的加载速度，提高用户的体验
  
  ```css
  /*css文件内*/
  div{
  	border: 1px solid red;
  }
  span{
  	border: 2px solid yellow;
  }
  ```
  
  ```html
  <!--html文件内-->
  <!DOCTYPE html>
  <html>
  <head>
      <link rel="stylesheet" type="text/css" href="WebTest.css"/>
  </head>
  </html>
  ```
  
  

### 2. CSS选择器

#### 2.1 常用选择器

**1.元素选择器**

- 根据标签名来选中指定的元素

  语法：`标签名{}`
  
  ```css
  div{
  	border: 1px solid red;
  }
  
  span{
  	border: 2px solid yellow;
  }
  ```

**2.id选择器**

- 根据元素的id属性值选中一个元素

  语法：`#id属性值{}`
  
  ```css
  #id001{
  	border: 1px solid red;
  }
  
  #id002{
  	border: 2px solid yellow;
  }
  ```

**3.class类型选择器**

- 根据元素的class属性值选中一组元素

  语法：`.class属性值`
  
  ```css
  .class001{
  	border: 1px solid red;
  }
  
  .class002{
  	border: 2px solid yellow;
  }
  ```

**4.通配选择器**

- 选中页面中的所有元素

  语法：`*{}`

#### 2.2 复合选择器

**1.交集选择器**

- 同时选中符合多个条件的元素

  语法：`选择器1选择器2选择器n{}`
  
  （交集选择器中如果有元素选择器，必须使用元素选择器开头）
  
  ```css
  /*div元素中有red的class类型的元素*/
  div.red{
  	font-size: 30px;
  }
          
  /*同时具有a,b,c的class类型的元素*/
  .a.b.c{
  	color: blue;
  }
  ```
  

**2.并集选择器**

- 同时选中多个选择器对应的元素

  语法：`选择器1,选择器2,选择器n{}`

  （交集选择器中如果有元素选择器，必须使用元素选择器开头）

  ```css
  /*选中h1元素和span元素*/
  h1,span{
  	font-size: 30px;
  }
  ```

#### 2.3 关系选择器

- **元素关系**

  - 父元素：**直接**包含子元素的元素叫做父元素

  - 子元素：**直接**被父元素包含的元素是子元素

  - 祖先元素：直接或间接包含后代元素的元素叫做祖先元素

    ​					祖先元素不一定是父元素，父元素一定是祖先元素

  - 后代元素：直接或间接被祖先元素包含的元素叫做后代元素

    ​					子元素一定是后代元素，后代元素不一定是子元素

  - 兄弟元素：拥有相同父元素的元素是兄弟元素

**1.子元素选择器**

- 选中指定父类元素的指定子元素

  语法：`父元素 > 子元素`
  
  ```css
  div.box > span{
  	color: orange;
  }
  
  div > p > soan{
  	color: red;
  }
  ```

**2.后代元素选择器**

- 选中指定元素内的指定后代元素

  语法：`祖先 后代`
  
  ```css
  div span{
  	color: red;
  }
  ```

**3.兄弟元素选择器**

- 选择下一个兄弟

  语法：`前一个 + 下一个`

- 选择下面所有兄弟

  语法：`兄 ~ 弟`

```css
p + span{
	color: red;
}

p ~ span{
	color: red;
}
```

#### 2.4 属性选择器

- 根据属性名和属性值选中指定元素

  语法：

  - `[属性值] 选择含有指定属性的元素`
  - `[属性名=属性值] 选择含有指定属性和属性值的元素`
  - `[属性名^=属性值] 选择属性值以指定值开头的元素`
  - `[属性名$=属性值] 选择属性值以指定值结尾的元素`
  - `[属性名*=属性值] 选择属性值中含有某值的元素`

  ```css
  p[title]{
  	color: red;
  }
  
  p[title=abc]{
  	color: red;
  }
  
  p[title^=abc]{
  	color: red;
  }
  
  p[title$=abc]{
  	color: red;
  }
  ```

#### 2.5 伪类选择器

**常见伪类**

- 伪类用来描述一个元素的特殊状态，如：第一个子元素、被点击的元素等

  语法：`:要选中的伪类`

  常见的伪类：

  - `:first-child` 第一个子元素

  - `:last-child` 最后一个子元素

  - `:nth-child(number)` 选中第n革子元素

    - n 选中第n个元素
    - 2n 选中偶数位的元素
    - 2n+1 选中奇数位的元素

    以上这些伪类都是根据所有的子元素进行排序

  - `:first-of-type`、`:last-of-type`、`:nth-of-type()`

    这几个伪类的功能和上述的类似，但他们是在同类型元素中进行排序

  - `not()` 否定伪类，将符合条件的元素从选择器中去除

  ```css
  ul > li:first-child{
  	color: red;
  }
  ul > li:nth-child(2n+1){
  	color: red;
  }
  ul > li:first-of-type{
  	color: red;
  }
  ul > li:not(:nth-child(3)){
  	color: red;
  }
  ```

**超链接伪类**

- `:link` 用来表示没访问过的链接

- `:visited` 用来表示访问过的链接

- `:hover` 用来表示鼠标移入的状态

- `:active` 用来表示鼠标点击

  ```html
  a:link{
  	color: red;
  }
  a:visited{
  	color: red;
  }
  a:hover{
  	color: red;
  }
  a:active{
  	color: red;
  }
  ```

#### 2.6 伪元素选择器

- 伪元素，表示页面中一些特殊的，并不真实存在的元素

- 语法：`元素::伪元素`

  常见的伪元素：

  - `::first-letter`：表示第一个字母

  - `::first-line`：表示第一行

  - `::selection`：表示选中的内容

  - `::before`：元素的开始

    `::after`：元素的末尾

    before和after必须结合content属性使用

  ```css
  p::first-letter{
  	color: red;
  }
  
  p::first-line{
  	color: red;
  }
  
  p::selection{
  	color: red;
  }
  
  div::before{
  	content: 'hi';
  	color: red;
  }
  
  div::after{
  	content: 'haha';
  	color: blue;
  } 
  ```
  
  

### 3. 盒模型

- CSS将页面中的所有元素都设置为一个矩形的盒子，对页面的布局就相当于将不同的盒子摆放到不同的位置。

- 盒模型组成部分：
  - 内容区(content)
  - 内边距(padding)
  - 边框(border)
  - 外边距(margin)

#### 3.1 内容区

- 元素中的所有子元素和文本内容都在内容区中排列

#### 3.2 边框

- 边框处于盒子边缘，是盒子内部和外部的分界线。边框的大小会影响到整个盒子的大小

- 边框的常用属性：

  - 边框的宽度 `border-width`

  - 边框的颜色 `border-color`

  - 边框的样式 `border-style`

    - 实线 `solid`
    - 点状虚线 `dotted`
    - 虚线 `dashed`
    - 双线 `double`
  
  
  ```css
  .box{
  	/*content部分属性设置*/
  	width: 200px;
  	height: 200px;
  	backgroud-color: green;
              
  	/*设定边框宽度*/
  	border-width: 10px;
              
  	/*设定边框颜色*/
  	border-color: orange;
              
  	/*设定边框样式*/
  	border-style: solid;
              
  	/*以下三个属性都可以被设置为none*/
  	border-width:none;
  	border-color:none;
  	border-style:none;
              
  	/*border简写设置*/
  	border: solid 10px orange;
  	border-top:solid 10px orange;
  }

#### 3.3 内外边距

- 内容区和边框之间的距离称为内边距（padding）

  - 内边距的设置会影响到盒子的大小，背景颜色会延伸到内边距上

  边框之外的区域称为外边距（margin）

  - 外边距不会影响可见框的大小，但是外边距会影响盒子的位置和实际占用空间

  - 默认情况下设置的左和上外边距会移动盒子自身，而设置的下和右边距会移动其它元素

  ```css
  .box{        
  	padding: 10px;
  	padding: 10px 20px 30px 40px;
  	margin: 10px;
  	margin-bottom: 10px;
  }
  ```

#### 3.4 行内元素的盒模型

- 行内元素不支持设置宽度和高度

  行内元素可以设置padding，但垂直方向padding不会影响页面的布局

  行内元素可以设置border，但垂直方向的border不会影响页面的布局

  行内元素可以设置margin，但垂直方向的margin不会影响页面的布局



### 4. 浮动

- 使用float属性来设置元素的浮动，通过浮动可以使一个元素向其父元素的左侧或右侧移动
- 语法：
  - `none`：默认值，元素不浮动
  - `left`：元素向左浮动
  - `right`：元素向右浮动
- **浮动的特点：**
  - 浮动元素会完全脱离文档流，不再占据文档流中的位置
    - 块元素不再独占页面的一行
    - 块元素的宽度和高度默认都被内容撑开
    - 行内元素会变成块元素
  - 设置浮动以后元素会向父元素的左侧或右侧移动，且默认不会从父元素中移出
  - 元素设置浮动以后，水平布局的等式便不需要强制成立
  - 浮动元素在移动时，不会超过它之前的，其它浮动元素的位置和高度
  - 浮动元素不会盖住文字，可以利用浮动设置文字环绕图片的效果

#### 4.1 高度塌陷和BFC

- **高度塌陷**：在一般布局中，父元素的高度默认是被子元素撑开的。当子元素浮动后，其会脱离文档流，无法撑起父元素的高度，造成父元素的高度丢失，进而导致页面布局混乱。这样的问题称之为高度塌陷

- **BFC（块级格式化环境）**：BFC是CSS中的一个隐藏属性，开启BFC后该元素会变成一个独立的布局区域。

  开启BFC后的特点：

  - 开启BFC的元素不会被浮动元素所覆盖
  - 开启BFC的元素，子元素和都元素外边距不会重叠
  - 开启BFC的元素可以包含浮动的子元素

- **clear属性**：可以通过设置clear属性清除浮动元素对当前元素的影响

  语法：

  - `left`：清除左侧浮动元素对当前元素的影响
  - `right`：清除右侧浮动元素对当前元素的影响
  - `both`：清除两侧浮动元素中对当前元素的影响最大的那侧

  原理：设置清除浮动后，游览器会自动为元素添加一个上边距，以使其位置不受其它元素的影响

- **利用clear解决高度坍塌的方法**

  ```css
  .clearfix::before,
  .clearfix::after{
  	content:'';
  	display: table;
  	clear:both;
  }
  ```
  
  原理：将父元素的class设置为clearfix，使用伪类在前端和末端分别插入一个没有实际文本显示的空字符。插入在前端的文本可以解决外边距重叠问题，因为隔开了父元素和子元素的外边距。插入在后端的文本解决了高度塌陷的问题，由于clear的原因会在父元素末尾插入一个，高度包括浮动元素在内的空字符，这样父元素就会被撑开。最后display为table是因为table在空的情况下不占体积。



### 5. 定位

- 定位是一种更加高级的布局手段，通过**position属性**可以将元素摆放到页面的任意位置

  语法：

  - `static`：默认值，元素是静止的，没有开启定位
  - `relative`：开启元素的相对定位
  - `absolute`：开启元素的绝对定位
  - `fixed`：开启元素的固定定位
  - `sticky`：开启元素的粘滞定位

- 当元素开启了定位后，可以通过`top/bottom/left/right`来设置偏移量，从而调整元素位置

#### 5.1 相对定位

- 开启相对定位后元素的特点：
  - 如果不设置偏移量，元素不会发生任何变化
  - 相对位置是参照元素在文档流中的位置进行定位的
  - 相对定位会提升元素的层级
  - 相对定位不会改变的性质（块元素/行内元素）
  
  ```css
  .box1{
      height: 100px;
      width: 100ox;
      
      /*开启相对定位*/
      position: relative;
      
      /*向左上(实际是左下)偏移*/
      left:100px;
      top:-200px;
  }
  ```

#### 5.2 绝对定位

- 开启绝对定位后元素的特点：
  - 开启绝对定位后，如果不设置偏移量**元素的位置**不会发生任何变化
  - 开启绝对定位后，元素会从文档流中脱离
  - 绝对定位会提升元素的层级
  - 绝对定位会改变元素的性质，行内元素变成块元素，块元素的宽高将会被内容撑开
  - 绝对定位元素是相对于其包含块进行定位的
    - 绝对定位中的包含块，就是离它最近的，开启了定位的祖先元素

#### 5.3 固定定位

- 固定定位也是一种绝对定位，所以固定定位的大部分特点都和绝对定位一样。唯一不同的是固定定位永远参照于游览器的视口进行定位，即固定定位的元素不会随网页的滚动而滚动






### 6. 水平/垂直方向布局

#### 6.1 水平方向布局

**一般水平布局**

- 元素在其父元素中水平方向的位置由以下等式决定：

  `margin-left + border-left + padding-left + width + padding-right + border-right + margin-right == [其父元素内容区的宽度]`

- 该等式两侧必须相等，若相加结果使等式不成立，则等式会根据下列规则自动调整

  - width，margin-left，margin-right可以被设置为auto，作为自动调整的候选值
  - 如果等式的7个值中没有值为auto，则自动调整margin-right值使等式满足
  - 如果某一个值为auto，则会自动调整该值使等式成立
  - 如果宽度和一个外边距设置为auto，则宽度变为最大值，外边距为0
  - 如果两个外边距设置为auto，则会将外边距设置为相等的值
  - 如果三个值都设置为auto，则宽度变为最大值，外边距为0

**开启绝对定位后的水平布局**

- 元素在其父元素中水平方向的位置由以下等式决定：

  `{left} + margin-left + border-left + padding-left + width + padding-right + border-right + margin-right + {right} == [其父元素内容区的宽度]`

- 规则和一般水平布局一样，但是margin-left/right被替换：

  - 可以设置为auto的值 margin width left right
  - 如果9个值中没有auto则自动调整right值以使等式满足

#### 6.2 垂直方向布局

**一般垂直方向布局**

- 一般垂直方向布局相对水平方向来说要简单很多，只需要注意溢出(overflow)问题即可

- 如果子元素的大小超过了父元素，则子元素会从父元素中移除，需要使用overflow样式解决

  语法：

  `overflow: visible`：默认值，子元素会从父元素中直接溢出，在父元素的外部位置显示

  `overflow: hidden `：溢出内容将会被裁剪而不会显示

  `overflow: scroll`：生成两个滚动条，通过滚动条查看完整的内容

  `overflow: auto`：根据需要生成滚动条

**开启绝对定位后的垂直方向布局**

- 元素在其父元素中垂直方向的位置由以下等式决定，且必须满足：

  `top + margin-top + padding-top + border-top + height + border-bottom + padding-bottom + margin-bottom + bottom == [包含块的高度]`



### 7. 背景

- `background-color`：设置背景颜色
- `background-image`：设置背景图片
  - 可以同时设置背景图片和背景颜色，这样背景颜色将会成为图片的背景色
  - 如果背景的图片小于元素，则背景图会自动在元素中平铺将元素铺满
  - 如果背景的图片大于元素，背景将无法完全显示

- `background-repeat`：用来设置背景的重复方式

  - `repeat` 默认值，背景会沿着x轴和y轴双方向重复
  - `repeat-x` 沿着x轴方向重复
  - `repeat-y` 沿着y轴方向重复
  - `no-repeat` 背景图片不重复

- `background-position`：用来设置背景图片的位置

  - 方式一

    通过 top left right bottom center 来设置背景图片的位置

    使用方位词时必须同时指定两个值，如果只写一个则第二个默认就是center

  - 方式二

    直接用 `background-position:(水平方向偏移量) (垂直方向偏移量)`

- `background-origin`：背景图片的偏移量计算的原点

  - `padding-box` 默认值，background-position从内边距处开始计算
  - `content-box` 背景图片的偏移量从内容区处计算
  - `border-box` 背景图片的变量从边框处开始计算

- `background-clip`：设置背景的范围

  - `border-box` 默认值，背景会出现在边框的下边
  - `padding-box` 背景不会出现在边框，只出现在内容区和内边距
  - `content-box` 背景只会出现在内容区

- `background-size`：设置背景图片的大小

  - 方式一

    `background-size: (width) (height)` 直接设置宽度和高度，如果只写一个，第二个值默认为auto

  - 方式二

    `background-size:cover` 图片比例不变，将元素铺满

    `background-size:contain` 图片比例不变，将图片在元素中完整显示

- `background-attachment`：背景图片是否跟随元素移动（滚动条下拉时背景图片是否移动）

  - `scroll` 默认值，背景图片会跟随元素移动
  - `fixed` 背景会固定在页面中，不会随元素移动
  
  ```css
  .box1{
      width: 500px;
      height: 500px;
      
      background-color: #bfa;
      background-image: url("./img/2.jpg");
      background-repeat: no-repeat;
      
      background-position: center center;
      background-size: contain;
  }
  
  .box3{
      width: 500px;
      height: 500px;
      /*简写设置背景*/
      background: url('./img/2.jpg') #bfa center center/contain border-box no-repeat
  }
  /*
  背景相关的简写属性，所有背景相关的样式都可以通过该样式来设置，且没有顺序要求，也不一定全都要写
  注意：
  	background-size必须写在background-position的后边，并且使用"/"隔开
  	background-origin必须写在background-clip的前面
  */
  ```
  



### 8. 雪碧图

- **定义和原理**：将多个小图片统一保存到大图片中，然后通过调整`background-position`来显示响应的图片，这样就能解决因加载延迟带来的图片闪烁问题

- **使用步骤**：

  1. 先确定要使用的图标
  2. 测量图标的大小
  3. 根据测量结果创建一个元素
  4. 将雪碧图设置为元素的背景图片
  5. 设置一个偏移量以显示正确的图片

  ```css
  a:link{
      display : block;
      width: 93px;
      height: 29px;
      background: url("背景.png");
      background-position: 0 0;
  }
  
  a:hover{
      background-position: -93px 0;
  }
  
  a:active{
      background-position: -186px 0;
  }
  ```



### 9. 过渡

- 通过过渡可以指定一个属性发生变化时的切换方式，通常用过渡制作一些简单的动画效果

- 语法：

  - `transition-property`：指定要执行过渡的属性
    - 可以同时指定多个属性，用逗号隔开
    - 如果所有属性都需要过渡，使用all关键字
    - 大部分属性都支持过渡效果
    - 过渡时必须从一个有效数值向另一个有效数值进行过渡（auto不算有效数值）
  - `transition-duration`：指定过渡效果的持续时间
  - `transition-delay`：延迟过渡效果，等待一段时间后再执行过渡
  - `transition-timing-function`：过度的时序函数
    - linear 匀速运动
    - ease 默认值，慢速开始，先加速后减速
    - ease-in 加速运动
    - ease-out 减速运动
    - ease-in-out 先加速后减速
    - cubic-bezier() 指定时序函数
    - steps() 分布执行过渡效果，可以设置第二个值：
      - end 在时间结束时执行过渡
      - start 在时间开始时执行过渡
  - `transition`：同时设置过渡相关的所有属性，无顺序要求



### 10. 动画

- 动画和过渡类似，都可以实现一些动态效果。不同的是过渡需要在某个属性发生变化时才会触发，动画可以自动触发动态效果

- 设置动画效果，需要先设置一个关键帧，关键帧设置了动画执行的每一个步骤

  ```css
  @keyframes test {
      from {
          margin-left: 0;
      }
  
      to {
          margin-left: 900px;
      }
  }
  ```

- 语法：

  - `animation-name`：指定动画的关键帧名称

  - `animation-duration`：指定动画效果的持续时间

  - `animation-delay`：动画效果的延迟，等待一段时间后再执行动画

  - `animation-timing-function`：动画的时序函数
  - `animation-iteration-count`：动画执行的次数
    - `infinite` 无限次执行
  - `animation-direction` 指定动画运行的方向
    - `normal` 从from向to运行，默认值
    - `reverse` 从to向from运行
    - `alternate` 从from向to运行，重复执行动画时反向执行
    - `alternate-reverse` 从to向from运行，重复执行动画时反向执行
  - `animation-play-state` 设置动画的执行状态
    - `running` 动画执行，默认值
    - `paused` 动画暂停
  - `animation-fill-mode` 动画的填充模式
    - `none` 动画执行完毕，元素回到原来位置
    - `forwards` 动画执行完毕，元素停止在动画结束的位置
    - `backwards` 动画延时等待时，元素就会处在开始位置
    - `both` 结合了forwards和backwards



### 11. 变形：平移，旋转与缩放

- 变形是指通过css来改变元素的形状或位置，变形不会影响到页面的布局

  `transform`用来设置元素的变形效果

#### 11.1 平移

- **语法**

  `translateX()` 沿着x轴方向平移

  `translateY()` 沿着y轴方向平移

  `translateZ()` 沿着z轴方向平移

- **Z轴平移**

  Z轴平移，正常情况就是调整元素和人眼之间的距离，距离越大，元素离人越近

  Z轴平移属于立体效果，默认情况下网页不支持透视，需要设置网页的视距

  ```css
  html {
      background-color: rgb(71, 44, 32);
      perspective: 800px;
  }
  ```

- **几种水平垂直居中的方式对比**

1. 绝对定位的方式

   ```css
   /* 这种居中方式，只适用于元素的大小确定时 */
   position: absolute;
   top: 0;
   left: 0;
   bottom: 0;
   right: 0;
   margin: auto;
   ```

2. table-cell的方式

   ```css
   /* table-cell的方式具有一定局限性 */
   display: table-cell;
   vertical-align: middle;
   text-align: center;
   ```

3. transform的方式

   ```css
   /* transform变形平移的方式 */
   position: absolute;
   left: 50%;
   top: 50%;
   transform: translateX(-50%) translateY(-50%);
   ```

#### 11.2 旋转

- **语法**

  `rotateX()` 绕x轴旋转

  `rotateY()` 绕y轴旋转

  `rotateZ()` 绕z轴旋转

#### 11.3 缩放

- **语法**

  `scaleX()`水平方向缩放

  `scaleY()`垂直方向缩放

  `scale()` 双方向缩放



### 12. less

- less是一门css的预处理语言，通过less可以编写更少的代码实现更强大的样式

  由于less增添了许多对css的扩展，所以游览器无法直接执行less代码，执行前必须将less转换为css，然后由游览器执行

#### 12.1 less注释

```less
// `less`中的单行注释，注释中的内容不会被解析到`css`中

/*
`css`中的注释，内容会被解析到`css`文件中
*/
```



#### 12.2 父子关系嵌套

```less
body {
  height: 100px;
  width: 100px;
  div {
    height: 100px;
    width: 100px;
  }
  .box1 {
    background-color: #bfa;
    .box2 {
      background-color: red;
      >.box4{
        background-color: green;
      }
    }  
  }
}
```



#### 12.3 变量

- **语法**
  - 直接使用变量时，以`@变量名`的形式使用即可
  - 作为类名，属性名或一部分值使用时，必须以`@{变量名}`的形式使用

```less
@b1:box1;
@size:200px;
@color:red;
@bc:background-color;
@bi:background-image;
@path:image/a/b/c;

.@{b1}{
  width: @size;
  height: $width;
  @{bc}: @color;
  @{bi}: url("@{path}/1.png");
}
```



#### 12.4 混合函数

```less
//在函数形参处可设置默认值
.test(@w:200px, @h:100px, @bc:red){
  width: @w;
  height: @h;
  background-color: @bc;
}

.p6{
  // .test(200px, 100px, red); // 对应参数位传值，常规
  // .test(@h:200px,@w:100px,@bc:red); // 写明对应属性，可不按照顺序传参
  // .test(); //完全使用默认参数
  .test(300px); //部分使用默认参数
}
```



#### 12.5 其它

- `&` 拼接符，代表当前选择器的名字

  `:extend()` 继承括号内的选择器指定的样式

  `.p1()` 直接对指定的样式进行引用，相当于将p1的样式进行了赋值

```less
.p1{
  width: @size;
  height: $width;
  //相当于.p1-wrapper
  &-wrapper{
    background-color: peru;
  }
}
.p2:extend(.p1){
  color:@color;
}
.p3{
  .p1();
}
```



## 13. 弹性盒

- flex弹性盒是css中的一种布局手段，主要用来代替浮动来完成页面的布局。flex可以让元素随页面大小的改变而改变

### 13.1 弹性容器

- 使用display来设置弹性容器：
  - `flex` 设置为块级弹性容器
  - `inline-flex` 设置为行内的弹性容器

#### 主轴

- 弹性元素的排列方向称为主轴

- 常用属性：

  - `flex-direction` 设置容器中弹性元素的排列方式
    - `row` 默认值，弹性元素在容器中自左向右水平排列
    - `row-reverse` 弹性元素在容器中自右向左水平排列
    - `column` 弹性元素自上向下纵向排列
    - `column-reverse` 弹性元素自下向上纵向排列
  - `flex-wrap` 设置弹性元素是否在弹性容器中自动换行
    - `nowrap` 默认值，元素不会自动换行
    - `wrap` 元素沿着辅轴方向自动换行
  - `flex-flow` 可以同时设置`flex-direction` 和`flex-wrap` 

  - `justify-content` 设置如何分配主轴上的（空白）空间
    - `flex-start` 元素沿着主轴的起始边开始排列
    - `flex-end` 元素沿着主轴的终边开始排列
    - `center` 元素居中排列
    - `space-around` 空白分布到元素两侧
    - `space-between` 空白均匀分布到元素中间
    - `space-evenly` 空白分布到元素的单侧

#### 辅轴

- 与主轴方向垂直的方向称为辅轴

- 常用属性：
  - `align-items` 设置元素在辅轴上如何对齐
    - `stretch` 默认值，将元素的长度设置为相同的值
    - `flex-start` 元素不会拉伸，沿着辅轴的起始边对齐
    - `flex-end` 沿着辅轴的终边对齐
    - `center` 居中对齐
    - `baseline` 基线对齐
  - `align-content` 如何分配辅轴上的（空白）空间
    - `flex-start` 元素沿着辅轴的起始边开始排列
    - `flex-end` 元素沿着辅轴的终边开始排列
    - `center` 元素居中排列
    - `space-around` 空白分布到元素两侧
    - `space-between` 空白均匀分布到元素中间
    - `space-evenly` 空白分布到元素的单侧

**弹性居中**

```css
justify-content: center;
align-items: center;
```



### 13.2 弹性元素

- 弹性容器的子元素就是弹性元素，弹性元素同时也可以是弹性容器
- 常用属性：
  - `flex-grow` 指定弹性元素的伸展系数，默认为0
    - 当父元素有多余空间是，子元素根据系数按比例分配空间
  - `flex-shrink` 指定弹性元素的收缩系数，默认值为1
    - 当父元素内空间不足以容纳所有子元素时，子元素根据系数按比例分配空间
  - `order` 设置弹性元素的排列顺序



## 14. 移动端适配

### 14.1 vw适配

- 移动端开发中，由于不同设备视口和像素比不同，所以不能单纯使用px作为单位，需要使用vw做适配

  vw表示的是视口的宽度，1vw = 1%视口宽度。假设设计图为750px，则1px = 0.1333333vw


- 实际开发中，会将html的font-size作为换算的介质，即`font-size = 0.133333vw`

  这样，当需要一个750px的样式时，就可以直接使用750rem表示

- 但由于字体的大小最低不能低于12px，所以一般会将值扩大40倍，即`font-size = 5.133333vw`

  所以最终需要一个750px的样式时，就使用(750/40)rem表示



## 14.2 响应式设计

- 网页可以根据不同的设备或窗口大小呈现出不同的效果，使用响应式布局可以使一个网页适用于所有设备。需要使用媒体查询，为不同的设备或设备的不同状态来分别设计样式

- **媒体特性**

  - width 视口的宽度
  - height 视口的高度
  - min-width 视口的最小宽度（视口大于指定宽度时生效）
  - max-width 视口的最大宽度（视口小于指定宽度时生效）

  ```css
  @media (min-width:500px){
      body{
        background-color: #bfa;  
      }
  }
  
  /*
  	取交集使用 and 取并集使用 or
  	(min-width:500px) and (max-width:700px) 视口宽度大于500且小于700时生效
  	(min-width:700px) or (max-width:500px)  视口宽度大于700或小于500时生效
  */
  ```

  



## 15. 媒体查询


## 14. CSS其它知识点

### 【概念】

#### 1. 继承

- 为一个元素设置的样式同时也会应用到它的后代元素上，这种特性称之为样式的继承

  继承发生在祖先和后代之间，利用继承可以将一些通用的样式统一设置到共同的祖先元素上

- 并不是所有的样式都会被继承，如背景样式，布局样式等都不会被继承

#### 2. 选择器权重

- 当通过不同的选择器，选中相同的元素，并且为相同的样式设置不同的值时，此时便发生了样式的冲突。此时就需要根据"选择器权重"判断使用哪个样式

  | 选择器种类     | 权重       |
  | -------------- | ---------- |
  | 内联样式       | 1000       |
  | id选择器       | 100        |
  | 类和伪类选择器 | 10         |
  | 元素选择器     | 1          |
  | 通配选择器     | 0          |
  | 继承的样式     | 没有优先级 |

- 比较优先级时，需要将所有的选择器的优先级进行相加计算，最后显示优先级高的

  选择器的累加不会超过其最大的数量级，如类选择器的优先级再高也不会比id选择器高

  如果优先级计算后相同，则此时优先使用靠下的样式

#### 3. 长度单位

- 像素：不同屏幕的像素大小不同，像素越小越高清的屏幕显示的效果越清晰

- 百分比：设定百分比可以使子元素跟随父元素的改变而改变

- em：em是相对于元素的字体大小来计算的(1em = 1font-size)，em的大小随字体大小改变

- rem：rem是相对于根元素的字体大小来计算


#### 4. 外边距的折叠

- 相邻的垂直方向外边距会发生重叠现象

- 兄弟元素：兄弟元素间的相邻垂直外边距会取两者之间的较大值（无需处理）

  父子元素：父子元素相邻外边距，子元素的外边距会传递给父元素（需要处理）

#### 5. 简写属性中的上下左右

- 本质是按顺时针方向设置的

  - 一个值：上下左右

  - 两个值：上下 左右

  - 三个值：上 左右 下

  - 四个值：上 右 下 左

### 【属性】

#### 1. display和visibility

- display用来设置元素的显示类型

  - `inline` 将元素设置为行内元素

  - `block` 将元素设置为块元素

  - `inline-block` 将元素设置为行内块元素，既可以设置宽度和高度又不会独占一行

  - `table` 将元素设置为一个表格

  - `none` 元素不在页面中显示

- visibility用来设置元素的显示状态

  - `visible` 默认值，元素在页面中正常显示
  - `hidden` 元素在页面中隐藏不显示，但是依然占据页面的位置

#### 2. box-sizing

- box-sizing用来设置盒子尺寸的计算方式。盒子的可见框大小由内容区，内边距和边框共同决定
- 语法：
  - `box-sizing: content-box`：width和height用来设置内容区的大小
  - `box-sizing: border-box`：width和height用来设置整个盒子可见框的大小

#### 3. 轮廓，阴影和圆角

**3.1 轮廓**

- 用来设置元素的轮廓线，用法和border类似，但轮廓不会影响到可见框的大小
- 语法：`outline: [粗度] [颜色] [样式]`

**3.2 阴影**

- 用来设置元素的阴影效果，阴影效果不会影响页面布局
- 语法：`box-shadow: [水平偏移量] [垂直偏移量] [阴影的模糊半径] [阴影的颜色]`

**3.3 圆角**

- 用来设置圆角
- 语法：
  - 设定圆角：`border-radius: [px/%]`
  - 设定指定圆角：`border-(top-left/top-right/bottom-left/bottom-right)-radius: [px/%] `

#### 4. 元素层级

- 对于开启了定位的元素，可以通过z-index属性来指定元素的层级
  - z-index值越大，元素的层级越高，元素越优先显示
  - 若层级一样，则优先显示靠下的元素
  - 祖先元素的层级再高也无法覆盖后代元素

#### 5. 行高

- 行高是指文字占有的实际高度，它会在字体框的上下平均分配

- 可以通过line-height来设置行高。行高可以指定一个具体大小（px em）

  也可以直接为行高设置一个整数，行高将会是字体的指定的倍数

- 行高还经常用来设置文字的行间距，行间距=行高-字体大小

- 可以将行高设置为和高度一样的值，使单行文字在一个元素中垂直居中

#### 6. 文本的水平和垂直对齐

- `text-align` 设置文本的水平对齐方式
  - `text-align:left` 左侧对齐
  - `text-align:right` 右侧对齐
  - `text-align:center` 居中对齐
  - `text-align:justify` 两端对齐
- `vertical-align` 设置文本的垂直对齐方式
  - `vertical-align:baseline` 默认值，基线对齐
  - `vertical-align:top`顶部对齐
  - `vertical-align:bottom`底部对齐
  - `vertical-align:middle`居中对齐

#### 7. 文本修饰

- `text-decoration`设置文本修饰
  - `text-decoration:none` 什么都没有
  - `text-decoration:underline` 下划线
  - `text-decoration:line-through` 删除线
  - `text-decoration:overline` 上划线
