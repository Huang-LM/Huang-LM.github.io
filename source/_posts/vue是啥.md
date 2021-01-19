---
title: vue是啥
date: 2020-11-09 22:46:08
tags: "Vue"
categories: "前端"
cover: https://7.dusays.com/2020/10/23/cfb9789f2aa74.jpg
auto_open: true
---

# Vue是什么？

> Vue是一套用于构建用户界面的渐进式框架，Vue 被设计为可以自底向上逐层应用，Vue的核心是只关注视图层，不仅易于上手，还便于与第三方库或既有项目整合。

vue.js的两大核心：**数据驱动、组件系统**

- 数据驱动：
  在vue中，数据的改变会驱动视图的自动更新。传统的做法是需要手动改变DOM来使得视图更新，而vue只需要改变数据。

- 组件系统:
  组件化开发，优点很多，可以很好的降低数据之间的耦合度。将常用的代码封装成组件之后，就能高度的复用，提高代码的可重用性。一个页面/模块可以由多个组件所组成。

## 渐进式框架

就是一步一步渐进的意思，在使用vue的时候不要求把框架所有东西都用上。就可以先把vue核心库里的东西声明式渲染和组件系统给先用上，然后至于其他的客户端路由（router）、状态管理（vuex）、构建工具（vue-cli），你可以选择是否要使用上这些东西来开发你的程序，不一定全都要一起用着。

![](https://7.dusays.com/2020/11/25/c0fb248b1d2bc.jpg)

## 自底向上逐层应用

就可以字面意思理解，先写好基层，然后逐步逐层添加东西，就和搭积木一样。

## MVVM

vue是个根据mvvm（Model-View-ViewModel ）框架设计的库，基于前端开发的架构模式，主要就是实现view（视图层）和model（数据层）的双向数据绑定，其中viewModel（视图模型层）就是负责连接view和model的。

- 原生dom绑定：

  ```js
  var dom = document.getElementById('name');
  dom.innerHTML = 'Homer';
  dom.style.color = 'red';
  ```

- jqury

  ```js
  $('#name').text('Homer').css('color', 'red');
  ```

- mvvm

  ```js
  var person = {
      name: 'Bart',
      age: 12
  };
  
  person.name = 'hello';
  person.age = 18
  ```

## 组件化开发

vue允许你将网页分成一个个可以复用的组件，每个组件都有自己的html，css，js。组件的开发可以大幅减少我们写重复代码，修改时的不便等很多问题，所以我们如果在使用vue的时候要注重一下哪些东西需要或者适合拆分出组件来复用。

## 怎么构建

就用vue-cli来构建

## 调用后台接口

- 使用的是axios这个工具，同时也是官方推荐的，本质上也是对原生XHR（XmlHttpRequest）的封装，只不过它是Promise的实现版本，符合最新的ES规范。

- 传统ajax就是指XHR，jquery的ajax是对原生XHR的封装。

- Axios 是一个基于 promise 的 HTTP 库，可以用在浏览器和 node.js 中。

  ```js
  //jquery
  $.get/post("/url", () => {
  
  })
  ```

  ```js
  //axios
  axios
    .get/post("/url", { params: Info })
    .then(() => {})
    .catch(() => {});
  ```

