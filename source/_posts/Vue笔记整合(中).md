---
title: Vue笔记整合(中)
date: 2020-1-19 13:18:05
tags: "Vue"
categories: "前端"
cover: https://7.dusays.com/2020/10/23/9923c29b1589e.png
auto_open: true
---

# vue学习(4)--响应式数组、JavaScript高阶函数、v-model双向绑定

## 一、响应式数组

```javascript
btnClick() {
  //1、push()在数组结尾加上数据
  this.name.push('aa', 'bb','cc');

  //2、pop()删除数组的最后一个元素
  this.name.pop();

  //3、shift()删除数组中的第一个元素
  this.name.shift();

  //4、unshift()在数组最前面添加元素
  this.name.unshift();

  //5、splice(从哪个位置开始，要删除几个，‘要添加替换的元素’)
  //若为输入第二个参数数则删除后面所有元素
  //若要插入元素时，就输入第二个参数为0，后面跟上要加上的数字
  this.name.splice(1, 2, 'a', 'b'); 

  //6、reverse()将数组内容反转
  this.name.reverse()
}
					
```

## 二、JavaScript高阶函数

### 关于for循环

1、普通for循环

```javascript
let total = 0,
    for(let i = 0; i < this.books.length; i++){
      total += this.books[i].price * this.books[i].count
    }
return total
```

2、for(let i in this.books)

```javascript
let total = 0,
    for(let i in this.books){
      total += this.books[i].price * this.books[i].count
    }
return total
```

3、for(let item of this.books)

```javascript
let total = 0,
    for(let item of this.books){
      total += item.price * item.count
    }
return total
```

> 编程范式：命令式范式/声明式范式

### filter

- 对数组进行筛选

- filter中回调函数有一个要求：必须返回boolean
- 返回true时，函数内部会自动将这次回调的n加入数组
- 返回false时，函数内部会过滤掉这次n

```javascript
let nums = [10,11,13,44,135,324,555]
let newNums = nums.filter(function(n){
  return false
})
```

### map

- 对数组统一操作

```javascript
let new2Nums = newNums.map(function(n){
  return n * 2
})
```

### reduce

- 对数组中所有内容进行汇总

```javascript
let new2Nums = [20, 10, 50, 20]
let total = new2Nums.reduce(function(preValue, n) {
  return preValue + n
}, 0) 

//进行四次遍历
//第一次：preValue: 0， n：20
//第二次：preValue: 20， n：10
//第三次：preValue: 30， n：50
//第四次：preValue: 80， n：20
```

再简单一些完成上述操作

```javascript
let total = nums.filter(function(n){
  return n < 100
}).map(function(n){
  return n * 2
}).reduce(function(preValuce, n){
  return preValuce + n
}, 0)
```

或者使用指向函数

```javascript
let total = nums.filter(n => n < 100).map(n => n * 2).reduce((pre, n), pre + n)
```

## 三、v-model双向绑定

### 原理：实现表单元素和数据的双向绑定

```html
<input type="text" v-model="message"/>

//message: 'hhh',
```

v-model相当于：

- v-bind绑定一个value元素
- v-on指令给当前元素绑定input事件

### radio

```html
			<label for="male">
				<input type="radio" id="male" value="男" v-model="sex"/>男
			</label>
			
			<label for="female">
				<input type="radio" id="female" value="女" v-model="sex"/>女
			</label>
			
			<h2>你选择的性别：{{sex}}</h2>
```

### chexbox

单选：大多用在那种同意协议注册那种

```html
			<label for="">
				<input type="checkbox" id="agree" v-model="isAgree"/>同意
			</label>
			
			<h2>你选择的是：{{isAgree}}</h2>
			<button type="button" :disabled="!isAgree">next</button>
```

多选：

```html
			<input type="checkbox" value="1" v-model="count"/>1
			<input type="checkbox" value="2" v-model="count"/>2
			<input type="checkbox" value="3" v-model="count"/>3
			<h2>{{count}}</h2>

			count: [],
```

