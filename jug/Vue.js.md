# Vue.js

----

[toc]





## Terminology

```javascript

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

```javascript
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



```javascript
单文件组件
顾名思义, Vue 的单文件组件会将一个组件的逻辑 (JavaScript), 模板 (HTML) 和样式 (CSS) 封装在同一个文件里. 
(也被称为 *.vue 文件，英文 Single-File Components，缩写为 SFC)

以下三部分都写在一个文件里.
# 逻辑 (JavaScript)
<script>
export default {
  data() {
    return {
      count: 0
    }
  }
}
</script>

# 模板 (HTML)
<template>
  <button @click="count++">Count is: {{ count }}</button>
</template>

# 样式 (CSS)
<style scoped>
button {
  font-weight: bold;
}
</style>

```



```javascript
单文件组件的书写风格: 
Vue 的组件可以按两种不同的风格书写: 选项式 API 和 组合式 API.
实际上，选项式 API 是在组合式 API 的基础上实现的！
选项式 API 以“组件实例”的概念为中心 (即上述例子中的 this)

# 选项式 API (Options API)
用包含多个选项的对象来描述组件的逻辑, 例如 data、methods 和 mounted.
选项所定义的属性都会暴露在函数内部的 this 上, 它会指向当前的组件实例.
<script>
export default {		# 组件对象实例
  // data() 返回的属性将会成为响应式的状态
  // 并且暴露在 `this` 上
  data() {					# 选项, 本身也是对象
    return {
      count: 0			# 选项对象的属性 count, 暴露在 this 上, 也就是 this.count 访问
    }
  },

  // methods 是一些用来更改状态与触发更新的函数
  // 它们可以在模板中作为事件监听器绑定
  methods: {				# 选项
    increment() {		# 选项对象的方法
      this.count++
    }
  },

  // 生命周期钩子会在组件生命周期的各个不同阶段被调用
  // 例如这个函数就会在组件挂载完成后被调用
  mounted() {				# 选项
    console.log(`The initial count is ${this.count}.`)
  }
}
</script>

<template>
  <button @click="increment">Count is: {{ count }}</button>
</template>


# 组合式 API (Composition API)
使用导入的 API 函数来描述组件逻辑.
在单文件组件中, 组合式 API 通常会与 <script setup> 搭配使用.
这个 setup attribute 是一个标识，告诉 Vue 需要在编译时进行一些处理，让我们可以更简洁地使用组合式 API
比如，<script setup> 中的导入和顶层变量/函数都能够在模板中直接使用
<script setup>		# setup 属性是一个标识, 告诉 Vue 需要在编译时进行一些处理，让我们可以更简洁地使用组合式 API
import { ref, onMounted } from 'vue'		# 导入

// 响应式状态
const count = ref(0)										# 顶层变量 count 常量

// 用来修改状态、触发更新的函数
function increment() {									# 顶层函数 incrrement()
  count.value++
}

// 生命周期钩子
onMounted(() => {
  console.log(`The initial count is ${count.value}.`)
})
</script>

<template>															# 模版中直接使用 count
  <button @click="increment">Count is: {{ count }}</button>
</template>

```





## Overview

