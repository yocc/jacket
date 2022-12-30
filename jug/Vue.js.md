# Vue.js

----

[toc]





## Terminology

```shell

Vite, 
脚手架 scaffolding, 官方, 构建工具.
$ npm init vue@latest
https://cn.vitejs.dev/

Vue CLI, 
官方提供的基于 Webpack 的 Vue 工具链.

IDE,
VSCode 插件 Volar, https://github.com/johnsoncodehk/volar
vim 插件 coc-volar, https://github.com/yaegassy/coc-volar
Chrome 插件 https://chrome.google.com/webstore/detail/vuejs-devtools/nhdogjmejiglipccpnnnanhbledajbpd

更多,
https://cn.vuejs.org/guide/scaling-up/tooling.html#project-scaffolding
```



## e.g.

```shell
See Also: https://cn.vuejs.org/guide/quick-start.html#creating-a-vue-application
1. 前提条件
熟悉命令行
已安装 16.0 或更高版本的 Node.js

2. $ npm init vue@latest
这一指令将会安装并执行 create-vue, https://github.com/vuejs/create-vue, 它是 Vue 官方的项目脚手架工具.
你将会看到一些诸如 TypeScript 和测试支持之类的可选功能提示：
✔ Project name: … <your-project-name>
✔ Add TypeScript? … No / Yes
✔ Add JSX Support? … No / Yes
✔ Add Vue Router for Single Page Application development? … No / Yes
✔ Add Pinia for state management? … No / Yes
✔ Add Vitest for Unit testing? … No / Yes
✔ Add Cypress for both Unit and End-to-End testing? … No / Yes
✔ Add ESLint for code quality? … No / Yes
✔ Add Prettier for code formatting? … No / Yes

Scaffolding project in ./<your-project-name>...
Done.

> cd <your-project-name>
> npm install
> npm run dev
> npm run build

全局构建版本,
CDN, <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
全局构建版本的 Vue，该版本的所有顶层 API 都以属性的形式暴露在了全局的 Vue 对象上.
<script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
<div id="app">{{ message }}</div>
<script>
  const { createApp } = Vue
  createApp({
    data() {
      return {
        message: 'Hello Vue!'
      }
    }
  }).mount('#app')
</script>

ES 模块构建版本,
https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Modules
一起的开始.
```





## Overview





## FAQ & troubleshooting



## See Also

