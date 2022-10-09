# Vue

[toc]

## 1. 初识Vue

- 先看一个示例

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<body>
    <div id="index">
        <p>{{ message }}</p>
    </div>
    <script src="./js/vue.js"></script>
    <script>
        new Vue({
            //el用于指定当前Vue实例为哪个容器服务，值通常为css选择器字符串
            el: '#index',
            //data中用于存储数据，数据供el所指定的容器去使用
            data:{
                message:'Hello Vue!'
            }
        })
    </script>
</body>
</html>
```

- 知识点总结：
  1. 想让Vue生效，必须创建一个Vue实例，且需要传入一个配置对象
  2. root容器中的代码依旧符合html规范，只不过混入了一些特殊的Vue语法
  3. root容器中的代码被称为【Vue模板】
  4. Vue实例和容器是一一对应和
  5. 真实开发中只有一个Vue实例，并且会配合着组件一起使用
  6. {{xxx}}中的xxx要写js表达式，且xxx可以自动读取到data中的所有属性
  7. 一旦data中的数据发生改变，那么模板中用到该数据的地方自动更新

## 2. 基础知识

### 2.1 模板语法

- Vue模板语法有两大类：

  1. **插值语法**

     功能：用于解析标签体内容

     语法：`{{xxx}}`，xxx代表js表达式，插值语法可以直接读取到data中的所有属性

  2. **指令语法**

     功能：用于解析标签（包括标签属性、标签体内容、绑定事件......）

     举例：`v-bind:href="xxx"`

     ```html
     <div id="index">
     	<!--插值语法-->
     	<p>{{ message }}</p>
         
     	<!--指令语法,会自动解析href内的内容-->
     	<a v-bind:href="school.url.toUpperCase()">超链接</a>
     </div>
     ```

     ```html
     <script>
     	new Vue({
     		el: '#index',
     		data:{
     			message:'Hello Vue!'
     			school:{
     				url:'https://www.baidu.com/'
     			}
     		}
     	})
     </script>
     ```

     

### 2.2 数据绑定

- Vue中有两种数据绑定的方式

  1. **单向绑定**(v-bind)：数据只能从data流向页面
  2. **双向绑定**(v-model)：数据不仅能从data流向页面，还可以从页面流向data
     - 双向绑定一般只能应用在表单类元素上（如input、select等）

  ```html
  <div id="root">
      <!--单向数据绑定-->
      <input type="text" v-bind:value="name">
      <!--双向数据绑定-->
      <input type="text" v-model:value="name">
  </div>
  <!--vue写法与上相同-->
  ```

#### 2.2.1 v-model

- 若`<input type="text"/>` 则v-model收集的是value值，用户输入的内容就是value值
- 若`<input type="radio/">` 则v-model收集的是value值，且要给标签配置value属性，如果不配置收集到的将是null
- 若`<input type="checkbox"/>`
  - 没有配置value属性，那么收集的是checked属性（true/false）
  - 配置了value属性且v-model的初始值是**数组**，那么手机的就是value组成的数组
- **v-model的修饰符**
  - `lazy` 失去焦点后再收集数据
  - `number` 将输入的字符串转换为有效的数字
  - `trim` 输入首尾空格过滤



### 2.3 el和data的两种写法

- **el的两种写法**

  1. new Vue的时候就配置el属性
  2.  先创建Vue实例，随后通过vm.$mount('#root')指定el的值

  ```html
  <script>
  <!--方式1-->
  const v = new Vue({
      el:'#root'
  })
  
  <!--方式2-->
  v.$mount('#root')
  </script>
  ```

  

- **data的两种写法**

  1. 对象式
  2. 函数式
     - 在使用组件时，data必须使用函数式，否则会报错

  ```html
  <script>
  <!--方式1-->
  data:{
      name:'jack'
  }
  <!--方式2-->
  data:function(){
      return{
          name:'jack'
      }
  }
  </script>
  ```

  

### 2.4 MVVM模型

- M：模型(Model)，对应data

- V：视图(View)，模板

- VM：视图模型(ViewModel)，实例对象

  <img src="C:\Users\20723\Desktop\学习\vue\笔记\MVVM.png" style="zoom:50%;" />



### 2.5 **数据代理**

- 数据代理可以通过vm对象来代理data对象中属性的操作（读/写），能更加方便的操作data中的数据
- 基本原理：通过Object.defineProperty()把data对象中所有属性添加到vm上，再为每一个添加到vm上的属性，都指定一个getter/setter，在getter/setter内部去操作data中对应的属性



## 3. 事件处理

### 3.1 基本使用

- 使用 `v-on:xxx` 或 `@xxx` 绑定事件，其中xxx是事件名

- 事件的回调需要配置在methods对象中，最终会在vm上

- methods中配置的函数，都是被Vue所管理的函数，this指向的是vm或组件实例对象

- `@click="demo"` 和 `@click="demo($event)"` 效果一致，但后者可以传递额外参数

  ```html
  <div id="root">
  <button @click="showInfo1">点击获取提示信息1</button>
  <button @click="showInfo2(66,"hi",$event)">点击获取提示信息2</button>
  </div>
  ```

  ```html
  <script>
  	const vm = new Vue({
          el:'#root',
          data:{
              name:"Jack"
          },
          method:{
              showInfo(event){
                  alert('hi')
              },
              showInfo2(String,number,event){
                  alert(String)
              }
          }
      })
  </script>
  ```



### 3.2 事件修饰符

- prevent：阻止默认事件（常用）

- stop：阻止事件冒泡（常用）

- once：事件只触发一次（常用）

- capture：使用事件的捕获模式

- self：只有event.target是当前操作的元素才触发事件

- passive：事件的默认行为立即执行，无需等待事件回调执行完毕

  ```html
  <div id="root">
      <div class="demo1" @click="showInfo">
          <!--原来点击超链接会在提示信息后跳转，现在不会跳转-->
          <a href="https://www.baidu.com/" @click.prevent="showInfo">点击提示信息</a>
          <!--原来点击后，由于冒泡会提示两次，现在只会提示一次-->
      	<button @click.stop="showInfo">点击提示信息</button>
          <!--事件只触发一次-->
          <button @click.once="showInfo">点我提示信息</button>
          
          <!--链式修饰-->
          <button @click.once.prevent.stop="showInfo">点我提示信息</button>
      </div>
  </div>
  ```



### 3.3 键盘事件

- Vue中常用的按键别名：

  | 回车   | Enter  | 删除   | delete   | 退出   | Esc      | 空格   | Space     | 换行 | tab  |
  | ------ | ------ | ------ | -------- | ------ | -------- | ------ | --------- | ---- | ---- |
  | **上** | **up** | **下** | **down** | **左** | **left** | **右** | **right** |      |      |

- Vue未提供别名的按键，可以使用按键原始的key值去绑定，但是注意要转为kebab-case（短横线命名）

  - 比如`CAPS LOCKS`键，就要书写为`caps-locks`

- 系统修饰键

  系统修饰键包含：ctrl、alt、shift、meta，它们的使用方法比较特殊

  - 配合keyup使用：按下修饰键的同时，再按下其它键，随后释放其它键，事件才会被触发
  - 配合keydown使用：正常触发事件

- `Vue.config.keyCodes.自定义键名 = 键码` 可以去定制按键别名

  ```html
  <div id="root">
      <!--按下回车键执行showInfo1方法-->
      <input type="text" @keydown.enter="showInfo1">
      <!--同时按下ctrl键和y键执行showInfo2方法-->
      <input type="text" @keyup.ctrl.y="showInfo2">
  </div>
  ```



## 4. 计算属性和监视属性

### 4.1 计算属性

- **定义**：使用的属性起初并不存在，要通过已有属性计算得来，这样的属性称作计算属性

- **原理**：底层借助了Object.defineproperty方法提供的getter和setter

  - get函数会在初次读取时执行一次；当依赖的数据发生改变时会被再次调用

- 与methods实现相比，内部有缓存机制以供复用，效率更高，调试方便

  计算属性最终会出现在vm上，直接读取使用即可

  如果计算属性要被修改，那必须写set函数去响应修改，且set中要引起计算时依赖的数据发生

  ```html
  <div id="root">
      姓: <input type="text" v-model="firstName"> <br/><br/>
      名: <input type="text" v-model="lastName"> <br/><br/>
      全名: <span>{{fullName}}</span>
  </div>
  ```

  ```html
  <script>
  	new Vue({
          el:'#root',
          data:{
              firstName:'张',
              lastName:'三'
          },
          computed:{
              fullName:{
                  get(){
                      return this,firstName + '-' + this.lastName;
                  },
                  set(value){
                      const arr = value.split('-')
                      this.firstName = arr[0]
                      this.lastName = arr[1]
                  }
              }
          }
      })
  </script>
  ```

  



### 4.2 监视属性

- **定义**：当有数据需要随着其它数据变动而变动，需要设置监视属性。当被监视的属性变化时，回调函数自动调用，进行相关操作

- 监视的属性必须存在，才能进行监视

- 监视的两种写法：

  - new Vue时传入watch配置
  - 通过vm.$watch监视

  ```html
  <div id="root">
      <h2>今天天气很{{info}}</h2>
      <button @click="changeWeather">切换天气</button>
  </div>
  ```

  ```html
  <script>
  	new Vue({
          el:'#root',
          data:{
              isHot:true
          },
          method{
          	changeWeather(){
          		this.isHot = !this.isHot
      		}
      	},
          computed:{
              info(){
                  return this.isHot ? '炎热' : '凉爽'
              }
          },
          watch:{
              'isHot':{
                  immediate:true, //初始化时让handler调用一下
                  handler(newVal,oldVal){
                      console.log('isHot被修改',newVal,oldVal)
                  }
              }
          }
      })
  
      /*
      写法二：
      vm.$watch('isHot',{
          immediate:true, //初始化时让handler调用一下
          handler(newVal,oldVal){
          	console.log('isHot被修改',newVal,oldVal)
          }
      })
      */
  </script>
  ```

#### 4.2.1 深度监视

- Vue中的watch默认不监测对象内部值的改变（一层）

  配置deep:true可以监测对象内部值改变（多层）

- Vue自身可以监测对象内部值的改变，但Vue提供的watch默认不可以

  使用watch时根据数据的具体结构，决定是否使用深度监视

  ```html
  <script>
  	new Vue({
          el:'#root',
          data:{
              numbers:{
                  a:1,
                  b:1
              }
          },
          watch:{
              'numbers':{
                  deep:true,
                  handler(){
                      console.log('numbers改变了')
                  }
              }
          }
      })
  </script>
  ```

  

### 4.3 computed和watch对比

- computed能完成的功能，watch都能完成

  watch能完成的功能，computed不一定能完成。如异步操作

  但介于computed比watch方便得多，所以优先使用computed

- 所有被Vue管理的函数，最好写成普通函数，这样this的指向才是vm或组件实例对象

  所有不被Vue所管理的函数（如定时器的回调函数，ajax的回调函数等），最好写成箭头函数，这样this的指向才是vm或组件实例对象



## 5. 绑定样式

### 5.1 绑定class样式

- **语法**：`class="xxx"` xxx可以是字符串、对象、数组
  - 字符串写法适用于：类名不确定，要动态获取
  - 对象写法适用于：要绑定多个样式，个数不确定，名字也不确定
  - 数组写法适用于：要绑定多个样式，个数确定，名字确定，但不确实是否使用

```html
<div id="root">
    <!--字符串写法-->
    <div class="basic" :class="mood" @click="changeMood">{{name}}</div><br/>
    <!--对象写法-->
    <div class="basic" :class="classArr">{{name}}</div><br/>
    <!--数组写法-->
    <div class="basic" :class="classObj">{{name}}</div><br/>