```javascript
# 本地搭建 Vue 单页应用, 包括用 Vite 构建 + Vue 的单文件组件 SFC
1. 已经安装了 Node.js
2. npm init vue@latest, 安装并执行 create-vue(Vue 官方的项目脚手架工具)
3. 在项目被创建后, 通过以下步骤安装依赖并启动开发服务器: cd <your-project-name; npm install; npm run dev
现在应该已经运行起来了你的第一个 Vue 项目！请注意，生成的项目中的示例组件使用的是组合式 API 和 <script setup>
4. 将应用发布到生产环境时, 请运行: npm run build
此命令会在 ./dist 文件夹中为你的应用创建一个生产环境的构建版本
部署: https://cn.vuejs.org/guide/best-practices/production-deployment.html
5. 借助 script 标签直接通过 CDN 来使用 Vue
<script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
通过 CDN 使用 Vue 时, 不涉及“构建步骤”. 这使得设置更加简单, 并且可以用于增强静态的 HTML 或与后端框架集成. 但是, 你将无法使用单文件组件 (SFC) 语法.
5.1. 使用 '全局构建版本' 的 Vue, 该版本的所有顶层 API 都以属性的形式暴露在了全局的 Vue 对象上.
<script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>			# 全局

<div id="app">{{ message }}</div>		# 模版

<script>		# 逻辑
  const { createApp } = Vue
  
  createApp({
    data() {
      return {
        message: 'Hello Vue!'
      }
    }
  }).mount('#app')
</script>
5.2. 使用 'ES 模块构建版本', ES 模块语法
<div id="app">{{ message }}</div>		# 模版

<script type="module">		# ES 模块
  import { createApp } from 'https://unpkg.com/vue@3/dist/vue.esm-browser.js'		# ES模块语法
  
  createApp({
    data() {
      return {
        message: 'Hello Vue!'
      }
    }
  }).mount('#app')
</script>
5.3. 启用 Import maps
上面的示例中, 我们使用了完整的 CDN URL 来导入, 但在文档的其余部分中，你将看到如下代码:
import { createApp } from 'vue'
使用 导入映射表 (Import Maps) 来告诉浏览器如何定位到导入的 vue
<script type="importmap">			# 导入映射表 (Import Maps)
  {
    "imports": {
      "vue": "https://unpkg.com/vue@3/dist/vue.esm-browser.js"
    }
  }
</script>

<div id="app">{{ message }}</div>

<script type="module">
  import { createApp } from 'vue'

  createApp({
    data() {
      return {
        message: 'Hello Vue!'
      }
    }
  }).mount('#app')
</script>

```





## FAQ & troubleshooting

### ES6 的模块化语法

#### 何谓模块

```javascript
模块是一段 JavaScript 代码, 这段代码会自动运行、而且是在严格模式下、并且无法退出运行.
```

ES6 引入了模块化, 其设计思想是在编译时就能确定模块的依赖关系, 以及输入和输出的变量
ES6 的模块化分为导出(export) @与导入(import)两个模块. 使用 `export` 关键字导出一个变量(或者类型)

#### 模块特点

- 模块内定义的变量不会被自动添加到全局作用域
- 由于上面的原因, 模块要向外面暴露一些自己的数据, 这些数据可以被外界访问到
- 一个模块可以从另外一个模块中导入数据(即可以使用其他模块的数据)
- 模块顶层的 `this` 是 `undefined`, 并不是 `window` 或  `global`
- ES6 的模块自动开启严格模式, 不管你有没有在模块头部加上 `use strict;`
- 模块中可以导入和导出各种类型的变量, 如函数, 对象, 字符串, 数字, 布尔值, 类等
-  每个模块都有自己的上下文, 每一个模块内声明的变量都是局部变量, 不会污染全局作用域
-  每一个模块只加载一次(是单例的), 若再去加载同目录下同文件, 直接从内存中读取

> 模块内作用域默认限制在模块内, 就好比是 php 的 class, 变量是属性, function 是方法, 模块顶层是 this 不是 window, 相当于 class 的 this/self, 为了模块可以对外被调用, 需要手动显式暴露, 同时也需要调用时, 手动显式导入, 这点和 python 很像.

#### 模块和脚本的区别

共同点:

- 都可以是单独文件
- 都可以被别人引用, 也可以引入别人

不同点:

- 可以按需导入模块中的数据, 但是不能按需导入脚本中的数据
- 脚本一旦引入, 所有数据均导入到全局作用域中, 而模块是可以按需选择控制的.

#### 一次性导入一个模块暴露出来的所有数据

可以用命名空间导入的方法, 即将一个模块暴露出来的所有的数据挂载到一个对象上，通过这个对象来调用这些数据.

```javascript
import * as aObj from './a.js';				// a 模块所有暴露出的数据都挂到了 aObj 上

// 通过 aObj 来访问和调用 a 模块中暴露出来的变量和方法
console.log(aObj.name);
aObj.say();
```

