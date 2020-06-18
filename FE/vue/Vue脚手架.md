## 1. 基本用法

- 官方 https://cli.vuejs.org/zh/

- 安装

  1. 安装3.x版本的Vue脚手架

     ```bash
     npm install -g @vue/cli
     ```

  2. 检测是否安装完成
  
     ```bash
     vue -V
     ```

- 用脚手架安装vue项目

  - 基于图形化界面的方式创建vue项目

    ```bash
    vue ui
    ```

  - 选择创建=>选择路径=>选择在此创建新项目
  
    ![1582256630819](C:\Users\zhangjinqiang\Desktop\note\FE\vue\media\1582256630819.png)
  
  - 详情中  包管理器选择默认, 可以选择初始化Git
  - 预设中  选择手动
  - 功能中  选择 开启Babel Router Linter/Formatter 使用配置文件
  - 配置中  选择 Pick a linter/formatter config 选择 ESlint+Standard config

## 2. 脚手架的自定义配置

- 通过package.json配置项目(不推荐, 因为package.json主要用来管理包的配置信息)

  ```json
  "vue": {
      "devServer": {
          "prot": "8888",
          // 自动打开浏览器
          "open": true
      }
  }
  ```

- 将配置信息单独定义到vue.config.js配置文件中

  1. 在项目跟目录中创建vue.config.js

  2. 在该文件中配置从而覆盖默认配置

     ```js
     // vue.config.js
     module.exports = {
         "devServer": {
             "prot": "8888",
             // 自动打开浏览器
             "open": true
         }
     }
     ```

     