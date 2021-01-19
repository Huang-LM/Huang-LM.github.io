---
title: Vue笔记整合(下)
date: 2020-7-21 22:11:40
tags: "Vue"
categories: "前端"
cover: https://7.dusays.com/2020/10/23/cfb9789f2aa74.jpg
auto_open: true
---

# Vue学习(8)-vue-router、Promise的学习

## 一、vue-router学习

### 路由的传值

- URL：
  - 协议://主机:端口/路径？查询
  - scheme://host:post/path?query#fragment

```html
<router-link :to="{path: '/profile'}, query: {name: 'why', age: 18, height: 1.88}">档案</router-link>

<h2>
  {{$route.query}}
</h2>
<h2>
  {{$route.query.name}}
</h2>
```

```js
<buutton @cilck="profileClick">档案</buutton>


data() {
	return{
		userId: 'zhangsan',
	}
}
method: {
	userClick() {
		this.$router.push('/user/' + this.userId)
	},
	profileClick() {
		this.$rounter.push({
			path: '/profile',
			query: {
				name: 'kobe',
				age: 19,
      }
		})
	}
}
```

### `$router`和`$route`的区别

- `$rouer`为VueRouter实例，想要导航到不同url，则使用`$router.push`
- `$route`为当前router跳转对象里可以获取name、path、query、params等。

### 全局路由守卫

在路由跳转时监听改变网页标题

```js
//index.js
	{
    path: '/about',
    name: 'About',
    // mate: {
    //   title: 'home'
    // },
    beforeEnter: (to, from, next) => {
      //路由独享的守卫
      next()
    },  
    component: About
  },
	
    
//前置守卫（guard）
router.beforeEach((to, from, next) => {
  document.title = to.name,
  // document.title = to.matched[0].meta.title,
  next()
  //调用next方法后，才能进入下一个钩子
})

//后置钩子（hook）
router.afterEach((to, from) => {
 	//不需要next()
})
```

### 组件内的守卫

```js
const Foo = {
  template: `...`,
  beforeRouteEnter (to, from, next) {
    // 在渲染该组件的对应路由被 confirm 前调用
    // 不！能！获取组件实例 `this`
    // 因为当守卫执行前，组件实例还没被创建
  },
  beforeRouteUpdate (to, from, next) {
    // 在当前路由改变，但是该组件被复用时调用
    // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
    // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
    // 可以访问组件实例 `this`
  },
  beforeRouteLeave (to, from, next) {
    // 导航离开该组件的对应路由时调用
    // 可以访问组件实例 `this`
  }
}
```