#### 使用 `as` 对导入和导出要暴露出去的数据重命名

```javascript
语法为: export { 原名称 as 新名称 }
export {say as speak};		// 导出/暴露 as
import { speak as howling} from './a.js';  		// 导入时重命名 as
```

#### 将一个值作为默认值暴露出去 `default`

将一个数据作为默认值暴露出去的3种语法:

1. 定义的时候就暴露
2. 先定义后暴露
3. 将这个值用 as 命名为 default 后暴露

```javascript
// 1. 定义的时候就暴露
export default function() {
    console.log('方法1暴露函数为默认值');
}

// 2. 先定义后暴露
let name = 'doug';
export default name;

// 3. 将这个值用 as 命名为 default 暴露
class Student{ // ... }
export {Student as default};
```

#### 导入其他模块暴露出的默认值

导入默认值的语法和导入非默认值的语法略有不同. 导入一个模块暴露出的默认值, 并不用 {} 来包裹, 而且不用使用 as 关键字就可以将这个默认值重命名: 例如导入上面例子中的 name， 并将其重命名为 familyName

```javascript
import familyName from './a.js';
```

#### 同时导入默认值和非默认值的语法

非默认值用`{}`括起来, 而默认值不用. 语法为:

```javascript
import 默认值 { 非默认值 } from './a.js';
```

#### 将从其他模块导入的数据立即直接暴露出去

从其他模块导入的某些数据再暴露出去, 这有两种语法:

```javascript
1. 先导入, 再暴露
import { name } from './a.js';   	// 导入
export { name };   			// 暴露

2. 一行代码之内同时导入和暴露, 也可以重命名后再暴露出去
export { name } from './a.js';
export { name as lastName } from './a.js';
```

#### export 导出

在`/src/compute.js`中定义如下内容, 并导出给其它模块使用:

```javascript
export var testNum1 = 6, testNum2 = 3;
export function add(a, b) {
    return a + b;
}
export function minus(a, b) {
    return a - b;
}
export function multiply(a, b) {
    return a * b;
}
export function divide(a, b) {
    return a / b;
}
```

也可以在最后一起导出, 如下写法和上面是等价的:

```javascript
var testNum1 = 6, testNum2 = 3;  
function add(a, b) {  
    return a + b;  
}  
function minus(a, b) {  
    return a - b;  
}  
function multiply(a, b) {  
    return a * b;  
}  
function divide(a, b) {  
    return a / b;  
}  
//这里导出一堆变量  
export {add, minus, multiply, divide, testNum1, testNum2}
```

也可以在导出时, 指定别名如下:

```javascript
var testNum1 = 6, testNum2 = 3;  
function jia(a, b) {  
    return a + b;  
}  
function jian(a, b) {  
    return a - b;  
}  
function cheng(a, b) {  
    return a * b;  
}  
function chu(a, b) {  
    return a / b;  
}  
//这里导出一堆变量  
export {jia as add, jian as minus, cheng as multiply, chu as divide, testNum1, testNum2}
```



#### import 导入

在`/src/main.js`中引入前面的导入:

```javascript
import {add, minus, multiply, divide, testNum1, testNum2} from './compute.js'  
var num1 = 10, num2 = 2;  
alert("testNum1: " + testNum1 + ", testNum2: " + testNum2 + " " + add(num1, num2) + " " + minus(num1, num2)+ " " + multiply(num1, num2)+ " " + divide(num1, num2));
```

导入的文件中, 也可以不加后缀, 如下写法是等价的:

```javascript
import {add, minus, multiply, divide, testNum1, testNum2} from './compute'  
var num1 = 10, num2 = 2;  
alert("testNum1: " + testNum1 + ", testNum2: " + testNum2 + " " + add(num1, num2) + " " + minus(num1, num2)+ " " + multiply(num1, num2)+ " " + divide(num1, num2));
```

也可以在导入时, 通过as改变变量的名字. 如下代码正常运行:

