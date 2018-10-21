---
title: this is it in JavaScript
date: 2018-07-1 08:32:13
tags: JavaScript
categories: JavaScript
---

## this是什么
&#160; &#160; &#160; &#160;函数是语句的容器, 而this代表函数调用invocation时的上下文环境context

## this从哪里来
&#160; &#160; &#160; &#160;每个函数默认会接收两个附加的参数, this和arguments

## this的5种应用场景
<!-- more -->
1. 函数调用
&#160; &#160; &#160; &#160;当函数不作为宿主全局Global对象(浏览器里为window, node.js里为global)以外的对象的方法来调用时, this指向全局对象
![window](/images/this/window.png)
![global](/images/this/global.png)

2. 方法调用
&#160; &#160; &#160; &#160;当函数作为宿主全局Global对象(浏览器里为window, node.js里为global)以外的对象的方法来调用时, this指向该方法的拥有者对象
![window-obj](/images/this/window-obj.png)
![global-obj](/images/this/global-obj.png)

3. 构造器调用
&#160; &#160; &#160; &#160;当函数被new调用时, this指向连接到该函数原型对象的新对象
![new](/images/this/new.png)

4. 转移上下文调用
&#160; &#160; &#160; &#160;apply, call, bind是函数的方法, 会将函数执行时的this指向作为参数传入的对象

5. 箭头函数调用
&#160; &#160; &#160; &#160;箭头函数中的this指向定义时所在的对象, 而不是使用时所在的对象

```javascript
// 借用阮大大的栗子
function foo () {
  setTimeout(() => {
    console.log('id:', this.id)
  }, 100)
}

var id = 21

foo.call({ id: 42 })
// id: 42
```
