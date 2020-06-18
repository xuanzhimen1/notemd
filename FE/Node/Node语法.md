## 1. Node开发概述

- 什么是Node
  
- Node就是JavaScript代码的运行环境（好比Chrome V8是JavaScript的运行环境）
  
- Node.js环境安装

  - Ubuntu的安装
    1. 官网下载最新的node.js对应操作系统的安装包
    2. `curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -`   注：setup_12.x为对应版本，如果下载的是13.x的版本则对应改为setup_13.x
    3. `sudo apt-get install -y nodejs` 安装nodejs

  - windows的安装

    1. 下载最新node.js对应的windows版本

    2. 一路下一步安装完成后win+r => powershell 键入node -v  出现版本号即成功

    3. 安装失败解决

       - 错误代码2502、2503

         原因：系统帐户权限不足： 以管理员身份运行powershell => 输入运行安装包命令：msiexec /package node安装包位置

       - node -v出现错误

         原因：Node安装目录写入环境变量失败：将Node安装目录添加到环境变量中即可（在系统变量path中添加nodejs的安装路径即可）

- Node运行js文件
  
  - node xxx.js

## 2. Node语法

### 2.1 Node的模块化开发

- JavaScript存在的问题
  1. 文件依赖  不明确
  2. 命名冲突  多文件共用变量

- Node.js中模块化开发规范
  - Node.js规定一个javaScript文件就是一个模块，模块内部定义的变量和函数默认不对外部开放
  - 模块内部可以使用**exports**对象进行成员导出，使用**require**方法导入其它模块











































