```javascript
import {add as jia, minus as jian, multiply as cheng, divide as chu, testNum1, testNum2} from './compute.js'  
var num1 = 10, num2 = 2;  
alert("testNum1: " + testNum1 + ", testNum2: " + testNum2 + " " + jia(num1, num2) + " " + jian(num1, num2)+ " " + cheng(num1, num2)+ " " + chu(num1, num2));
```

也可以采用整体加载导入的方式, 即用星号 (*) 指定一个对象, 所有输出值都加载在这个对象上面. 如下:

```javascript
import * as tmp from './compute.js'  
var num1 = 10, num2 = 2;  
alert("testNum1: " + tmp.testNum1 + ", testNum2: " + tmp.testNum2 + " " + tmp.add(num1, num2) + " " + tmp.minus(num1, num2)+ " " + tmp.multiply(num1, num2)+ " " + tmp.divide(num1, num2));
```



#### default 默认

- 在一个文件或模块中，export、import 可以有多个，export default 仅有一个。
- export default 中的 default 是对应的导出接口变量。
- 通过 export 方式导出，在导入时要加{ }，export default 则不需要。
- export default 向外暴露的成员，可以使用任意变量来接收。

```javascript
var a = "My name is Tom!";
export default a; // 仅有一个
export default var c = "error"; // error，default 已经是对应的导出变量，不能跟着变量声明语句 
import b from "./xxx.js"; // 不需要加{}， 使用任意变量接收
```

##### 2.2.1 导出

`/src/year.js`：

```javascript
export default {  
    year: 2022,  
    zodiac: 'tiger'  
}
```

##### 2.2.2 导入

`/src/main.js`：

```javascript
import temp from './year.js'//这里不用加大括号  
alert("year: " + temp.year + ", zodiac: " + temp.zodiac);
```

#### 只读属性说明

不允许在加载模块的脚本里面, 改写接口的引用指向, 即可以改写 import 变量类型为对象的属性值, 不能改写 import 变量类型为基本类型的值.

```javascript
import {a} from "./xxx.js"
a = {}; // error 
import {a} from "./xxx.js"
a.foo = "hello"; // a = { foo : 'hello' }
```

#### 模块路径

- 相对模块路径(路径以 `.` 开头, 例如: `./someFile` 或者 `../../someFolder/someFile` 等);
- 其他动态查找模块(如: `lodash`, `react` 或者甚至是 `react/core` 等).

它们的主要区别来自于系统如何解析模块.

##### 相对模块路径

这很简单, 仅仅是按照相对路径:

- 如果文件 `bar.js` 中含有 `import * as foo from './foo'`, `foo` 文件所存在的地方必须是相同文件夹下;
- 如果文件 `bar.js` 中含有 `import * as foo from '../foo'`, `foo` 文件所存在的地方必须是上一级目录;
- 如果文件 `bar.js` 中含有 `import * as foo from '../someFolder/foo'`, `foo` 文件所在的文件夹 `someFolder` 必须与 `bar.ts` 所在文件夹在相同的目录下.

或者, 你还可以想想其他相对路径导入的情景.

##### 动态查找

