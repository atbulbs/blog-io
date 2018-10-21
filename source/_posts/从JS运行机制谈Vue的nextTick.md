---
title: 从JS运行机制谈Vue的nextTick
date: 2018-07-27 13:57:13
tags: 源码探秘
categories: vue
---

## 从JavaScript的单线程谈起
&#160; &#160; &#160; &#160;什么是单线程, 大白话: 同一时间, 只能做一件事情. JavaScript的诞生主要是为了操作DOM, 如果有多个线程, 假设线程A在它的时间片里去修改DOM_FOO, 但是来不及修改完就得退出CPU, 进入就绪状态, 等待下一次调度, 接着线程B占用CPU, 然而它的任务是删除DOM_FOO, 而且线程B这哥们比较麻溜儿的在一个时间片里就把DOM_FOO给干掉了, 功成身退, 等到线程A再来执行任务的时候压根就找不到DOM_FOO,这下就尴尬了:joy:, 完不成任务该咋整啊, 要主人来一套锁机制吧, 会不会成本太高, 布兰登巴巴一开始就把它设计成了单线程(而且与浏览器渲染DOM共用一个线程), 并被瑞安大大发扬光大, 膜拜一下我JS两大男神:heart_eyes:

<!-- more -->

![brendan-eich](/images/next-tick/brendan-eich.jpg)

![ryan-dahl](/images/next-tick/ryan-dahl.jpg)


&#160; &#160; &#160; &#160;JavaScript的事件轮询Event Loop: 同步任务在执行环境栈ECS(Execution Context Stack)中运行, 主线程执行一次叫做一个tick, 异步任务有了结果后进入任务队列TQ(Task Quene)执行, task分为macro task(如 setImmediate, MessageChannel, setTimeout)和 micro task(如Promise.then), 每个macro task结束后, 所有的micro task 都会被handle, 主线程空了就回去读取任务队列, 而且定时器的执行时间到了才能返回主线程, 如此重复重复再重复

![event-loop](/images/next-tick/event-loop.png)

&#160; &#160; &#160; &#160;setTimeout(fn, 0), 在任务队列尾部添加事件, HTML5标准中规定延迟时间不低于4ms, 所以你写的0, 实际执行时在4ms以上

&#160; &#160; &#160; &#160;主流屏幕刷新率在60hz, DOM的变动不是立即执行, 而是每16毫秒执行一次

&#160; &#160; &#160; &#160;node.js中process.nextTick方法可以在当前执行栈的尾部和下一次Event Loop(主线程读取任务队列)之前触发回调函数,它指定的任务优先于所有异步任务之前执行, node.js中setImmediate方法则是在当前任务队列的尾部添加事件, 相当于window中的setTimeout(fn, 0)

## Vue中的nextTick

&#160; &#160; &#160; &#160;见官方文档, nextTick的应用场景, 大白话, 数据变化后, DOM在下一个tick才会重新渲染 

![vue-doc-1](/images/next-tick/vue-doc-1.png)

![vue-doc-2](/images/next-tick/vue-doc-2.png)

&#160; &#160; &#160; &#160;nextTick代码在vue源码核心的工具类里vue/src/core/util/next-tick.js, 对外暴露两个函数, withMacroTask 和 nextTick