正常使用时

```html
			<label v-for="item in books">
				<input type="checkbox" :value="item" v-model="count"/>{{item}}
			</label>

			//count: [],
			//books: ['x','y','z'],
```

### v-model修饰符

- lazy修饰符

  - 默认情况下，v-model默认时在input事件中同步输入数据。一旦发生数据改变data中的数据就会自动改变
  - lazy修饰符可以让数据在失去焦点或者回车的时候才会更新

- number修饰符

  - 默认情况下，在输入框中无论我们输入的是字母还是数字，都会被当作字符串类型进行处理
  - 有时希望可以处理的是数字的时候就可以通过number修饰符将输入框中输入的内容自动转成数字类型

- trim修饰符

  - 如果输入的内容首位有很多空格，可以通过trim修饰符过滤内容左右两边的空格


# vue学习(5)--组件化的基本使用、父子组件间的访问和传递数据

## 一、组件化的基本使用

1. 创建组件构造器对象

   ```javascript
   const cpnC = Vue.extend({
   					template: `
   					<div>
   						<h2>
   							标题
   						</h2>
   					</div>`
   				})
   ```

    >``(模版字符串)是ES6中的新的定义字符串的语法，最大的特点就是可以换行
    >
    >```javascript
    >//使用''的话，换行要用+
    >				const str =	'abc' +
    >				'bcd'
    >//使用``的话
    >				const str =	`abc
    >				bcd`		
    >```

2. 注册组件

   ```javascript
   Vue.component('my-cpn', cpnC)
   ```

3. 使用组件

   ```html
   <div id='app'>
   	<my-cpn></my-cpn>
   </div>
   ```

整体代码

```html
    <div id='app'>
      //3、使用组件
      <my-cpn></my-cpn>
    </div>

		<script src="vue.js"></script>
			<script>
				//1、创建组件构造器对象
				const cpnC = Vue.extend({
					template: `
					<div>
						<h2>
							标题
						</h2>
					</div>`
				})
				
				//2、注册组件
				Vue.component('my-cpn', cpnC)
				
		    const app = new Vue({
		    el: '#app',
		    data: {
					
		    }
      })
			</script>    
```

### Vue.extend()

- 调用Vue.extend()创建的是一个组件构造器
- 通常在创建组件构造器时，传入template代表我们自定义组件的模版。
- 该模版就是使用到组件的地方，要显示的HTML代码
- 不过现在从vue2.x之后几乎不像这样写了

### Vue.component()

- 调用Vue.component()是将刚才的组件构造器注册成一个组件，并且给它起一个组件的标签名称。
- 所以要传入两个参数：1、注册组件的标签名 2、组件构造器

> 组件还需要挂载在某个vue实例下才会生效

## 二、全局组件和局部组件

直接在script中写这个的就是全局组件

```javascript
Vue.component('my-cpn', cpnC)
```

如果在vue实例下注册就是局部组件

```javascript
compotents: {
	//cpn使用组件时的标签名
	cpn: cpnC 
}
```

### 父组件和子组件

```JavaScript
				//子组件
				const cpnC1 = Vue.extend({
					template: `
					<div>
						<h2>
							标题
						</h2>
					</div>`
				})
				
				//父组件
				const cpnC2 = Vue.extend({
					template: `
					<div>
						<h2>
							标题
						</h2>
						<cpnc1></cpnc1>
					</div>`,
					components: {
						cpnc1: cpnC1
					}
				})
        
        //root组件
		    const app = new Vue({
		    el: '#app',
		    data: {
					
		    }
				
				components:{
					cpnc2: cpnC2
				},
				})
```

### 组件的语法糖使用

省去了调用Vue.extend()的步骤，而是直接可以使用一个对象来代替。

- 全局组件创建注册语法糖：

```javascript
Vue.component('my-cpn', {
  template: `
					<div>
						<h2>
							标题
						</h2>
						<cpnc1></cpnc1>
					</div>`
})
```

- 局部组件创建注册语法糖：