</div>
```

```html
<script>
	new Vue({
        el:'#root',
        data:{
            name:'Jack',
            mood:'normal',
            classArr:['class_1','class_2','class_3'],
            classObj:{
                atguigu1:false,
                atguigu2:false.
            }
        },
        methods:{
            changeMood(){
                const arr = ]['happy','sad','normal']
                const index = Math.floor(Math.random()*3)
                this.mood = arr[index]
            }
        }
    })
</script>
```



### 5.2 绑定style样式

- **语法**：`:style="xxx"` 其中xxx可以是字符串、对象、数组

```html
<div id="root">
    <!--对象写法-->
    <div class="basic" :style="styleObj">{{name}}</div><br/>
    <!--数组写法,同时使用两个样式-->
    <div class="basic" :style="[styleObj,styleObj2]">{{name}}</div><br/>
</div>
```

```html
<script>
	new Vue({
        el:'#root',
        data:{
            name:'Jack',
            mood:'normal',
            styleObj:{
                fontSize: '40px',
                color:'red',
            },
            styleObj2:{
                backgroundColor:'orange'
            }
        }
    })
</script>
```



## 6. 条件渲染和列表渲染

### 6.1 条件渲染

#### 6.1.1 v-if

- 适用于元素切换频率较低的场景，不展示的DOM元素直接被移除

- **语法**：

  `v-if = "表达式"`

  `v-else-if = "表达式"`

  `v-else = "表达式"`

  - 以上三种语法可以混合使用，但必须连在一起，不能隔断

#### 6.1.2 v-show

- 适用于切换频率较高的场景，不展示的DOM元素未被移除，仅仅是使用样式隐藏掉
- **语法**：`v-show = "表达式"`

```html
<div id="root">
    
    <button @click="n++">点我n+1</button>
    
    <div v-show="n===1">Angular</div>
    <div v-show="n===2">React</div>
    <div v-show="n===3">Vue</div>
    
    <div v-if="n===1">Angular</div>
    <div v-else-if="n===2">React</div>
    <div v-else>React</div>
    
    <!--v-if和template的配合使用-->
    <!--template标签不影响结构，html页面中不会显示此标签-->
    <template v-if="n===1">
        <h3>你好</h3>
        <h3>你好</h3>
    </template>
