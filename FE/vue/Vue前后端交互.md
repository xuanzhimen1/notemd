## 1. 前后端交互模式

- URL地址格式

  - 传统的URL

    schema://host:port/path?query#fragment

    协议 主机 端口 路径 查询参数 锚点

  - Restful形式的URL

    GET POST PUT DELETE

    http://www.hello.com/books  GET

    http://www.hello.com/books  POST

    http://www.hello.com/books/123  PUT  

    http://www.hello.com/books/123  DELETE

## 2. Promise用法

- Promise概述：异步编程的一种解决方案

- 基本用法

  1. 实例化Promise对象，构造函数中传递函数， 该函数中用于处理异步任务
  2. resolve和reject两个参数用于处理成功和失败的两种情况，并通过p.then获取处理结果

  ```js
  var p = new Promise(function (resolve, reject) {
      // 成功调用resolve()
      var flag = true;
      if (flag) {
          resolve("OK");
      }
      // 失败调用reject()
      else {
          reject("ERROR")
      }
  });
  // 从resolve或reject中获取处理结果，分别对应data与info
  p.then(function (data) {
      // 从resolve中得到结果
      console.log(data)
  }), function (info) {
      // 从reject中得到结果
      console.log(info)
  }
  ```

  