```javascript
const app = new Vue({
		    el: '#app',
		    data: {
					
		    }
				
				components:{
					'cpn2': {
						template: `
					<div>
						<h2>
							标题
						</h2>
						<cpnc1></cpnc1>
					</div>`
					}
				},
				})
```

若在script中写太多html代码不好看，可以将其分离：

```html
<script type="text/template" id="cpn">
			<div>
				<h2>
					title
				</h2>
			</div>
</script>

//或者使用template标签
<template id="cpn">
			<div>
				<h2>
					title
				</h2>
			</div>
</template>
		
<script src="vue.js"></script>
		<script>
			Vue.component('my-cpn', {
			  template: '#cpn'
			})
			
			const app = new Vue({
				el: '#app',
				data: {
			
				}
</script>		
		
```

> 组件不可以访问vue实例数据。vue组件应该有自己保存数据的地方。 
>
> 组件数据存于data()中，data必须是一个函数，其返回一个对象，对象内部保存数据。
>
> ```javascript
> Vue.component('my-cpn', {
> template: `#cpn`, 
> data() {
>  return {
>    title: 'abc'
>  }
> }
> })
> ```
>
> 

## 三、父子组件之间传递数据

### 父传子：props

```html
		<!-- Vue实例模版 -->
		<div id="app">
			<!-- 需要使用v-bind语法才可以让Vue识别movies为变量 -->
			<cpn :cmovies="movies" :cmessage= "message"></cpn>
		</div>
		
		<!-- 子组件模版 -->
		<template id="cpn">
			<div>
				<ul v-for="item in cmovies">{{item}}</ul>
				<h2>{{cmessage}}</h2>
			</div>
		</template>
			
		<script src="vue.js"></script>
		<script>
			// 子组件
			const cpn = {
				template: '#cpn',
				props: ['cmovies', 'cmessage'],
				data(){
					return{}
				},
				methods: {
					
				}
			}
			// 父组件
			const app = new Vue({
				el: '#app',
				data: {
					movies: ['1', '2', '3'],
					message: 'www'
				},
				components: {
					cpn
				}
				})
		</script>
```

### props数据验证

```javascript
props: {
					// 1、类型限制
					comive: Array,
					cmessage: String,
					
					// 2、提供一些默认值，或者一些必传值
					cmessage: {
						type: String,
						default: 'flase',
						required: true
					},
					// 类型是数组或者对象（Array或者Object）的时候，默认值必须是一个函数
					cmovies: {
						type: Array,
						default() {
							return []
						}
					}
				}
```

### 子传父（自定义事件）

```html
<!-- 父组件模版 -->
		<div id="app">
			<cpn @item-click="cpnClick"></cpn>
		</div>
		
		<!-- 子组件模版 -->
		<template id="cpn">
			<div>
				<button v-for="item in catagories"
								@click="btnClick(item)"
				>{{item.name}}</button>
			</div>
		</template>
			
		<script src="vue.js"></script>
		<script>
			// 子组件
			const cpn = {
				template: '#cpn',
				
				data() {
					return {
						catagories: [
							{id: 'aaa', name: 'one'},
							{id: 'bbb', name: 'two'},
							{id: 'ccc', name: 'three'},
						]
					}
				},
				methods: {
					btnClick(item) {
						// 发射事件： 自定义事件
						this.$emit('item-click', item)
					}
				}
			}
			// 父组件
			const app = new Vue({
					el: '#app',
					data: {
					},
					components: {
						cpn
					},
					methods:{
						cpnClick(item) {
							console.log('cpnClick', item);
						}
					}
				})
		</script>
