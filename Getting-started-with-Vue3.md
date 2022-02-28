# Vue3 入门

## 编辑器

编辑器采用 Visual Studio Code，简称为 VS Code

开发 Vue3 需要的插件：

- ESLint：JS 校验
- Prettier：代码格式化
- Vue Language Features (Volar)：Vue3 语法高亮等
- TypeScript Vue Plugin (Volar)：使用 TypeScript 时需要
- Markdown Preview Enhanced：Markdown 预览，根据需要安装

VS Code 配置：通过图形界面进行配置，特殊配置手动更改配置文件，配置文件存放位置`~/Library/Application Support/Code/User/settings.json`

配置文件内容参考：

```json
{
  "workbench.startupEditor": "none",
  "editor.fontSize": 18,
  "files.autoSave": "onFocusChange",
  "editor.wordWrap": "on",
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.formatOnSave": true,
  "editor.formatOnType": true,
  "editor.minimap.enabled": false,
  "terminal.integrated.fontSize": 18,
  "markdown.preview.fontSize": 18,
  "markdown-preview-enhanced.previewTheme": "solarized-dark.css"
}
```

可选配置：

```json
{
  "security.workspace.trust.untrustedFiles": "newWindow",
  "liveServer.settings.donotShowInfoMsg": true,
  "editor.linkedEditing": true,
  "explorer.confirmDelete": false,
  "editor.tabSize": 2,
  "files.associations": {
    "*.cjson": "jsonc",
    "*.wxss": "css",
    "*.wxs": "javascript",
    "*.wxml": "html"
  },
  "emmet.includeLanguages": {
    "wxml": "html"
  },
  "minapp-vscode.disableAutoConfig": true,
  "eslint.nodePath": "/Users/whb/.nvm/versions/node/v16.13.0/lib/node_modules",
  "javascript.updateImportsOnFileMove.enabled": "never",
  "editor.unicodeHighlight.includeComments": false
}
```

## 浏览器

浏览器采用 Chrome，并安装`Vue DevTools`插件

## 创建项目

命令：

- `npm init vue@latest`
- 选项：
  ✔ Project name: … vue3
  ✔ Add TypeScript? … No
  ✔ Add JSX Support? … No
  ✔ Add Vue Router for Single Page Application development? … Yes
  ✔ Add Pinia for state management? … Yes
  ✔ Add Vitest for Unit Testing? … No
  ✔ Add Cypress for both Unit and End-to-End testing? … No
  ✔ Add ESLint for code quality? … Yes
  ✔ Add Prettier for code formatting? … Yes

## 配置项目

格式化设置：/.prettierrc

```json
{
  "singleQuote": false,
  "semi": true
}
```

/.eslintrc.cjs：
`"plugin:vue/vue3-essential",` 改为 `"plugin:vue/vue3-strongly-recommended",`

## 开发规范

开发在`src`文件中，规范为：

- 目录：
  - 命名：单词小写，多个单词使用`-`分隔
  - 文件夹：
    - api：数据请求
    - assets：静态资源
    - components：公用组件
    - router：路由设置
    - store：状态管理
    - views：页面文件（组件）
    - utils：工具文件
- 文件：
  - 使用 ESLint、Prettier 格式化代码
- 组件：
  - 命名：不能使用一个单词，至少由两个单词组成，一个单词时单词前加`The`组成两个单词
    - 页面文件：一个单词时，单词末尾加`View`组成两个单词
  - 引用：模板中使用所有单词首字母大写的方式`<TheWelcome />`

单文件组件（SFC）：

```html
<script setup></script>

<template></template>

<style scoped lang="scss"></style>
```

## 代码参考

路由：router/index.js

```js
import { createRouter, createWebHashHistory } from "vue-router";
import HomeView from "../views/HomeView.vue";

const router = createRouter({
  history: createWebHashHistory(import.meta.env.BASE_URL),
  routes: [
    {
      path: "/",
      name: "home",
      component: HomeView,
    },
    {
      path: "/about",
      name: "about",
      // route level code-splitting
      // this generates a separate chunk (About.[hash].js) for this route
      // which is lazy-loaded when the route is visited.
      component: () => import("../views/AboutView.vue"),
    },
  ],
});

export default router;
```

状态管理：store/counter.js

```js
import { defineStore } from "pinia";

export const useCounterStore = defineStore({
  id: "counter",
  state: () => ({
    counter: 0,
  }),
  getters: {
    doubleCount: (state) => state.counter * 2,
  },
  actions: {
    increment() {
      this.counter++;
    },
  },
});
```

入口文件：/src/main.js

```js
import { createApp } from "vue";
import { createPinia } from "pinia";

import App from "./App.vue";
import router from "./router";

const app = createApp(App);

app.use(createPinia());
app.use(router);

app.mount("#app");
```

## Elementu Plus

https://element-plus.gitee.io/zh-CN/

安装：`npm install element-plus --save`

按需导入：

- 安装插件：`npm install -D unplugin-vue-components unplugin-auto-import`
- 配置 vite.config.js

vite.config.js：

```js
import AutoImport from "unplugin-auto-import/vite";
import Components from "unplugin-vue-components/vite";
import { ElementPlusResolver } from "unplugin-vue-components/resolvers";

export default {
  plugins: [
    // ...
    AutoImport({
      resolvers: [ElementPlusResolver()],
    }),
    Components({
      resolvers: [ElementPlusResolver()],
    }),
  ],
};
```

## 网络请求
