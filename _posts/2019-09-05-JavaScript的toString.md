---
layout:     post
title:      JavaScript的toString()详解
# subtitle:   --随机数应用
date:       2019-09-05
author:     CNJ
header-img: img/post-bg-js-version.jpg
catalog: true
tags:
    - 开发技巧
    - JS细节
---

# 基本概念
JavaScript中有许多常用的方法，其中转换字符串最常用的就是toString()方法。toString方法在JavaScript对应的其中数据类型当中除了null和undefined，都有toString()方法。
## Boolean类型的toString()
对于Boolean对象或值，内置的 toString 方法返回字符串 "true" 或 "false"，具体返回哪个取决于布尔对象的值

## Number类型的toString()
Number类型的toString会返回Number对象的字符串表现形式，但是Number类型的toString()方法存在一个可传递参数radix，这个值的取值范围是(2, 36)，表toString()的Number对象是什么进制，默认为10进制。如果超过2-36的范围，JavaScript会抛出错误值异常。

注:为什么最大值是36，是因为toString()只能做到0-9加上字母a-z。直接对数字进行toString()需要加上小括号括起来。

```Javascript
var number = 10;
console.log(number.toString());    // 输出 '10' 
console.log(number.toString(2));   // 输出 '1010' 
console.log(number.toString(16));  // 输出 'a' 
console.log(number.toString(36));  // 输出 'a' 
```
## String类型的toString()
String类型的toString()，返回字符串本身。

## Object类型的toString()
Object类型的toString()，被继承在每个对象的原型当中，如果未被对象自行覆盖原始的toString()，toString()会返回"[Object Object]"。

但是，我们能够通过改变对象的原型链toString()方法去获取我们想要获取的值。

具体如下：
```Javascript
var obj = new Object();

obj.prototype.toString = function objToString() {
    return 'this is a new toString()'
}
```