当导入路径不是相对路径时, 模块解析将会使用 [Node 模块解析策略](https://link.zhihu.com/?target=https%3A//nodejs.org/api/modules.html%23modules_all_together), 以下我将给出一个简单例子:

- 当你使用 `import * as foo from 'foo'`，将会按如下顺序查找模块:

- - `./node_modules/foo`
  - `../node_modules/foo`
  - `../../node_modules/foo`
  - 直到系统的根目录

- 当你使用 `import * as foo from 'something/foo'`，将会按照如下顺序查找内容

- - `./node_modules/something/foo`
  - `../node_modules/something/foo`
  - `../../node_modules/something/foo`
  - 直到系统的根目录



### module, export, import

```javascript
module 模块
export 规定模块的对外接口
import 导入其他模块成员
🌟 没有暴露|导出的数据外部是访问不到的
🌟 导入的属性是不能被赋值的, 需要按访问器模式来使用, 即用自定义set方法来修改, 然后用get()方法来获取
🌟 只读属性说明: 不能改写 import 引入进来的数据(称为接口)的原类型和值, 只能修改引入数据(称为接口)的类型为对象{}类型的值 
🌟 export 和 import 一样也可以有 from, 表示直接在导入时直接紧接着就暴露了
🌟 导入语句可以不加后缀(.js). import * as aObj from './a.js'; 等同于 import * as aObj from './a';

模块的导入和导出的三个层次:
1. 默认导出和导入, 只能是一个数据(变量或者函数)
2. 按需指定导出和导入
3. 直接导入模块并执行模块中的额代码

1.1. 将一个数据作为默认值暴露出去的3种语法:
     // 1.1.1. 定义的时候就暴露
     export default function() {
     	 console.log('方法1暴露函数为默认值');
     }

     // 1.1.2. 先定义后暴露
     let name = 'doug';
     export default name;

     // 1.1.3. 将这个值用 as 命名为 default 暴露
     class Student{ // ... }
     export {Student as default};
1.2. 默认导出 export default, 定义在封装模块中.
		 一个模块定义时只能定义一次 export default 默认导出
		 格式: export default 默认导出的成员
		 // export-default.js
		 export default function () {		// 定义的同时暴露数据
		   console.log('foo');
		 }
		 定义了一个匿名导出函数(对象)
1.3. 默认导入 import, 定义在调用模块中.
		 导入一个模块暴露出的默认值, 并不用 {} 来包裹, 而且不用使用 as 关键字就可以将这个默认值重命名
		 格式: import 默认值 from './a.js';
		 格式: import 默认值 { 非默认值 } from './a.js';			// 同时导入默认值和非默认值
		 格式: import 任意名称 from '模块标识符(模块的路径)'
		 import yocc from './export-default'		// 用任意名字接收默认导出, 用 yocc 接收默认导出的匿名成员
		 yocc(); 	// 'foo', 调用的结果
1.4. 默认导入/导出特点:
		 在一个文件或模块中, export、import 可以有多个, export default 仅有一个
		 export default 中的 default 是对应的导出接口变量
		 通过 export 方式导出, 在导入时要加{ }, export default 则不需要
		 export default 向外暴露的成员, 可以使用任意变量来接收
		 
2.1. 导出指定的成员, 定义在封装的模块中.
		 格式: export { 原名称 }		 
		 格式: export { 原名称 as 新名称 }
		 // 定义在模块里, math.js
		 var basicNum = 0;
		 var add = function (a, b) {
		   return a + b;
		 };
		 export {basic};		// 可以多次导出, 不同于默认导出只能仅一次. 也可 export {basic, add};
		 export {add};			// 两个过程, 先定义、再暴露, 可以合成一个过程, 在定义变量的同时暴露数据, 只要在声明符之前加上 export 就可以, 类似默认导出 export default function () {} 
2.2. 导入指定的成员, 定义在调用的模块中.
		 格式: import { 数据1, 数据2, ... , 数据n } from 模块名
		 格式: import { 原名称 as 新名称 }
		 格式: import * as aObj from './a.js'; 		// a 模块所有暴露出的数据都挂到了 aObj 上
		 格式: import * as aObj from './a';
		 // 调用|引用模块 math.js
		 import {basicNum, add} from './math';		// 接收导入时名字必须和导出时一致, 可以使用 as 关键字进行重命名
		 function test (ele) {
		   ele.textContent = add(99 + basicNum);
		 }

3.1. 直接导入模块并执行此模块
		 只是单纯的执行某个模块中的代码, 并不需要得到模块中向外共享的成员
		 import '../request.js'
		 
```



## See Also

https://www.cnblogs.com/nxmxl/p/14900814.html 	ES6的模块化语法

https://www.jianshu.com/p/d69bfe9b38b6		ES6模块化语法简介

https://zhuanlan.zhihu.com/p/70094674 		模块路径