```

### 自定义事件的流程

- 在子组件中，通过$emit()来触发事件。
- 在父组件中，通过v-on来监听子组件事件

## 四、父子组件的访问

- 父组件访问子组件：`$children`或`$refs`
- 子组件访问父组件 ：`$parent`

### 父访问子

- `$children`：一般用于取出**所有**子组件的message状态。

  ```html
  <div id="app">
    <cpn></cpn>
    <cpn></cpn>
    <cpn></cpn>
    <button @click="btnClick">1212</button>
  </div>
  
  <template id="cpn">
    <div>我是子组件</div>
  </template>
  
  <script src="vue.js"></script>
  <script>
    const app = new Vue({
      el: '#app',
      data: {
        message: '你好！'
      },
      methods: {
        btnClick() {
          // 1、$children
          console.log(this.$children);
          console.log(this.$children[2].name);
        }
      },
      components: {
        cpn: {
          template: '#cpn',
          data() {
            return {
              name: '子组件的name'
            }
          }
        },
      }
    })
  </script>
  ```

- `$refs`  =>对象类型， 默认是一个空对象 

  ```html
  <cpn ref="aaa"></cpn>
  
   btnClick() {
          //2、$refs
        console.log(this.$refs.aaa);
        }
  ```



# Vue学习(6)-插槽、模块化导入导出、webpack基础配置使用

## 一、插槽的基本使用

- 插槽的基本使用:`<slot></slot>`
- 插槽的默认值：例`<slot><button>101</button></slot>`
- 如果有多个值，同时放入到组件中进行替换时，一起作为替换元素。

```html
<div id="app">
  <cpn></cpn>
  <cpn>
    <div>是替换的值</div>
    <button>200</button>
  </cpn>
</div>

<template id="cpn">
  <div>
    <div>
      我是子组件默认值
    </div>
    <slot>
      <button>1111</button>
    </slot>
  </div>
</template>
```

### 具名插槽

 可以有多个插槽，并且对专门的其中一个插槽进行修改时就需要用到具名插槽：

```html
<div id="app">
  <cpn>
    <div slot="center">中中</div>
  </cpn>
</div>

<template id="cpn">
  <div>
    <div>
      我是子组件
    </div>
    <slot name="left">
      <button>前</button>
    </slot>
    <slot name="center">
      <button>中</button>
    </slot>
    <slot name="right">
      <button>后</button>
    </slot>
  </div>
</template>
```

> 父组件模版的所有东西都会在父级作用域内编译；子组件模版的所有东西都会在子级作用域内编译。
>
> 插槽方面在新版本里使用v-slot，不过好像不经常用以后再补。。。

## 二、模块化的导入和导出

### ES

- 使用`export`指令导出模块对外提供的接口，再通过`import`命令来加载对应的模块

- 先在HTML中引入两个js文件，并且类型设置module

  ```html
  <script src="main.js" type="module"></script>
  ```

想要导出时：

```javascript
let flag = ture;

function sum(num1, num2){
  return num1 + num2
}

export {
	flag, sum
}

//或另一种
export let flag = ture;

export function sum(num1, num2){
  return num1 + num2
}
```

导入时：

```javascript
import {flag} from "导入信息原文件地址"

if(flag){
  console.log('flag为ture')
}
```

> 某些情况下，一个模块中包含某个的功能，我们并不希望给这个功能命名，而是让导入者可以自己来命名，那么就可以使用`export default`:

```javascript
export default function () {
		
}
```

```javascript
import xxx from "./"
//不需要大括号
```

> `export default`在同一个模块中不允许存在多个。

### Commonjs

- 导出

  ```javascript
  module.exports = {
  	flag: ture,
  	test(a, b) {
  		return a + b
  	},
  	demo(a, b) {
  		return a * b
  	}
  }
  ```

- 导入

  ```javascript
  //CommonJs模块
  let {test, demo, flag} = require('moduleA');
  
  //等同于
  let _mA = require('moduleA');
  let test = _mA.test;
  let demo = _mA.demo;
  let flag = _mA.flag;
  ```

  

## 三、webpack

- 从本质上来说，webpack是一个现代的JavaScript应用的静态模块打包工具

- webpack依赖node环境，node环境通过npm（node packages manager）工具来管理各种包

  > 局部安装webpack
  >
  > --save-dev是开发时依赖，项目打包后不需要继续使用的
  >
  > ```
  > cd 对应目录
  > npm install webpack@3.6.0 --save-dev
  > ```

- 使用时在终端中输入：

  ```
  webpack ./src/main.js ./dist/bundle.js
  ```

  通过这样对模块化开发的js文件进行打包使得网页再使用js时直接调用打出来的包就可以，不用一个一个的导入。

- 文件和文件夹的解析：

  - dist文件夹：用于存放之后打包的文件
  - src文件夹：用于存放源码
    - main.js：项目的入口文件。
    - mathUtils.js：定义了一些数学工具函数，可以在其他地方引用使用。
  - index.html：浏览器打开展示的首页html
  - package.json：通过npm init生成的，npm包管理的文件。

### webpack.config.js文件

为了每次使用webpack时不用写上入口和出口参数，就通过这个文件

```js
const path = require('path')//引用path

