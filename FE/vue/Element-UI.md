## 1. Element-UI概述

- 概述: Element-UI是基于Vue2.0的桌面端组件库
- 官网: http://element-cn.eleme.io/#/zh-CN

## 2. 安装与使用

- 安装

  ```bash
  npm i element-ui -S
  ```

- 导入Element-UI资源

  ```js
  // 导入组件库
  import ElementUI from 'element-ui';
  // 导入组件相关样式
  import 'element-ui/lib/theme-chalk/index.css'
  // 配置Vue插件
  Vue.use(ElementUI)
  ```

  