---
title: javascript设计模式之单例模式
date: 2017-6-7 14:48
tags: [javascript,设计模式,单例模式]
---

### 介绍
<span style="color:#0099ff">单例模式的定义</span>是保证一个类有且只有一个实例存在。

<span style="color:#0099ff">单例模式的的用途</span>试想一下,当我们单击登录按钮的时候,页面中会出现一个登录浮窗,而这个登录浮窗是唯一的,无论单击多少次登录按钮,这个浮窗都只会被创建一次,那么这个登录浮窗就适合用单例模式来创建。比如大家都知道的12306 是唯一购票网站，所有人要网上订票都得访问这个单例。

<span style="color:#0099ff">单例模式带来的好处</span>除了减少不必要的重复的实例创建、减少内存占用外，更重要的是避免多个实例的存在造成逻辑上的错误。比如超级马里奥的游戏中，虽然各种小怪的实例会不断创建多个，但当前的玩家肯定只有一个，如果游戏运行过程中创建出新的马里奥的实例了，显然就出 bug 了。
### 实现
实现的方法是先判断实例存在与否，如果存在直接返回，如果不存在就创建了再返回，这就确保了一个类只有一个实例对象。在JavaScript里，单例作为一个命名空间提供者，从全局命名空间里提供一个唯一的访问点来访问该对象。

``` javascript
var Single = function( name ){
    this.name = name;
    this.instance = null;
};

Single.getInstance = function( name ){
    if ( !this.instance ){
        this.instance = new Single( name );
    }
    return this.instance;
};

var obj1 = Single.getInstance( 'name1' );
var obj2 = Single.getInstance( 'name2' );
alert ( obj1 === obj2 );// true
```

### 扩展
<span style="color:#0099ff">用代理实现单例模式</span>
``` javascript
var Single = function( name ){
    this.name = name;
};

var proxy = (function( name ){
    var instance;
    return function(name){
         if ( !instance ){
             instance = new Single( name );
         }
        return instance;
    } 
})();

var obj1 = proxy( 'name1' );
var obj2 = proxy( 'name2' );
alert ( obj1 === obj2 );// true
```