module.exports = {
  //入口：可以是字符串/数组/对象
  entry:'./src/main.js',
  //出口：通常是一个对象，里面至少包含两个重要的属性，path和filename
  output: {
    path: path.resolve(__dirname, 'dist'),//path通常绝对路径
    filename: 'bundle.js'
  }
}
```

这时直接在终端里输入`webpack`即可打包。

### package.json文件

如何生成该文件：在终端输入`npm init`

若是局部安装webpack的话就要到webpack路径下调用才行，不过可以通过package.json中定义启动。

```json
{
	"name": "meetwebpack",
	...
	"scripts": {
		"build": "webpack"//在这里面定义脚本
	},
	...
}
```

这时只用在终端中输入`npm run build`，即可调用局部webpack。

> 若package.json文件中有需要依赖的东西就在终端中输入`npm install`来下载

### loaders

可以用来解析各种文件给webpack方便打包，因为webpack无法识别全部文件。

- 例如需要一起打包css文件

  - 安装`npm install css-loader --save-dev`，`npm install style-loader --save-dev`

  - 在webpack.config.js中

    ```javascript
    module.exports = {
      entry: '',
      output: {
        path: ,
        filename: '',
      },
      module: {
        rules: [
          {
            test: /\.css$/,
            //css-loader只负责将css文件进行加载
            //style-loader负责将钥匙添加到DOM中
            //使用多个loader时，是从右向左的
            use:['style-loader','css-loader']
          }
        ]
      }
      
    }
    ```

更多见：https://www.webpackjs.com/

### 使用vue的配置过程

在终端中输入`npm install vue --save`安装上vue

使用

```js
import Vue from 'vue'

new Vue({
  el: '#app',
  data: {
    name: 'code'
  }
})
```

在打包时因为Vue不同版本的构建会报错，runtime-only和runtime-compiler的区别，因此要在webpack中的module.exports添加配置：

```js
resolve: {
  alias: { //别名
    'vue$': 'vue/dist/vue.esm.js'
  }
}
```



# Vue学习(7)-plugin、Vue-Cli、箭头函数、vue-router准备

## 一、vue的使用方法

创建vue时，当template和el同时存在时，会将template中的东西替换el中的东西。

要对.vue文件进行封装处理就需要安装vue-loader和vue-template-compiler.

```
npm install vue-loader vue-template-compiler --save-dev
```

因为脚手架还没学到，现在这个形式好像是很接近脚手架的了。

- App.vue

  ```vue
  //模版样式
  <template>  
    <div>
      <h2 class="title">{{message}}</h2>
      <button @click="btnClick">www</button>
    </div>
  </template>
  
  //js代码
  <script>
    export default {
      name: "App",
      data() {
        return {
          message: 'hello world',
        }
      },
      methods: {
        btnClick() {
  
        }
      }
    }
  </script>
  
  //css样式
  <style scoped>
    .title {
      color: #4fc08d;
    }
  </style>
  ```

- main.js

  ```js
  import Vue from "vue";
  // import App from "./vue/app";
  import App from "./vue/App";
  // require("./css/back.css")  commentJs的方式
  import "./css/back.css";
  
  new Vue({
    el: "#app",
    template: `<App/>`, //之后替换app中的东西
    components: {
      App
    }
  
  })
  ```

- Index.html

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
  </head>
  <body>
  
  <div id="app">
  </div>
  
  <script src="./dist/bundle.js"></script>
  </body>
  </html>
  ```

