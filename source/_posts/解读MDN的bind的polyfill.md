---
title: 解读MDN的bind的polyfill
date: 2018-5-08 9:07:25
tags: polyfill
categories: JavaScript
---
[https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind#Polyfill](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind#Polyfill)

```javascript
if (!Function.prototype.bind) {
  Function.prototype.bind = function(oThis) {
    // 如果给函数对象的属性而不是方法来调用bind
    // 如:
    // fn = function () {}
    // fn.test = {}
    // fn.test.bind(this)
    // 则抛出异常
    if (typeof this !== 'function') {
      // closest thing possible to the ECMAScript 5
      // internal IsCallable function
      throw new TypeError('Function.prototype.bind - what is trying to be bound is not callable');
    }

    var aArgs   = Array.prototype.slice.call(arguments, 1),
        // 待绑定的函数
        fToBind = this,
        // 中间构造函数
        fNOP    = function() {},
        // bind返回的函数
        fBound  = function() {
          // 用new去调用bind返回的函数, 之前绑定的oThis会失效
          return fToBind.apply(this instanceof fNOP
                 ? this
                 : oThis,
                 aArgs.concat(Array.prototype.slice.call(arguments)));
        };

    // 如果待绑定的函数有原型对象, 考虑Function.prototype没有原型对象
    if (this.prototype) {
      // typeof Function.prototype === 'function'
      // Function.prototype doesn't have a prototype property
      // 让中间构造函数的原型对象为待绑定函数的原型对象
      fNOP.prototype = this.prototype; 
    }
    // bind返回的函数的原型对象为中间构造函数的一个实例中间空对象
    fBound.prototype = new fNOP();

    return fBound;
  };
}
```