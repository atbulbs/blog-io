---
title: typescript
date: 2018-10-19 16:08:17
tags: typescript
categories: typescript
---

# TypeScript

## 类

* 访问控制符：定义声明的属性和方法的可访问的范围
  public 公开的(默认的, 可省略)
  private 私有的, 只能在类的内部访问
  protected 只能在类的内部和子类访问
<!-- more -->
```javascript
class AccessControl {
    defualtAccess () {
        console.log('defualtAccess')
        this.defualtAccess_()
        this.protectedAccess()
        this.privateAccess()
    }
    public defualtAccess_ () {
        console.log('defualtAccess_')
    }
    protected protectedAccess () {
        console.log('protectedAccess')
    }
    private privateAccess () {
        console.log('privteAccess')
    }
}

class SubAccessControl extends AccessControl {
    test() {
        this.defualtAccess()
        this.defualtAccess_()
        this.protectedAccess()
        this.privateAccess() // ERR
     }
 }

const AccessTest = new AccessControl()

AccessTest.defualtAccess()
AccessTest.defualtAccess_()
AccessTest.protectedAccess() // ERR
AccessTest.privateAccess() // ERR
```

* 继承关键字
  extends 继承
  super 在子类构造函数里调用父类的构造函数

## 接口

* 作用
  建立代码约定
* 关键字
  interface
  implements
* 用法
1. 接口的属性声明

```javascript
// TS
interface ProgrammeLanguages {
    languages: Array<String>
}

class WebDev {
    constructor(public lang: ProgrammeLanguages) {
        this.lang = lang
    }
    coding () {
        console.dir(this.lang)
     }
}

const FrontEnd = new WebDev({
    languages: ['HTML', 'CSS', 'JavaScript']
})

FrontEnd.coding()

// JS
var WebDev = /** @class */ (function () {
    function WebDev(lang) {
        this.lang = lang
        this.lang = lang
    }
    WebDev.prototype.coding = function () {
        console.dir(this.lang)
    }
    return WebDev
}())
var FrontEnd = new WebDev({
    languages: ['HTML', 'CSS', 'JavaScript']
})
FrontEnd.coding()
```

2. 接口的方法声明

```javascript
// TS
interface WebDev {
    coding()
 }

class FrontEndDev implements WebDev {
    coding() {
        console.log('happy hacking!')
     }
}

const VFE = new FrontEndDev()

VFE.coding()

// JS
var FrontEndDev = /** @class */ (function () {
    function FrontEndDev () {
    }
    FrontEndDev.prototype.coding = function () {
        console.log('happy hacking!')
    }
    return FrontEndDev;
}())
var VFE = new FrontEndDev()
VFE.coding()
```