```javascript

/* @flow */
/* globals MessageChannel */

import { noop } from 'shared/util'
import { handleError } from './error'
import { isIOS, isNative } from './env'

const callbacks = []
let pending = false

function flushCallbacks () {
  pending = false
  const copies = callbacks.slice(0)
  callbacks.length = 0
  for (let i = 0; i < copies.length; i++) {
    copies[i]()
  }
}

// Here we have async deferring wrappers using both microtasks and (macro) tasks.
// In < 2.4 we used microtasks everywhere, but there are some scenarios where
// microtasks have too high a priority and fire in between supposedly
// sequential events (e.g. #4521, #6690) or even between bubbling of the same
// event (#6566). However, using (macro) tasks everywhere also has subtle problems
// when state is changed right before repaint (e.g. #6813, out-in transitions).
// Here we use microtask by default, but expose a way to force (macro) task when
// needed (e.g. in event handlers attached by v-on).
let microTimerFunc
let macroTimerFunc
let useMacroTask = false

// Determine (macro) task defer implementation.
// Technically setImmediate should be the ideal choice, but it's only available
// in IE. The only polyfill that consistently queues the callback after all DOM
// events triggered in the same loop is by using MessageChannel.
/* istanbul ignore if */
if (typeof setImmediate !== 'undefined' && isNative(setImmediate)) {
  macroTimerFunc = () => {
    setImmediate(flushCallbacks)
  }
} else if (typeof MessageChannel !== 'undefined' && (
  isNative(MessageChannel) ||
  // PhantomJS
  MessageChannel.toString() === '[object MessageChannelConstructor]'
)) {
  const channel = new MessageChannel()
  const port = channel.port2
  channel.port1.onmessage = flushCallbacks
  macroTimerFunc = () => {
    port.postMessage(1)
  }
} else {
  /* istanbul ignore next */
  macroTimerFunc = () => {
    setTimeout(flushCallbacks, 0)
  }
}

// Determine microtask defer implementation.
/* istanbul ignore next, $flow-disable-line */
if (typeof Promise !== 'undefined' && isNative(Promise)) {
  const p = Promise.resolve()
  microTimerFunc = () => {
    p.then(flushCallbacks)
    // in problematic UIWebViews, Promise.then doesn't completely break, but
    // it can get stuck in a weird state where callbacks are pushed into the
    // microtask queue but the queue isn't being flushed, until the browser
    // needs to do some other work, e.g. handle a timer. Therefore we can
    // "force" the microtask queue to be flushed by adding an empty timer.
    if (isIOS) setTimeout(noop)
  }
} else {
  // fallback to macro
  microTimerFunc = macroTimerFunc
}

/**
 * Wrap a function so that if any code inside triggers state change,
 * the changes are queued using a (macro) task instead of a microtask.
 */
export function withMacroTask (fn: Function): Function {
  return fn._withTask || (fn._withTask = function () {
    useMacroTask = true
    const res = fn.apply(null, arguments)
    useMacroTask = false
    return res
  })
}

export function nextTick (cb?: Function, ctx?: Object) {
  let _resolve
  callbacks.push(() => {
    if (cb) {
      try {
        cb.call(ctx)
      } catch (e) {
        handleError(e, ctx, 'nextTick')
      }
    } else if (_resolve) {
      _resolve(ctx)
    }
  })
  if (!pending) {
    pending = true
    if (useMacroTask) {
      macroTimerFunc()
    } else {
      microTimerFunc()
    }
  }
  // $flow-disable-line
  if (!cb && typeof Promise !== 'undefined') {
    return new Promise(resolve => {
      _resolve = resolve
    })
  }
}

```

&#160; &#160; &#160; &#160;nextTick在vue/src/core/global-api/index.js作为静态方法被绑定在Vue的构造函数上

![global-api](/images/next-tick/global-api.png)

&#160; &#160; &#160; &#160;nextTick在vue/src/core/instance/render.js绑定在Vue原型对象上并且this会指向调用$nextTick的实例

![instance-render](/images/next-tick/instance-render.png)

&#160; &#160; &#160; &#160;对macro task的实现, 依次判断是否支持原生的setImmediate和原生的MessageChannel, 否则降级为setTimeout(flushCallbacks, 0)

&#160; &#160; &#160; &#160;对micro task的实现, 判断是否支持原生的Promise, 否则降级为macro task

&#160; &#160; &#160; &#160;nextTick函数, 把传入的回调函数cb压入callbacks数组, 最后一次性地根据useMacroTask条件执行macroTimerFunc或者是 microTimerFunc, 而它们都会在下一个tick执行flushCallbacks

&#160; &#160; &#160; &#160;flushCallbacks拷贝并遍历callbacks并执行相应的回调函数

&#160; &#160; &#160; &#160;如果nextTick函数没有接收到cb而且支持Promise, 则提供Promise化的调用