详细可见官网[组件内的守卫](https://router.vuejs.org/zh/guide/advanced/navigation-guards.html#%E7%BB%84%E4%BB%B6%E5%86%85%E7%9A%84%E5%AE%88%E5%8D%AB)

### vue-router(keep-alive)

通过添加`<keep-alive>`使被包含的组件保留状态，或避免重新渲染。

```html
//App.vue
<keep-alive>
  <router-view/>
</keep-alive>

//include和exclude中填字符串或正则表达式
<keep-alive exclude="组件的name,组件的name">
  //任何匹配到的组件都不会被缓存
  //注意','后不能加空格
</keep-alive>

<keep-alive exclude="组件的name,组件的name">
  //只有匹配到的组件才会被缓存
</keep-alive>
```

> 注意一点：activated,deactivated这两个生命周期函数一定是要在使用了keep-alive组件后才会有的，否则则不存在

若是因为重定向默认挂载某个组件而无法使用`<keep-alive>`返回之前的组件：

```js
//Home.vue
export default {
    name: "Home",
    data() {
      return {
        path: '/home/news'
      }
    },
    // created() {
  	// 创建完毕状态：完成了 data 数据的初始化，el没有
    // },
    // deactivated() {
  	// 销毁完成状态
    // },
    activated() {
    // 组件在激活状态时调用
    // console.log('activated');
      //返回时push到之前的
      this.$router.push(this.path);
    },
    // deactivated() {
  	// 	 组件被移除时调用
    //   console.log('deactivated');
    // },
    beforeRouteLeave (to, from, next) {
    // 导航离开该组件的对应路由时调用
      console.log(this.$route.path);
      // 离开时记录最后活跃的路径名
      this.path = this.$route.path;
      next()
    }
  }
```

### 文件路径引入问题（别名）

若是有些导入路径太麻烦，import时可以用@/components/。。。来指定从src根目录开始导入，而例如img或style就要用～@/。。。来导入。

### style中的scoped的使用

在`vue`组件中我们我们经常需要给`style`添加`scoped`来使得当前样式只作用于当前组件的节点。添加`scoped`之后，实际上`vue`在背后做的工作是将当前组件的节点添加一个像`data-v-1233`这样唯一属性的标识，当然也会给当前`style`的所有样式添加`[data-v-1233]`这样的话，就可以使得当前样式只作用于当前组件的节点。

> 注意因为App.vue是主容器，不能在其中的style再添加`scoped`了，若父组件有`scoped`，子组件没有设置，同样，也是不能在父组件中设置子组件的节点的样式的，因为父组件用了`scoped`,那么父组件中`style`设置的样式都是唯一的了，不会作用与其他的组件样式。

## 二、Promise的基本使用

- 一般情况下是有异步操作时，使用promise对这个一步操作进行封装
- new -> 构造函数（1.保存了一些状态信息，2.执行传入的函数）
- 在执行传入的回调函数时会传入两个参数，resolve（成功时执行），reject（失败时执行），这两个本身也是函数

```javascript
    new Promise((resolve, reject) => {
      //假设第一次网络请求
      setTimeout(() => {
        //成功之后调用then()中方法
        resolve()
        //失败后调用catch()中的方法
      }, 1000)
    }).then(() => {
      console.log('hello 1');
      return new Promise((resolve, reject) => {
        //假设第二次网络请求
        setTimeout(() => {
          //调用之后then()中方法
          resolve()
        }, 1000)
      }).then(() => {
        console.log('hello 2');
      })
    }).catch(() => {
      console.log('false');
    })
```

### Promise的链式调用

```javascript
    new Promise((resolve, reject) => {
      setTimeout(() => {
        resolve("aaa")
      }, 1000)
    }).then(res => {
      //1、先自己处理代码
      console.log(res, '第一次');
      //2、对结果进行第一次处理
      return new Promise(resolve => {
        resolve(res + "111")
      }).then(res => {
        console.log(res, '第2次');
        //简化一
        return Promise.resolve(res + "222");
        //要检测错误时
        // return  Promise.reject();
        //或
        // throw "err"
      }).then(res => {
        console.log(res, '第三次');
        //简化二
        return res + "333"
      }).catch(err => {
        console.log("err")
      })
    })
```

### Promise的all方法使用

```javascript
    //当前两个网络请求都成功时用then
    new Promise.all([
      new Promise(resolve => {
        setTimeout(() => {
          resolve('11')
        }, 1000);
      }),
      new Promise(resolve => {
        setTimeout(() => {
          resolve('22')
        }, 1000);
      })
    ]).then(results => {
      console.log(results);
    })
```



# Vue学习(9)-vuex

## 一、vuex的基本使用

![vuex](https://vuex.vuejs.org/vuex.png)

Store\index.js

```js
import Vue from 'vue'
import Vuex from 'vuex'

//安装插件
Vue.use(Vuex)

// 创建对象
export default new Vuex.Store({
  state: {
    //
    counter: 1000
  },
  mutations: {
  },
  actions: {
  },
  modules: {
  }
})
```

### Mutations

mutation是用来追踪状态变化，可在devtools里查看

```js
//index.js

 mutations: {
    //方法
    increment(state) {
      state.counter++
    },
    decrement(state) {
      state.counter--
    }
  },
```

```js
//App.vue
<template>
		<button @click="addition">+</button>
    <button @click="subtraction">-</button>
</template>

<script>    
    methods: {
      addition() {
        this.$store.commit('increment')
      },
      subtraction() {
        this.$store.commit('decrement')
      }
    }
</script>    
```

Mutation可以携带参数传参

```js
//.vue
<button @click="addCount(5)">+5</button>
<button @click="addStu">++</button>

			addCount(count) {
        //Payload载荷
        this.$store.commit('incrementCount', count)
      },
      addStu() {
        const stu = {id: 3, text: '...', done: false}
        this.$store.commit('incrementStu', stu)
      },
			addCount2(count) {
        this.$store.commit({
          type: 'incrementCount2',
          count
        })
```

```js
    incrementCount(state, count) {
      state.counter += count
    },
    incrementStu(state, stu) {
      state.todos.push(stu)
    }
		//另一种提交风格
		incrementCount2(state, payload) {
      state.counter += payload.count
    }
```

> Mutations中必须是同步操作，异步的话devtool会无法追踪，就要用到action

### Getters

可以认为是 store 的计算属性

```javascript
state: {
    counter: 1000,
    todos: [
      { id: 1, text: '...', done: true },
      { id: 2, text: '...', done: false },
      { id: 3, text: '...', done: false }
    ]
  },
getters: {
    //对state中数据进行处理
    powerTodos(state) {
      return state.todos.filter(s => s.id >1)
    },
    //还可同时调用getters里的数据
    doneTodos(state, getters) {
      return getters.powerTodos.length
    },
    //想要在网页通过传来的数进行处理
    cutTodos(state) {
      return function (id) {
        return state.todos.filter(s => s.id > id)
      }
    }
  },   

//.vue
<div>    
    <h2>{{$store.getters.powerTodos}}</h2>
    <h2>{{$store.getters.doneTodos}}</h2>
    <h2>{{$store.getters.cutTodos(2)}}</h2>
</div>
```

### vuex的数据响应原理

```vue
changeInfo() {
        this.$store.commit('updateInfo')
      }
```

```js
updateInfo(state) {
      //注释掉的是非响应式的
      // state.info.id = '6'
      Vue.set(state.info, 'id', '6')

      // delete state.info.id
      Vue.delete(state.info, 'id')
    }
```

### Actions

> Action 类似于 mutation，不同在于：
>
> - Action 提交的是 mutation，而不是直接变更状态。
> - Action 可以包含任意异步操作。
>
> Action 函数接受一个与 store 实例具有相同方法和属性的 context 对象，因此你可以调用 `context.commit` 提交一个 mutation，或者通过 `context.state` 和 `context.getters` 来获取 state 和 getters。

```js
//.vue
 updateInfo() {
        this.$store
            .dispatch('a_update', 'news')
            .then(res => {
              console.log(res);
            })
      }
```

```js
//index.js
actions: {
    a_update(context, payload) {
      return new Promise((resolve, reject) => {
        setTimeout(() => {
          context.commit('updateInfo');
          console.log(payload);
          resolve('111')
        }, 1000)
      })
    }
  },
```

### Modules

由于使用单一状态树，应用的所有状态会集中到一个比较大的对象。当应用变得非常复杂时，store 对象就有可能变得相当臃肿。

为了解决以上问题，Vuex 允许我们将 store 分割成**模块（module）**。每个模块拥有自己的 state、mutation、action、getter、甚至是嵌套子模块——从上至下进行同样方式的分割：

```js
const moduleA = {
  state: () => ({ ... }),
  mutations: { ... },
  actions: { ... },
  getters: { ... }
}

const moduleB = {
  state: () => ({ ... }),
  mutations: { ... },
  actions: { ... }
}

const store = new Vuex.Store({
  modules: {
    a: moduleA,
    b: moduleB
  }
})

store.state.a // -> moduleA 的状态
store.state.b // -> moduleB 的状态
```

> 为了方便管理，可以将各个功能抽离，在导入会store中。



# Vue学习(10)--axios

## 一、axios基本使用

### get/post

```js
axios.get('/user?ID=12345')
  .then(response => {
    console.log(response);
  })
  .catch(error => {
    console.log(error);
  });

// 可选地，上面的请求可以这样做
axios.get('/user', {
  params: {
    ID: 12345
  }
})
  .then(response => {
    console.log(response);
  })
  .catch(error => {
    console.log(error);
  });

//post，拼接的时候用data
axios.post('/user', {
  firstName: 'Fred',
  lastName: 'Flintstone'
})
  .then(response => {
    console.log(response);
  })
  .catch(error => {
    console.log(error);
  });
```

### 执行多个并发请求

```js
function getUserAccount() {
  return axios.get('/user/12345');
}

function getUserPermissions => {
  return axios.get('/user/12345/permissions');
}

axios.all([getUserAccount(), getUserPermissions()])
  .then(axios.spread((acct, perms) => {
 		// axios.spread()是用来将axiosall返回的数组进行拆分的
	  // 两个请求现在都执行完成
  }));
```

### 一些可以设置的全局默认配置

```js
axios.defaults.baseURL = '默认地址';
axios.defaults.timeout = 5000
```

因为并不是全都会使用全局aixos，要针对不同的请求来进行不用的配置

```js
//创建一个axios实例
const instance1 = axios.create({
  baseURL: '默认地址',
  timeout: 5000
})

instance1({
  url: '拼接地址'
}).then(res => {
  console.log(res);
})
```

### 对axios进行模块封装

防止日后axios出问题时维护困难

```js
//request.js
//封装axios

import axios from 'axios';

export function request(config, success, failure) {
  //创建实例
  const instance = axios.create({
    baseURL: '',
    timeout: 5000
  })

  //发送网络请求
  instance(config)
    .then(
      res => {
        success(res);
      }
    )
    .catch(
      err => {
        failure(err);
      }
    )

}
```

```js
//.vue
//组件调用

import {request} from "./network/request";

request({
  url: ''
}, res => {
  console.log(res);
}, err => {
  console.log(err);
})
```

再进一步封装

```js
import axios from 'axios';

export function request(config) {
  return new Promise((resolve, reject) => {
    //创建实例
    const instance = axios.create({
      baseURL: '',
      timeout: 5000
    })

    //发送网络请求
    instance(config)
      .then(
        res => {
          resolve(res);
        }
      )
      .catch(
        err => {
          reject(err);
        }
      )
  })
}
```

```js
import {request} from "./network/request";

request({
  url: '',
}).then(res => {
  console.log(res);
}).catch(err => {
  console.log(err);
})
```

因为instance返回的就是Promise，所以可以直接返回instance

```js
//request.js
import axios from 'axios';

export function request(config) {
    //创建实例
    const instance = axios.create({
      baseURL: '',
      timeout: 5000
    })

    //发送网络请求
    return instance(config)
}
```

axios的拦截器

因为听说前端拦截不太管用，先搁置着先。（结果很快就用上了。。。）