</div>
```

```html
<script type="text/javascript">
	new Vue({
        el:'#root',
        data:{
            name:'尚硅谷'
            n:0
        }
    })
</script>
```



### 6.2 列表渲染

#### 6.2.1 v-for指令

- v-for用于展示列表数据，可用于遍历数组、对象、字符串、指定次数
- **语法**：`v-for="(item,index) of items" :key="(唯一标识)"`

```html
<div id="root">
	<!--遍历数组-->
    <ul>
    	<li v-for="(p,index) of persons" :key="index">{{p.name}}-{{p.age}}</li>
    </ul>
    <!--遍历对象-->
    <ul>
    	<li v-for="(value,key_) of persons" :key="key_">{{k}}-{{value}}</li>
    </ul>
</div>
```



#### 6.2.2 key的作用与原理

- **虚拟DOM中key的作用**
  - key是虚拟DOM对象的标识，当数据发生变化时，Vue会根据【新数据】生成【新的虚拟DOM】
  - 随后Vue按照**比较规则**进行【新虚拟DOM】与【旧虚拟DOM】的差异比较

- **比较规则**
  - 若旧虚拟DOM中找到了与新虚拟DOM相同的key：
    - 若虚拟DOM中内容没变，则之间使用之前的真实DOM
    - 若虚拟DOM中内容变化，则生成新的真实DOM，替换掉页面中的旧DOM
  - 若旧虚拟DOM中未找到与新虚拟DOM相同的key
    - 创建新的真实DOM，随后渲染到页面
- 用index作为key可能会引发的问题
  - 若对数据进行破坏顺序的操作，会产生没有必要的真实DOM更新，界面正常，但效率低下
  - 若之前的操作，结构中还带有输入的DOM，会产生DOM更新错误，界面出现问题

**代码请参考练习01**



### 6.3 其它内置指令

这里补充的大多数都没什么用

- **v-text**：向其所在的节点中渲染文本内容。v-text会替换掉节点中的内容，{{xxx}}不会
- **v-html**：向指定节点中渲染包含`html`结构的内容。v-html可以识别html结构，会替换掉节点中所有内容。不安全，不建议使用
- **v-cloak**：本质是一个特殊属性，Vue实例创建完毕接管容器后，会自动删除v-cloak属性。使用css配合v-cloak可以解决网速慢时页面展示出骨架的问题
- **v-once：**所在节点在初次动态渲染后，就视为静态内容
- **v-pre**：跳过v-pre所在节点的编译过程，从而加快编译



## 7. **数据监视**

- vue会监视data中所有层次的数据
- **监测对象中的数据**
  
  - 通过setter实现监视，且在new Vue()时就传入要监测的数据
  - 对象创建后追加的属性，Vue默认不做响应式处理
  - 如需后续添加响应式属性，可以使用API`Vue.set(target,propertyName/index,value)`
- **监测数组中的数据**
  - 通过包裹数组更新元素的方法实现，本质做了两件事：
    - 调用原生对应的方法对数组进行更新
    - 重新解析模板，进而更新页面
  - 在Vue修改数组中的某个元素必须使用以下方法：
    - push()，pop()，unshift()，shift()，splice()，sort()，reverse()
    - 这几个方法重写了Vue.set()
- **注意**：Vue.set()不能给vm或vm的根数据对象（如data，computed等）添加属性

- **总结**：

  问题主要集中在后期数据添加或修改时能否被Vue检测到

  - 对于一般对象中的属性：
    - 不能直接添加，需要通过Vue.set()设置
    - 若该属性已经具有响应式，可以直接修改
  - 对于数组中的数据（带索引的元素）：
    - 不能直接添加，需要通过七个数组操作方法设置
    - 不能直接修改，需要通过Vue.set()设置
    - 对于数组中的数据的对象属性，则又遵从对象属性规则

```html
<div id="root">
    <h1>学生信息</h1>
    <button @click="addSex">添加性别属性，默认值：男</button> <br/>
    <button @click="addFriend">在列表首位添加一个朋友</button> <br/>
    <button @click="updateFirstFriendName">修改第一个朋友的名字为：张三</button> <br/>
    <button @click="addHobby">添加一个爱好</button> <br/>
    <button @click="updateHobby">修改第一个爱好为：开车</button> <br/>
    <button @click="removeSmoke">过滤掉爱好中的抽烟</button> <br/>
    <h3>姓名：{{student.name}}</h3>
    <h3>年龄：{{student.age}}</h3>
    <h3 v-if="student.sex">性别：{{student.sex}}</h3>
    <h3>爱好：</h3>
    <ul>
        <li v-for="(h,index) in student.hobby" :key="index">
            {{h}}
        </li>
    </ul>
    <h3>朋友们：</h3>
    <ul>
        <li v-for="(f,index) in student.friends" :key="index">
            {{f.name}}--{{f.age}}
        </li>
    </ul>