## 二、plugin

### 认识plugin

- plugin是插件的意思，通常用于对某个现有框架进行扩展。webpack中的插件就是对webpack现有功能的各种扩展，比如打包优化，文件压缩等。
- loader和plugin的区别：
  - loader主要用于转换某些类型的模块，是一个转换器。
  - plugin是插件，对webpack本身进行扩展，是个扩展器。
- 使用过程：
  - 通过npm安装需要使用的plugins（内置过的不用再安）
  - 在webpack.config.js中的plugins中配置插件。

### 添加版权的plugin

在webpack.config.js中添加

```js
const path = require('path');
const webpack = require('webpack');

module.exports = {
  ...
  plugins: [
    new webpack.BannerPlugin('最终版权归大家所有')
  ]
}
```

即可在bundle.js开头添加声明。

> 不过应该不常用吧

### 打包html的plugin

因为发布时是发布dist里的文件，但是dist里没有index.html，因此需要将html也打包进去。

- 安装HtmlWebpackPlugin插件：`npm install html-webpack-plugin --save-dev`

- 要使用时在webpack.config.js文件中进行修改

  ```js
  const HtmlWebpackPlugin = require('html-webpack-plugin');
  
  ...
  plugins: [
      new HtmlWebpackPlugin({
        template: 'index.html'	//打包的模版
      })
    ]
  ```

- 这时就会在dist文件中生成一个index.html

> 我在使用的时候出了点问题，安装htmlwebpackplugin安不上。可以这样解决：
>
> 1. 清缓存：`npm cache clean --force`
> 2. 重新安装：`npm install html-webpack-plugin --save-dev`

### 搭建本地服务器

便于加快开发进度，在浏览器中刷新自动更新，就是热加载功能。

- 安装

  ```js
  npm install --save-dev webpack-dev-server@2.9.1
  //因为和之前安的vue2.x版本相对应
  ```

- devserver作为webpack的一个选项，有以下属性可以设置：

  - contentBase：为哪一个文件夹提供本地服务，默认根目录，在这要填./dist
  - port：端口号(默认8080)
  - inline：页面实时刷新
  - historyApiFallback：在SPA页面中，依赖HTML5的history模式。

- 将webpack.config.js中添加：

  ```js
    devServer: {
      contentBase: './dist',
      inline: true
    }
  ```

- 同时为了在方便使用，在package.json中添加

  ```json
  "scripts": {
  	...
    "dev": "webpack-dev-server --open"
  },
  ```

## 三、Vue-CLI

### runtime-compiler和runtime-only的区别

- runtime-compiler：template -> ast(抽象语法树) -> render -> vitual dom -> UI

  ```js
  new Vue({
  	el: '#app',
  	template: '<App/>',
  	components: {App}
  })
  ```

- runtime-only：render -> vitual Dom -> UI

  性能高，代码量少

  ```js
  new Vue({
  	el: '#app',
  	render: h => h(App),
    //render: function (h) {
    // 		return h(App) 	
  	//}
  })
  ```

  

### Vue-cli3的创建

- 输入`vue create projectName`

- ```
  .browserslistrc =>浏览器相关支持情况
  .babel.config.js =>ES语法转换
  ```

也可以使用`vue ui`来网页创建和管理



## 四、箭头函数

```js
 		//1、function
    const aaa = function () {

    }

    //2、对象字面量中定义函数
    const obj = {
        bbb() {

        }
    }

    //3、ES6中的箭头函数
    const ccc = (参数列表) => {

    }
    //若一个参数时小括号可以省略

    //返回值若只有一行时可以直接写
    const ddd = (num1, num2) => num1 * num2

    //箭头函数的this是一层一层往外找的
```



