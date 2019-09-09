---
layout:     post
title:      Vue-Router使用详解
# subtitle:   --随机数应用
date:       2019-09-09
author:     CNJ
header-img: img/post-bg-2015.jpg
catalog: true
tags:
    - 开发技巧
    - JS细节
---

# 基本概念
Vue的路由概念，乃至Angular、React等单页面应用，与传统路由的概念不一样，它相当于一个单页应用(Single Page web Application，简称SPA)的路径管理器。传统路由主要通过超链接(例如<a>标签或window.location等)进行页面的切换，但是SPA应用的路径切换本质上是组件之间的切换，切换的依据就是各自的路径管理器。
## Vue-Router
Vue-Router 是一个语Vue.js深度集成的路由插件。SPA路由的核心是更新视图而不重新请求页面。Vue-Router提供了两种路由模式：1)Hash模式；2)History模式，根据mode参数来决定采用路由模式。
### 1)Hash模式
vue-router 默认 hash 模式 —— 使用 URL 的 hash 来模拟一个完整的 URL，于是当 URL 改变时，页面不会重新加载。 hash（#）是URL 的锚点，代表的是网页中的一个位置，单单改变#后的部分，浏览器只会滚动到相应位置，不会重新加载网页，也就是说hash 出现在 URL 中，但不会被包含在 http 请求中，对后端完全没有影响，因此改变 hash 不会重新加载页面。
### 2)History模式
这种模式充分利用了HTML5 history interface 中新增的 pushState() 和 replaceState() 方法。这两个方法应用于浏览器记录栈，在当前已有的 back、forward、go 基础之上，它们提供了对历史记录修改的功能。只是当它们执行修改时，虽然改变了当前的 URL ，但浏览器不会立即向后端发送请求。

```javascript
// 定义路由: lin
 export const routes = [ 
  {path: "/", name: "homeLink", component:Home}
  {path: "/register", name: "registerLink", component: Register},
  {path: "/login?userId=:userId", name: "loginLink", component: Login},
  {path: "*", redirect: "/"}]

//main.js文件中
const router = new VueRouter({
  mode: 'history',
  routes: routes
})
```