</div>
```

```html
<script type="text/javascript">
    Vue.config.productionTip = false;
    const vm = new Vue({
       el: '#root',
       data:{
           student:{
               name:'tom',
               age:18,
               hobby:['抽烟','喝酒','烫头'],
               friends:[
                   {name:'jerry',age:35},
                   {name:'tony',age:36}
               ]
           }
       },
       methods:{
           addSex(){
               this.$set(this.student, 'sex', '男');
           },
           addFriend(){
               this.student.friends.unshift({
                   name:'jack',
                   age:70,
               })
           },
           updateFirstFriendName(){
               this.student.friends[0].name = '张三'
           },
           addHobby(){
               this.student.hobby.push('学习');
           },
           updateHobby(){
               this.$set(this.student.hobby, 0, '开车');
           },
           removeSmoke(){
               //直接替换
               this.student.hobby = this.student.hobby.filter(h => h !== '抽烟');
           }
       }
    });
</script>
```



## 8. **生命周期**

- **生命周期**又称为生命周期回调函数，生命周期钩子，是Vue在关键时刻帮我们调用的一些特殊名称的函数。

  生命周期函数的名字不可更改，但函数的具体内容是程序员根据需求编写的。

  生命周期函数中的this的是vm或组件实例对象。

- 常用的生命周期钩子

  - mounted发送ajax请求，启动定时器，绑定自定义事件，订阅信息等初始化操作
  - beforeDestroy清除定时器，解绑自定义事件，取消订阅消息等收尾工作

- 关于销毁Vue实例

  - 销毁后借助Vue开发者工具看不到任何消息
  - 销毁后自定义事件会失效，但原生Dom事件仍然有效
  - 一般不会再beforeDestroy操作数据，即使操作数据，也不会再触发更新流程了

```html
<div id="root">
    <h2 v-text="n"></h2>
    <h2>当前的n值是:{{n}}</h2>
    <button @click="add">点我n+1</button>
    <button @click="bye">点我销毁vm</button>