## 五、vue-router准备

### 页面url更新但页面不重新加载

```
//将url压入栈结构

location.hash='url地址'
history.pushState({},'','url地址')

history.back()
history.forward()
//等同
history.go(前进或后退的地址数：例如-1,1)
```

### 安装与配置

router/index.js

```javascript
//相关配置
import Vue from 'vue'
import VueRouter from 'vue-router'
import Home from '../views/Home.vue'

//1、安装插件
Vue.use(VueRouter)

//2、创建VueRouter对象
  const routes = [

]

const router = new VueRouter({
  mode: 'history',
  base: process.env.BASE_URL,
  //配置路由和组件间的应用关系
  routes
})

//3、将router对象传入vue实例中
export default router
```

```javascript
//4、在main.js中进行挂载
new Vue({
  router,
  render: h => h(App)
}).$mount('#app')
```

### 映射配置和展现

```js
//index.js
import About from '../components/About.vue'

const routes = [
  {
    //点开默认显示的组件
    path: '/',
    name: 'Home',
    component: Home
  },
  {
    path: '/',
    name: 'Home',
    //点开默认显示的组件，同时地址栏显示
    redirect: Home
  },
  {
    path: '/about',
    name: 'About',
    component: About
  }
]
```

```vue
<template>
  <div id="app">
    <div id="nav">
      <router-link to="/">Home</router-link> |
      //默认是a标签，加上tag可以使其变为其他的属性
      <router-link to="/" tag="button">Home</router-link> |
      //若想使用的history.replaceState(),即是不能用浏览器的前进后退跳转
      <router-link to="/" replace>Home</router-link> |
      <router-link to="/about">About</router-link>
    </div>
    //占位
    <router-view/>
  </div>
</template>

<style>
.router-link-active{
  //可对点击过的router-link处于活跃状态高亮显示的样式进行增添修改
}
</style>  
```

或者想自己定义函数来跳转

```vue
<template>
  <div id="app">
    <div id="nav">
      <div @click="homev">home</div>  |
      <div @click="aboutv">about</div>
    </div>
    <router-view/>
  </div>
</template>

<script>
  export default {
    name: "App",
    methods: {
      homev() {
        this.$router.push("/")
      },
      aboutv() {
        this.$router.push("about")
        //或this.$router.replace("about")
      }
    }
  }
</script>
```

### 动态路由

```js
//index.js  
{
  	//当之后那个组件要获取时就是获取：后面的值
    path: '/user/:aaa',
    name: 'user',
    component: user
  }
```

```vue
 <router-link :to="'/user/'+userId">user</router-link>

<script>
  ...
    data () {
      return{
        userId: 'we'
      }
    },
  ...   
</script>
```

- 若要获取例如用户名或者商品id，对组件进行一定修改

```vue
<div>
		<h1>{{aaa}}</h1>
  //或是这样直接获取
  	<h1>{{$route.params.aaa}}</h1>
</div>

<script>
  export default {
    name: "user",
    computed: {
      aaa() {
        return this.$route.params.aaa
      }
    }
  }
</script>
```

#### 路由懒加载

```js
const  Home = () => import('../components/Home')


const routes = [
  {
    path: '/',
    name: 'Home',
    component: Home
    // 或是直接component: () => import('../components/Home')
  },

```

#### 路由的嵌套使用

- 创建子组件

- 在父组件中定义位置

  ```html
  <div>
      <h1>Home</h1>
      <router-link to="/news">news</router-link>
      <router-view></router-view>
  </div>  
  ```

- 在路由中定义

  ```js
  const  HomeNews = () => import('../components/HomeNews')
  
  const routes = [
    {
      path: '/',
      name: 'Home',
      component: Home,
      children: [
        {
          //要让其默认显示news子组件时的默认路径
          path: '',
          redirect: 'news'
        },
        {
          path: 'news',
          component: HomeNews,
        }
      ]
    },
  ]  
  ```

  



