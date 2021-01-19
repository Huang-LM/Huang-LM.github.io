---
title: Vue笔记整合(上)
date: 2019-11-26 23:23:05
tags: "Vue"
categories: "前端"
cover: https://7.dusays.com/2020/10/23/109f97c621f79.jpg
auto_open: true
---

# vue学习(1)--引入、小实例、mvvm、生命周期

Vue 是一套用于构建用户界面的渐进式框架。编程范式：声明式编程。

## 一、引入

```vue
<script src="vue.js的地址"></script>
```

## 二、小实例

### 建一个简单Vue应用：

```vue
<div id="app">{{ message }}</div>

<script src="vue.js的地址"></script>
<script>
    const app = new Vue({
    el: '#app',
    data: {
      message: 'Hello Vue!'
    }
  })
</script>
```



- **在es6里面定义`const`(常量)/`let`(变量)。**
- 在创建Vue对象时，传入一些option:{}
  - `el`属性：
    - 类型：string｜HTMLElement
    - 该属性决定了这个Vue对象挂载到哪一个元素上面。
  - `data`属性：
    - 类型：Object｜Function(组件中data必须是一个函数)
    - Vue实例对应的数据对象。通常存储一些数据：可以是自己定义的，也可以是来自网络服务器加载的。

### 	写个列表：

```vue
<div id="app">
	<ul>
    <li v-for="item in count">{{item}}</li>
  </ul>
</div>

<script src="../vue.js"></script>
<script>
    const app = new Vue({
    el: '#app',
    data: {
      count:['one','two','three','four'],
    }
  })
</script>
```

- v-for指令
- 指令带有前缀`v-`,以表示它们是 Vue 提供的特殊特性。

### 实现计数器：

```vue
<div id="app">
	<h2>
    当前记数：{{count}}
  </h2>
  <button v-on:click="add">
    +
  </button>
  <button @click="sub">
    -
  </button>
</div>

<script src="../vue.js"></script>
<script>
    const app = new Vue({
    el: '#app',
    data: {
      counter:0
    },
    methods:{
      add:function(){
        console.log('add被执行');
        this.counter++
      },
      sub:function(){
        console.log('sub被执行');
        this.counter--
      }
    }  
  })
</script>
```

- `Methods` 属性：
  - 类型：{[key:string]:Function}
  - 用于Vue对象中定义方法。可在其他地方调用，也可以
- `@click`指令：用于监听偶一个元素的点击事件，并且需要指定当发生点击时，执行的方法（通常是methods中定义的方法）。（语法糖，是`v-on:`的简写）
- 方法：`method`
- 函数：`function`

### 处理用户输入

```vue
<div id="app-5">
  <p>{{ message }}</p>
  <button v-on:click="reverseMessage">反转消息</button>
</div>

<script>
  var app5 = new Vue({
    el: '#app-5',
    data: {
      message: 'Hello Vue.js!'
    },
    methods: {
      reverseMessage: function () {
        this.message = this.message.split('').reverse().join('')
      }
    }
  })
</script>
```

- 用 `v-on` 指令添加一个事件监听器，通过它调用在 Vue 实例中定义的方法

### 双向绑定

```vue
<div id="app-6">
  <p>{{ message }}</p>
  <input v-model="message">
</div>

<script>
	var app6 = new Vue({
  el: '#app-6',
  data: {
    message: 'Hello Vue!'
  }
})
</script>
```

- `v-model` 指令，它能轻松实现表单输入和应用状态之间的双向绑定。

## 三、vue中的mvvm

虽然没有完全遵循 [MVVM 模型]，但是 Vue 的设计也受到了它的启发。因此在文档中经常会使用 `vm` (ViewModel 的缩写) 这个变量名表示 Vue 实例。

https://zh.wikipedia.org/wiki/MVVM

- View层
  - 视图层
  - 在前端开发中通常是DOM层
  - 主要用于给用户展示各种东西
- Model层
  - 数据层
  - 数据可能是我们定的，也可以是请求来自从服务器里的
- VueModel层
  - 视图模型层
  - 视图模型层是View和Model沟通的桥梁
  - 一方面实现了Data Binding（数据绑定），讲Model的改变反应到View里，另一方面实现Data Listener（Dom监听），当Dom发生一些事件（点击、滚动、touch）时，可以监听并在需要的时候改变对应的Data。