</div>
```

```html
<script>
	new Vue({
        el: '#root',
        data:{
            n: 1
        },
        methods:{
            add(){
                console,log('add')
            	this.n++
            },
            bye(){
                console.log('bye')
                this.$destroy()
            }
        },
        watch:{
            n(){
                console.log('n变了')
            }
        },
        //以下全都是生命周期函数
        beforeCreate(){console.log('beforeCreate')},
        created(){console.log('created')},
        beforeMount(){console.log('beforeMount')},
        mounted(){console.log('mounted')},
        beforeUpdate(){console.log('beforeUpdate')},
        updated(){console.log('updated')},
        beforeDestroy(){console.log('beforeDestroy')},
        destroyed(){console.log('destroyed')}
    })
</script>
```



![](C:\Users\20723\Desktop\学习\vue\笔记\生命周期.png)



## 9. 组件

- **定义**

  **组件是用来实现局部功能的代码和资源的集合**

  组件可以提高编码复用率，简化项目编码，提高运行效率

  当应用中的功能都是多组件的方式来编写的，那这个应用就是一个组件化的应用

- 非单文件组件：一个文件中包含有n个组件

  单文件组件：一个文件中只包含1个组件

### 9.1 组件的基本使用

1. **定义组件**

   使用`Vue.extend(options)`创建，其中`options`和`new Vue(options)`时传入的几乎一样，区别如下

   - `el` 不能写，因为最终所有的组件都要经过同一个vm的管理，由vm中的el决定服务哪个容器
   - `data` 必须写成函数，避免组件被复用时，数据存在引用关系

2. **注册组件**

   - 局部注册：`new Vue()`的时候`options`传入`components`选项
   - 全局注册：`Vue.component('组件名',组件)`

3. **使用组件**

   - 编写标签如`<school></school>`

### 9.2 组件注意事项

- **关于组件名**

  - 一个单词组成，首字母小写（school）
  - 多个单词组成，kebab-case命名（my-school）
  - 组件名尽可能回避HTML中已有的元素名称，如h2，H2等
  - 可以使用name配置项指定组件在开发者工具中呈现的名字

- **关于组件标签**

  - 第一种写法`<school></school>`
  - 第二种写法`<school/>`（需要Vue脚手架支持）

- **关于创建组件**

  声明组件可以将`const school = Vue.extend(options)` 简写为 `const school = options`

```html
<div id="root">
    <h2>{{msg}}</h2>
    <school></school>
