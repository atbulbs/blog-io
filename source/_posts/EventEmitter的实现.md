---
title: EventEmitter的实现
date: 2018-06-11 09:08:17
tags: 观察者模式
categories: 设计模式
---

### ES6的实现
<!-- more -->
```javascript
export class EventEmitter {
  constructor () {
    this.listenerList = {}
  }
  on (e, cb) {
    const cbs = this.listenerList[e] || []
    if (typeof cb !== 'function') {
      throw new TypeError(`${e}.${cb} is not a function`)
    } else {
      cbs.push(cb)
    }
    this.listenerList[e] = cbs
  }
  emit (e) {
    const cbs = this.listenerList[e]
    const restArgs = Array.prototype.slice.call(arguments, 1)
    if (cbs && cbs.length) {
      cbs.forEach((cb) => {
        cb(restArgs)
      })
    }
  }
  off (e, fn) {
    const cbs = this.listenerList[e]
    if (cbs && cbs.length) {
      if (fn && cbs.includes(fn)) {
        const index = cbs.indexOf(fn)
        cbs.splice(index, 1)
      } else if (!fn) {
        delete this.listenerList[e]
      }
    }
  }
}

```