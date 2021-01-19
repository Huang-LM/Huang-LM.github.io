---
title: emmet语法的使用
date: 2020-10-19 14:24:11
tags: "html"
categories: "前端"
cover: https://7.dusays.com/2020/10/23/af9999e571753.jpg
---

## id指令:# ; class指令:.

```html
//div#test
<div id="test"></div>

//div.test
<div class="test"></div>
```

## 子节点指令:> ; 兄弟节点指令:+ ; 上级节点:^

`div>ul>li>p`

```html
<div>
   <ul>
     <li>
       <p></p>
     </li>
   </ul>
 </div>
```

`div+ul+p`

```html
<div></div>
<ul></ul>
<p></p>
```

`ul>li^div`

```html
<div>
   <ul>
     <li>
     </li>
   </ul>
 </div>
```

## 重复（*）

`div*3`

```html
   <div></div>
   <div></div>
   <div></div>
```

编号（$）

`div>ul>li.test$*3`

```html
<div>
  <ul>
    <li class="test1"></li>
    <li class="test2"></li>
    <li class="test3"></li>
  </ul>
</div>
```



```html
//带有层级结构的：ul>li
	<ul>
		<li></li>
  </ul>

//ul>li{消息$}*4
	<ul>
    <li>消息1</li>
    <li>消息2</li>
    <li>消息3</li>
    <li>消息4</li>
	</ul>

//创建带有指定class样式的标签：div.box
	<div class="box"></div>

//文本内容填充：a{首页}
<a href="">首页</a>
```

```html
隐式标签：

>   .class
    <div class="class"></div>

>   #id
    <div id="id"></div>

>   ul>.class
    <ul>
        <li class="class"></li>
    </ul>

>   label[for='content']>#content
    <label for="content">
        <span id="content"></span>
    </label>
```

```html
$符号自增

    ul>li.$*3
    <ul>
        <li class="1"></li>
        <li class="2"></li>
        <li class="3"></li>
    </ul>

    ul>li{第$$条项目}*3
    <ul>
        <li>第01条项目</li>
        <li>第02条项目</li>
        <li>第03条项目</li>
    </ul>

最后来个复合式的案例：

    ul>li[id='item$']{第$$$条数据}*10
    <ul>
        <li id="item1">第001条数据</li>
        <li id="item2">第002条数据</li>
        <li id="item3">第003条数据</li>
        <li id="item4">第004条数据</li>
        <li id="item5">第005条数据</li>
        <li id="item6">第006条数据</li>
        <li id="item7">第007条数据</li>
        <li id="item8">第008条数据</li>
        <li id="item9">第009条数据</li>
        <li id="item10">第010条数据</li>
    </ul>
```