## 四、Vue的生命周期

Vue源码中的一部分

```javascript
// src/instance/init.js

//expose real self
vm._self = vm;
initLifecycle(vm);
initEvents(vm);
initRender(vm);
callHook(vm,'beforeCreate');//回调函数
initLnjections(vm);
initState(vm);
initProvide(vm);
callHook(vm,'created');//回调函数
```

并不是很懂。。。

![lifecycle](https://cn.vuejs.org/images/lifecycle.png)



# Vue学习(2)--插值基本语法、动态绑定、计算属性

## 一、插值基本语法

### Mustache（胡须，指双大括号）

### v-once

- 不让界面随意的因为后面的改变而改变

- 该指令表示元素和组件只会渲染一次，不会随着数据的改变而改变。后面不跟表达式

- ```html
  <h2 v-once>
    {{message}}
  </h2>
  ```

### v-html

- 某些情况下从服务器返回的数据本身是一个html代码，若直接用{{}}进行输出，会将html代码一起输出。如果希望按照html格式进行解析并输出对应内容，就该用到此方法。


```html
<div id="app">
			<h2 v-html="url"></h2>
		</div>
		
		<script src="vue.js"></script>
			<script>
		    const app = new Vue({
		    el: '#app',
		    data: {
			  url:'<a href="http://www.cctv.com">CCTV</a>'
		    }
		  })
		</script>
```

- 这样就解析出了：[CCTV](http://www.cctv.com) 。

### v-text

- 直接显示message里的内容，但是h2里若再多写文本则会被message覆盖。

```html
<h2 v-text="message"></h2>
```

### v-pre

- v-pre将会跳过这个元素和他的子元素的编译过程，用于显示原本的Mustache语法。

```html
<h2 v-pre>
  {{message}}
</h2>
```

```html
//网页上显示的
{{message}}
```

### v-cloak

- 某些情况下加载慢卡住还没到script时可以用v-cloak在解析前先显示，等解析完了会自动删除该属性。

```html
<style>
			[v-cloak]{
				display: none;
			}
</style>

<div id="app" v-cloak>
			<h2>{{message}}</h2>
		</div>
		
		<script src="vue.js"></script>
			<script>
				//在Vue解析前div是有一个v-cloak属性
				//在Vue解析后v-cloak属性被删除
		    const app = new Vue({
		    el: '#app',
		    data: {
				message:'hello'
		    }
		  })
		</script>
```

## 二、动态绑定

### 	v-bind

- 作用：动态绑定数据
- 语法糖：:

```html
<div id="app" v-cloak>
			<img v-bind:src="imgURL" alt="">
  		<a v-bind:href="aHref">超链接</a>
  		//语法糖写法
  		<img :src="imgURL" alt="">
  		<a :href="aHref">超链接</a>
		</div>
		
		<script src="vue.js"></script>
			<script>
		    const app = new Vue({
		    el: '#app',
		    data: {
					imgURL:'地址',
          aHref:'地址'
		    }
		  })
		</script>
```

### 动态绑定class

#### （对象语法）

- 对象语法的含义：class后面跟的是一个对象

- 以下用法

  ```html
  一、<h2 :class="{active: isActive}">Hello</h2>
  二、<h2 :class="{active: isActive, line: isLine}">Hello</h2>
  三、<h2 class="默认属性" v-bind:class="{active: isActive, line: isLine}">Hello</h2>
  ```

实例：

```html
<style>
			.active{
				color: red;
			}
		</style>

<div id="app">
			<!-- <h2 v-bind:class="{类名1：boolean, 类名2：boolean}"></h2> -->
			<h2 class="默认属性" v-bind:class="{active: isActive, line: isLine}">{{message}}</h2>
			<button v-on:click="btnClick()">按钮</button>
		</div>
		
		<script src="vue.js"></script>
			<script>
		    const app = new Vue({
		    el: '#app',
		    data: {
					message: 'hello',
					isActive: true,
					isLine: true,
		    },
				methods:{
					btnClick:function(){
						this.isActive = !this.isActive
					}
				}
		  })
		</script>
```

#### （数组语法）

- class后面跟着数组

- 用法如下：

  ```html
  一、<h2 :class="['active']">Hello</h2>
  二、<h2 :class="['active','line']">Hello</h2>
  三、<h2 class="默认属性" v-bind:class="['active','line']">Hello</h2>
  ```

### 动态绑定style

- 对象语法和数组语法

- 对象:style后跟一个对象类型

  ```html
  <!-- <h2 :style="{属性名1：value('属性值'), 属性名2：属性值}"></h2> -->
  <h2 :style="{fontSize: '50px'}">Hello</h2>
  <!--'50px'需加上单引号，否则就会当成变量去解析-->
  
  <h2 :style="{fontSize: finalSize}">Hello</h2>
  <h2 :style="{fontSize: finalSize2 + 'px'}">Hello</h2>
  data{
      finalSize: '100px',
  		finalSize2: 100
      }
  ```

- 数组：style后跟数组类型

  ```html
  <div :style="[baseStyle, baseStyle2]"></div>
  data: {
  	baseStlye: {backgroundColor: 'red'},
  	baseStyle2: {fontSize: '100px'},
  }
  ```

## 三、计算属性

`computed`:用于更直观更简洁的展示数据

基本使用：

```html
<div id="app">
			<h2>{{fullName}}</h2>
		</div>
		
		<script src="vue.js"></script>
			<script>
		    const app = new Vue({
		    el: '#app',
		    data: {
					fistName: 'huang',
					lastName: 'ming'
		    },
				computed:{
					fullName:function(){
						return this.fistName + ' ' + this.lastName
					}
				}
		  })
		</script>
```

或者用来计算数字总和

```html
<h2>总价格：{{totalPrice}}</h2>

<script>
		    const app = new Vue({
		    el: '#app',
		    data: {
					books:[
						{id: 110, name: 'x1', price: 100},
						{id: 111, name: 'x2', price: 123},
						{id: 112, name: 'x3', price: 14},
						{id: 113, name: 'x4', price: 66},
					]
		    },
				computed:{
					totalPrice: function(){
						let result = 0;
						for (let i = 0;i < this.books.length;i++) {
							result += this.books[i].price
						}
						return result;
					}
				}
		  })
		</script>
```



# Vue学习(3)--计算属性和method的对比、部分ES6、事件监听、条件判断、v-show、v-for

## 一、计算属性与method的对比

method只会一次次的执行方法，而计算属性可以判断每次参数是否变化而决定是再执行一次还是返回第一次的结果，计算属性会进行缓存，若多次使用时，计算属性只会调用一次。总体来说计算属性比method更加高效。 

## 二、ES6语法的一些学习

### 块级作用域

- `let`是`var`的优化版，因为`let`具有块级作用域，而`var`没有。
- 在JavaScript中`if`和`for`没有块级作用域，只有函数有块级作用域。

### `const`的使用

- 在JavaScript中，使用const修饰的标识符为常量不可再被赋值。

- ps：在ES6开发中，优先使用const，只有在需要改变某一标识符时才使用let。

- 使用注意：

  - 在使用const定义时需赋值。 

  - 常量的含义是指向的对象不能修改，但是可以改变对象内部的属性。

    ```javascript
    const obj = {
    	name = 'x',
    	age: 18,
    	sex: man
    }
    
    obj.name = 'y';
    ```

### 对象字面量的增强写法

- 属性的增强写法

  ```javascript
  const name = 'x';
  const	age: 18;
  const	sex: man;
  
  //ES5
  const obj = {
  	name: name,
  	age: age,
  	sex: sex,
  }
  
  //ES6
  const obj = {
  	name,
  	age,
  	sex,
  }
  ```

- 函数的增强写法

  ```javascript
  //ES5
  const obj = {
  	run:function(){
  	
  	}
  }
  
  //ES6
  const obj = {
  	run(){
  	
  	}
  }
  ```

> Typescript中加入了类型检测
>
> ```javascript
> function split_string(str: String){
> 	str.split
> }
> 
> //在输入时就会进行类型检测，若输错会报错
> split_string();
> ```
>
> 

## 三、事件监听

### v-on

- 基本使用和语法糖

```html
<h2>
  {{counter}}
</h2>
<button v-on:click="add">+</button>
<button @click="sub">-</button>
```

```javascript
const app = new Vue({
    el: '#app',
    data: {
      counter:0
    },
    methods:{
      add(){
        this.counter++
      },
      sub(){
        this.counter--
      }
    }  
  })
```

- 参数问题

```html
<!-- 事件调用的方法没参数 -->
<button @click="btn1Click()">11</button>
<button @click="btn1Click">11</button>

<!-- 在事件定义时，写方法省略了小括号，但本身事件是需要参数的，这是Vue就会默认将浏览器产生的event事件对象作为参数传入方法中 -->
<button @click="btn2Click()">22</button>
<button @click="btn2Click">22</button>

<!-- 在调用时手动获取event对象用$event -->
<button @click="btn3Click(123,$event)"></button>
```

```javascript
methods:{
  btn1Click(){
    console.log("btn1Click"),
  }

    btn2Click(event){
      console.log(",,",event)
    }

    btn3Click(event){
      console.log(abc,event)
    }  
  }
```

## 四、条件判断

### v-if、v-else-if、v-else

- 与JavaScript的if、else、if else相似。

- 根据布尔值或条件判断是否渲染

  ```html
  <h2 v-if="true/flase">xxx</h2>
  ```

  ```html
  <h2 v-if="true/flase">xxx</h2>
  <h2 v-else>若v-if为flase时显示</h2>
  ```

  ```html
  <h2 v-if="score>90">优秀</h2>
  <h2 v-else-if="score>80">良好</h2>
  ```

  > ```html
  > <!--复杂更多的是使用computed或者methid中放函数来判断-->
  > 
  > <h2>{{result}}</h2>
  > 
  > <script>
  > 	computed:{
  > 					result(){
  > 						let show = ' ';
  > 						if(this.score>=90){
  > 							show = '优秀'
  > 						}else if(this.score>=80){
  > 							show = '良好'
  > 						}
  > 						return show;
  > 					}
  > 				}
  > </script>
  > ```

### 登陆切换小实例

```html
<div id="app">
			<span v-if="isUser">
				<label for="username">用户账号</label>
				<input type="text" id="username" placeholder="用户账号" key="username" />
			</span>
			<span v-else>
				<label for="email">用户邮箱</label>
				<input type="text" id="email" placeholder="用户邮箱" key="email" />
			</span>
			<button @click="isUser = !isUser">切换类型</button>
		</div>

<script>
		    const app = new Vue({
		    el: '#app',
		    data: {
					isUser = true,
		    },
</script>
```

> 加入key的作用是为了防止输入框内的数据在切换以后复用的问题。
>
> 因为vue中在进行DOM渲染时，出于性能考虑，已经可能的复用已经存在过的元素，而不是重新创建新元素。

## 五、v-show使用

### v-show和v-if的区别

- v-show是在为`flase`的时候给元素加上`display: none`，在dom层还会存在。
- v-if在为`flase`的时候则会直接将元素消除不显示。
- 在切换频率高的时候使用v-show性能更佳，只有一次的话v-if就可以了。

## 六、v-for遍历

- 在遍历过程中没有索引值

  ```html
  <ul>
    <li v-for="item in name">{{item}}</li>
  </ul>
  
  name: ['x','y','z'],
  ```

- 在遍历过程中没有索引值

  ```html
  <ul>
    <li v-for="(item, index) in name">{{index+1}}. {{item}}</li>
  </ul>
  
  name: ['x','y','z'],
  ```

- 遍历对象时

  ```html
  <ul>
    <li v-for="item in info">{{item}}</li>
  </ul>
  
  <ul>
    <li v-for="(item, key) in info">{{key}}:{{item}}</li>
  </ul>
  <!-- key是获取对象属性，item是获取对象value -->
  
  info:{
    name: 'x',
    age: '18'
  }
  ```

> 官方推荐在使用v-for遍历时，给对应元素加上一个：key属性。
>
> 使用key属性是为了高效的更新虚拟DOM。
>
> ```html
> <ul>
> <li v-for="item in info" :key="item">{{item}}</li>
> </ul>
> ```

