---
title: EventEmitter的实现
date: 2018-06-11 09:08:17
tags: 观察者模式
categories: 设计模式
---

### ES6的实现
<!-- more -->
```javascript
class EventEmitter {
  constructor () {
    this.listenList = {}
  }
  on (eventName, callback) {
    const callbacks = this.listenList[eventName] || []
    callbacks.push(callback)
    this.listenList[eventName] = callbacks
  }
  emit (eventName, ...args) {
    const callbacks = this.listenList[eventName]
    if (callbacks && callbacks.length) {	
      callbacks.forEach((callback) => {	
        callback(...args)
      })
    } 
  }
  off (eventName, functionName) {
    const callbacks = this.listenList[eventName]
    if (callbacks && callbacks.length) {
      if (functionName && callbacks.includes(functionName)) {	
        const index = callbacks.indexOf(funtionName)
        callback.splice(index, 1)
      } else if (!functionName) {
        delete this.listenList[eventName]
      }	
    }
  }
}
```