</div>
```

```html
<script>
	//定义组件
    const school = Vue.extend({
        name: 'atguigu',
        template:`
        	<div>
        		<h3>学校名称:{{name}}</h3>
        		<h3>学校名称:{{address}}</h3>
			</div>
        `,
        data(){
            return{
                name:'电子科技大学',
                address:'成都'
            }
        },
    })
    
    //全局注册组件
    //Vue.component('school',school)
    
    //定义vm本体
    new Vue({
        el:'#root',
        data:{
            msg:'hello'
        },
        //局部注册组件
        components:{
            school
        }
    })
</script>
```

### 9.3 VueComponent()

- 组件的本质是一个名为`VueComponent`的构造函数，这个函数不是程序员手动定义的，而是由`Vue.extend()`生成
- 只需要写`<school/>`，Vue解析时就会帮忙创建一个school组件的实例对象，即Vue帮忙执行`new VueComponent(options)`
- 每次调用`Vue.extend`，返回的都是一个全新的VueComponent，即不同组件是不同的对象
- 关于this指向
  - 组件配置中，data函数，methods中的函数，watch中的函数，computed中的函数，它们的this均是VueComponent实例对象
  - `new Vue(options)`配置中，以上函数中的this均是Vue实例对象

- **一个重要的内置关系**

  `VueComponent.prototype.__proto__ === Vue.prototype`

  这种关系可以让组件实例对象访问到Vue原型上的属性和方法







## 简写形式

- `v-bind:` → `:`

  例：`<a v-bind:href="xxx">` → `<a :href="xxx">`

- `v-model:value` → `v-model` （因为v-model默认收集的就是value值）

  例：`<input type="text" v-model:value="name">` → `<input type="text" v-model="name">`

- `v-on` → `@`

  例：`<button v-on:click="showInfo1">点击获取提示信息1</button>` → `<button @:click="showInfo1">点击获取提示信息1</button>`

- **计算属性**

  由于计算属性一般不需要获取，所以可以不设置setter函数

  ```html
  <!--完整写法-->
  <script>
  new Vue({
      el:'#root',
      data:{
          firstName:'张',
          lastName:'三'
      },
      computed:{
          fullName:{
              get(){
                  return this.firstName + '-' this.lastName
              },
              set(value){
                  const arr = value.split('-')
                  this.firstName = arr[0]
                  this.lastName = arr[1]
              }
          }
      }
  })
  </script>
  ```

  ```html
  <!--简写-->
  <script>
  new Vue({
      el:'#root',
      data:{
          firstName:'张',
          lastName:'三'
      },
      computed:{
          fullName(){
                  return this.firstName + '-' this.lastName
              }
          }
      }
  })
  </script>
  ```

  

- **监视属性**

  如果监视属性除了handler没有其它配置项的话，可以简写

  ```html
  <!--完整写法-->
  <script>
  new Vue({
      el:'#root',
      data:{
          isHot: true
      },
      watch:{
          'isHot': {
          	//配置项
          	immediate:true,
          	deep:true,
          	handler(newValue,oldValue){
              	console.log('isHot被修改了',newValue,oldValue)
          	}
      	}
      }
      
  })
  </script>
  ```

  ```html
  <!--简写-->
  <script>
  new Vue({
      el:'#root',
      data:{
          isHot: true
      },
      watch:{
          //无法添加配置项
          isHot()(newValue,oldValue){
              console.log('isHot被修改了',newValue,oldValue)
          }
      }
  })
  </script>
  ```

  
