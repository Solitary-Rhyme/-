# VueCLI

[toc]

## 1. 初识脚手架

### 1.1 基础指令

- `vue create xxx` 在当前目录创建项目

- `npm run serve` 启动项目

- `npm run build` 打包项目

- `Crtl+C` 暂停项目

### 1.2 脚手架文件结构

```
node_modules
public
	|— favicon.ico:页签图标
	|— index.html:主页面
src
	|—— assets:存放静态资源
		|_ logo.png
	|—— component:存放组件
		|_ HelloWorld.vue
	|——  App.vue:汇总说有组件
	|—— main.js:入口文件
.gitignore:git:版本管制忽略的配置
babel.config.js:babel的配置文件
package.json:应用包配置文件
README.md:应用描述文件
package-lock.json:包版本控制文件
```



## 2. 脚手架基础

### 2.1 render函数

- **定义**

  render函数用于将App组件放入容器中，相当于内置的模板解析器

  其他的组件不需要使用render，`.vue`文件会自动解析，只需要在`main.js`中放置App组件时使用

```js
import Vue from 'vue'
import App from './App.vue'

new Vue({
    el:'#app'
    //简写形式
    render: h => h(App)
	/*完整形式
	render(createrElement){
        return createElement(App)
    }*/
})
```

### 2.2 ref属性

- **定义**

  ref用来给元素或子组件注册引用信息（可以理解为id的替代品）。ref应用在html标签上获取的是真实DOM元素，应用在组件标签上获取的是组件实例对象vc

- **语法**

  - 设置：`<h1 ref="xxx"></h1>` 或`<school ref="xxx"></school>`
  - 获取：`this.$refs.xxx`

```vue
<template>
	<div>
        <h1 v-text="msg" ref="title"></h1>
        <button ref="btn" @click="showDOM">点击输出上方的DOM元素</button>
       	<school ref="sch"/>
    </div>
</template>

<script>
	import schoold from './components/school'
    
    export default{
        name:'App',
        components:{School},
        data(){
            return {
                msg:'hello'
            }
        },
        methods:{
            showDOM(){
                console.log(this.$refs.title)	//真实dom元素
                console.log(this.$refs.btn)		//真实dom元素
                console.log(this.$refs.sch)		//school组件的实例对象(vc)
            }
        }
    }
</script>
```



### 2.3 props配置项

- **定义**

  props可以让组件接收从外部传过来的数据

  props是只读的，Vue底层会监测对props的修改，如果进行了修改，就会警告

  如果需求要求修改，可以复制props的内容到data中，然后修改data中的数据

- **语法**

  传递数据：`<Demo name="xxx" :age="18">`

  - 此处age前必须绑定，否则默认传过去的将是字符串，而不是具体数值/表达式

  接收数据：

  - 普通方式 `props:['name','age]`

  - 限制类型 `props:{name:String, age:Number}`

  - 限制类型，限制必要性，指定默认值

    ```js
    props:{
        name:{
            type:String,
            required: true,
            default: 'Jack'
        }
    }
    ```




### 2.4 基础编码思想

- 组件化编码流程

  - 拆分静态组件：组件要按照功能点拆分

  - 实现动态组件：考虑好数据的存放位置，数据是一个组件在用，还是一些组件在用
    - 一个组件在用：放在组件自身即可
    - 一些组件在用：放在他们共同的父组件上

  - 实现交互：从绑定事件开始

- props适用于：

  - 父组件→子组件通信
  - 子组件→父组件通信，要求父组件先给子组件一个函数

- 使用`v-model`时要切记：v-model绑定的值不能是props传过来的值，因为props是不可以修改的

- props传过来的若是对象类型的值，修改对象中的属性时Vue不会报错，但不推荐这样做



## 4. 本地存储webStorage

- 游览器端通过Window.sessionStorage和Window.localStorage属性来实现本地存储机制

  存储内容大小一般支持5MB左右

- **相关API**

  `xxxStorage.setItem('key','value');`：该方法接受一个键和值作为参数，会把键值对添加到存储中，如果键名存在，则更新其对应的值

  `xxxStorage.getItem('person');`：该方法接受一个键名作为参数，返回键名对应的值

  `xxxStorage.removeItem('key');`:该方法接受一个键名作为参数,并把该键名从存储中删除

  `xxxStorage.clear()`：该方法会清空存储中的所有数据

- 注意事项

  - SessionStorage存储的内容会随着游览器窗口关闭而消失
  - LocalStorage存储的内容，需要手动清除才会消失
  - `xxxStorage.getItem('xxx')` 如果获取不到对应的value，那么返回值是null
  - `JSON.parse(null)` 的结果依然是null



## 5. 自定义事件

- 自定义事件是一种组件间通信的方式，适用于子组件传递数据给父组件

  如果子组件想给父组件传数据，那么就要在父组件中给子组件绑定自定义组件（事件的回调在A中）

- **绑定自定义事件**

  - 方法一：在父组件中`<Demo @事件名="方法名"/>`
  - 方法二：在父组件中`this.$refs.demo.$on('事件名',回调函数)`

  - 若想要让自定义事件只能触发一次，可以使用once修饰符，或`$once`方法
  - 组件上也可以绑定原生DOM事件，需要使用native修饰符，否则会被强制识别为自定义事件
  - 通过`this.$refs.xxx.$on('事件名',回调函数)`绑定自定义事件时，回调函数要么配置在methods中，要么使用箭头函数，否则this指向会出问题

  **触发自定义事件**： `this.$emit('事件名',数据)`

  **解绑自定义事件**： `this.$off('事件名')`



## 6. 全局事件总线

- 全局事件总线是一种可以在任意组件间通信的方式，本质上是一个对象

  它满足以下要求：

  1. 所有的组件对象都必须能看见它
  2. 这个对象必须能够绑定，触发和解绑事件