# element-plus

---



## 目录

[TOC]





## Overview

### 安装

```shell
# 包管理器
# NPM
$ npm install element-plus --save

# 浏览器直接引入
<head>
  <!-- Import style -->
  <link rel="stylesheet" href="//unpkg.com/element-plus/dist/index.css" />
  <!-- Import Vue 3 -->
  <script src="//unpkg.com/vue@3"></script>
  <!-- Import component library -->
  <script src="//unpkg.com/element-plus"></script>
</head>
```

### 用法

```shell
# 完全引入
// main.ts
import { createApp } from 'vue'
import ElementPlus from 'element-plus'
import 'element-plus/dist/index.css'
import App from './App.vue'

const app = createApp(App)

app.use(ElementPlus)
app.mount('#app')

# 按需导入(推荐)
## 先安装 unplugin-vue-components 和 unplugin-auto-import 这两款插件
npm install -D unplugin-vue-components unplugin-auto-import
## 将以下代码插入 vite 或者 webpack 配置文件中
### Vite
// vite.config.ts
import { defineConfig } from 'vite'
import AutoImport from 'unplugin-auto-import/vite'
import Components from 'unplugin-vue-components/vite'
import { ElementPlusResolver } from 'unplugin-vue-components/resolvers'

export default defineConfig({
  // ...
  plugins: [
    // ...
    AutoImport({
      resolvers: [ElementPlusResolver()],
    }),
    Components({
      resolvers: [ElementPlusResolver()],
    }),
  ],
})

### Webpack
// webpack.config.js
const AutoImport = require('unplugin-auto-import/webpack')
const Components = require('unplugin-vue-components/webpack')
const { ElementPlusResolver } = require('unplugin-vue-components/resolvers')

module.exports = {
  // ...
  plugins: [
    AutoImport({
      resolvers: [ElementPlusResolver()],
    }),
    Components({
      resolvers: [ElementPlusResolver()],
    }),
  ],
}
```

想了解更多打包 ([Rollup](https://rollupjs.org/), [Vue CLI](https://cli.vuejs.org/)) 和配置工具，请参考 [unplugin-vue-components](https://github.com/antfu/unplugin-vue-components#installation) 和 [unplugin-auto-import](https://github.com/antfu/unplugin-auto-import#install)。

### 全局配置

具体使用, 具体编码

在引入 Element Plus 时，可以传入一个包含 size 和 zIndex 属性的全局配置对象。 size 用于设置表单组件的默认尺寸，zIndex 用于设置弹出组件的层级，zIndex 的默认值为 2000。

```js
// 完全引入
import { createApp } from 'vue'
import ElementPlus from 'element-plus'
import App from './App.vue'

const app = createApp(App)
app.use(ElementPlus, { size: 'small', zIndex: 3000 })

// 按需引入(推荐)
<template>
  <el-config-provider :size="size" :z-index="zIndex">
    <app />
  </el-config-provider>
</template>

<script>
import { defineComponent } from 'vue'
import { ElConfigProvider } from 'element-plus'

export default defineComponent({
  components: {
    ElConfigProvider,
  },
  setup() {
    return {
      zIndex: 3000,
      size: 'small',
    }
  },
})
</script>
```





## 实际使用 

```shell

npm init vue@latest


1. nvm 环境确认
$ nvm use 17
Now using node v17.7.2 (npm v8.5.2)
$ node -v
v17.7.2
$ npm -v
8.5.2
$ npm install element-plus --save
npm WARN deprecated sourcemap-codec@1.4.8: Please use @jridgewell/sourcemap-codec instead

added 43 packages in 18s
npm notice 
npm notice New major version of npm available! 8.5.2 -> 9.4.2
npm notice Changelog: https://github.com/npm/cli/releases/tag/v9.4.2
npm notice Run npm install -g npm@9.4.2 to update!
npm notice 



1. 通过包管理器安装 ep(Element Plus)
# NPM
$ npm install element-plus --save

2. 为了在代码层面使用按需导入(推荐), 安装 unplugin-vue-components 和 unplugin-auto-import 这两款插件
## 先安装 unplugin-vue-components 和 unplugin-auto-import 这两款插件
npm install -D unplugin-vue-components unplugin-auto-import
## 将以下代码插入 vite 或者 webpack 配置文件中
### Vite
### Webpack
参考: https://element-plus.gitee.io/zh-CN/guide/quickstart.html#%E6%8C%89%E9%9C%80%E5%AF%BC%E5%85%A5

3. 代码层面全局配置
在引入 Element Plus 时, 可以传入一个包含 size 和 zIndex 属性的全局配置对象.
size 用于设置表单组件的默认尺寸.
zIndex 用于设置弹出组件的层级. zIndex 的默认值为 2000.
参考: https://element-plus.gitee.io/zh-CN/guide/quickstart.html#%E5%85%A8%E5%B1%80%E9%85%8D%E7%BD%AE

4.


```





## FAQ

### `<script setup>` 是什么

```
<script setup> 是什么？
<script setup> 是在单文件组件 (SFC) 中使用 composition api 的编译时语法糖。
```









## See Also









