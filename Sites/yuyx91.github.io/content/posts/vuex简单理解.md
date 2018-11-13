---
title: vuex简单理解
date: 2017-04-25 16:54:40
tags: 'vue'
---
Vuex 是一个专为 Vue.js 应用程序开发的状态管理模式。它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。
Vuex 也集成到 Vue 的官方调试工具 devtools extension，提供了诸如零配置的 time-travel 调试、状态快照导入导出等高级调试功能。
简而言之：Vuex 相当于某种意义上设置了读写权限的全局变量，将数据保存保存到该“全局变量”下，并通过一定的方法去读写数据。
**Vuex 并不限制你的代码结构。但是，它规定了一些需要遵守的规则：**

- 应用层级的状态应该集中到单个 store 对象中。

- 提交 mutation 是更改状态的唯一方法，并且这个过程是同步的。

- 异步逻辑都应该封装到 action 里面。

对于大型应用我们会希望把 Vuex 相关代码分割到模块中。下面是项目结构示例：
```
   ├── index.html
   ├── main.js
   ├── api
   │   └── ... # 抽取出API请求
   ├── components
   │   ├── App.vue
   │   └── ...
   └── store
       ├── index.js          # 我们组装模块并导出 store 的地方
       └── moving            # 电影模块
           ├── index.js      # 模块内组装，并导出模块的地方
           ├── actions.js    # 模块基本 action
           ├── getters.js    # 模块级别 getters
           ├── mutations.js  # 模块级别 mutations
           └── types.js      # 模块级别 types
```
转自[Vue服务端渲染和Vue浏览器端渲染的性能对比](http://www.cnblogs.com/tiedaweishao/p/6644267.html)