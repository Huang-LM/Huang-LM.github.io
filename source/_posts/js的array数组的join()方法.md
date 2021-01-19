---
title: js的array数组的join()方法
date: 2020-11-23 21:26:07
tags: "js"
categories: "js学习"
cover: https://7.dusays.com/2020/11/23/e28371b2c0754.jpg
hide: true
---



之前关于组合数组元素一直用着很蠢的for(let i of array)来循环组合出数组中的元素，然后知道做题才发现有个数组内置的join方法可以方便的解决之前的操作。

```js
// 1、将数组中每个元素转换成字符串
const num = ['q', 'we', 'r'];
console.log(num.join());
// 结果： q,we,r

// 2、将数组中每个元素组合拼接在一起
console.log(num.join(""));
// 结果：qwer

// 3、各元素中间加上间隔
console.log(num.join(" "));
// 结果：q we r
console.log(num.join("|"));
// 结果：q|we|r

// 4、判断数组是不是空字符数组，根据返回的是否为空白就可以判断是不是空字符数组
const num1 = [,,,,,]
console.log(num1.join(""));
// 结果：

// 5、神奇的用法
const array=["北京市","上海市","广州市","深圳市"];
const html="<option>" + array.join("</option><option>")+ "</option>";
// 相当于把数组中的元素依次放进<option></option>中间
```

总的来说join方法就是根据()输入的参数来对数组进行组合成一个字符串。

