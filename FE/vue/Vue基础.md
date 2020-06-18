## 1. Vue语法

### 1.1  插值表达式

- 语法

  ```html
  <div id="app">
      <div>{{msg}}</div>
      <div>{{1 + 2}}</div>
      <div>{{msg + '----' + 123}}</div>
  </div>
  <script src="js/vue.js"></script>
  <script>
      var vm = new Vue({
          el: '#app',
          data: {
              msg: 'hello vue'
          }
      })
  </script>
  ```

### 1.2 指令

- 指令本质：自定义属性

- `v-cloak`

  - 作用：解决插值表达式的闪动问题

  - 使用

    1. 提供样式

       ```css
       [v-cloak] {
           display: none;
       }
       ```

    2. 在插值表达式所在的标签中添加`v-cloak`指令

       ```html
       <div v-cloak>{{msg}}</div>
       ```

  - 原理：先通过样式隐藏内容，然后在内存中进行值的替换，之后显示最终结果

- `v-text`、`v-html`、`v-pre` 数据绑定指令

  - `v-text`：填充纯文本，比插值表达式更加简洁，无闪动问题
  - `v-html`：填充HTML片段，存在安全问题（本网站内部数据可以使用，第三方数据不可用）
  - `v-pre`：填充原始信息

  - 使用：

    ```html
    <div id="app">
        <div v-text='msg'></div>
        <div v-html='html1'></div>
        <div v-pre>{{msg}}</div>
    </div>
    <script src="js/vue.js"></script>
    <script>
        var vm = new Vue({
            el: '#app',
            data: {
                msg: 'hello vue',
                html1: '<h1>Hello</h1>'
            }
        })
    </script>
    ```

- 数据响应式

  - 数据的响应式：数据的变化导致页面内容的变化（不刷新页面的情况下）

  - 数据绑定：将数据填充到标签中
  - `v-once` 只编译一次，显示内容后不再具有数据响应式功能（可以提高性能）

  - 使用：

    ```html
    <div v-once>{{info}}</div>
    ```

- `v-model` 双向数据绑定

  ```html
  <input type="text" v-model="msg">
  ```

### 1.3 事件绑定

- `v-on`指令绑定事件

  ```html
  <div id="app">
      <div v-text="msg"></div>
      <!-- 绑定点击事件：执行简单逻辑 -->
      <input type="button" value="msg+2" v-on:click='msg += 2'>
      <input type="button" value="msg+1" @click='msg += 1'>
      <!-- 单击绑定事件：执行函数 -->
      <input type="button" value="bindMethodName" @click='say'>
      <input type="button" value="callMethodsay" @click='say()'>
      <input type="button" value="callMethodsay2" @click='say2("say2")'>
      <input type="button" value="callMethodsay3" @click='say3("say3", $event)'>
      <div @click="say2('a')">
          <!--阻止冒泡：上面的say2('a')不执行-->
          <a href="#" @click.stop="say()">阻止冒泡</a>
      </div>
      <!-- 阻止默认行为：页面不跳转到www.baidu.com -->
      <a href="www.baidu.com" @click.prevent="say()">阻止默认行为</a>
      <!-- 同时阻止冒泡和默认行为 -->
      <a href="www.baidu.com" @click.stop.prevent="say()">阻止冒泡、默认行为</a>
      <!-- 按键修饰符事件：输入用户名时按delete键时全部清空，输入密码后按enter键提交表单等 -->
      <input type="text" name="username" id="" @keyup.delete="say()">
      <input type="password" name="password" id="" @keyup.enter="say()">
      <!-- 自定义按键修饰符：定义如下：Vue.config.keyCodes.aaa = 65; 则可以使用aaa代替按键a
  按键的值可以通过event.keyCode获取-->
      <input type="text" @keyup.aaa="say()">
  </div>
  <script src="js/vue.js"></script>
  <script>
      // 绑定按键别名
      Vue.config.keyCodes.aaa = 65;
      var vm = new Vue({
          el: '#app',
          data: {
              msg: 'hello vue',
              html1: '<h1>Hello</h1>'
          },
          methods: {
              say: function() {
                  this.msg += 3;
              },
              say2: function(info) {
                  this.msg += info;
              },
              say3: function(info, event) {
                  this.msg += "--------" + info;
                  this.msg += "--------" + event.target.tagName;
                  this.msg += "--------" + event.target.innerHtml;
              }
          }
      })
  </script>
  ```

### 1.4 属性绑定

- `v-bind`

  ```html
  <div id="app">
      <div v-text="url"></div>
      <a v-bind:href="url">百度</a>
      <a :href="url">百度</a>
  </div>
  <script src="js/vue.js"></script>
  <script>
      var vm = new Vue({
          el: '#app',
          data: {
              url: 'http://www.baidu.com'
          }
      })
  </script>
  ```

### 1.5 样式绑定

- 语法

  ```html
  <div id="app">
      <!-- 对象语法 isActive为data中数据值为true/false -->
      <div v-bind:class="{active: isActive}"></div>
      <!-- 数组语法 -->
      <div v-bind:class="[activeClass, errorClass]"></div>
      <!-- 对象+数组组合 -->
      <div v-bind:class="[activeClass, errorClass, {active: isActive}]"></div>
      <!-- 简化写法 (推荐) -->
      <div v-bind:class="arrClasses"></div>
      <div v-bind:class="objClasses"></div>
      <!-- 默认的class会保留 -->
      <div class="aaa" v-bind:class="arrClasses"></div>
      <!-- 通过style绑定样式 -->
      <div v-bind:style="objStyle"></div>
  </div>
  <script src="js/vue.js"></script>
  <script>
      var vm = new Vue({
          el: '#app',
          data: {
              isActive: 'true',
              activeClass: 'active',
              errorClass: 'error',
              // 将class属性集中到data中，通过数组相关API操作class
              arrClasses: [active, error],
              objClasses: {
                  active: true,
                  error: true
              },
              objStyle: {
                  width: '20px'
              }
  
          }
      })
  </script>
  ```

  

### 1.6 分支循环结构

- 分支结构

  ```html
  <div id="app">
      <!-- v-if -->
      <div v-if='score >= 90'>优秀</div>
      <div v-else-if='score < 90 && score >= 60'>良好</div>
      <div v-else>差</div>
      <!-- v-show 页面中已经渲染，只是添加了display:none  而v-if则不满足条件不渲染 -->
      <div v-show="show">show</div>
  </div>
  <script src="js/vue.js"></script>
  <script>
      var vm = new Vue({
          el: '#app',
          data: {
              score: 90,
              show: false
          }
      })
  </script>
  ```

- 循环结构

  ```html
  <div id="app">
      <ul>
          <!-- 遍历数组 -->
          <li v-for="item in fruits">{{item}}</li>
          <li v-for="(item, index) in fruits">{{item}} + {{index}}</li>
          <!-- 遍历对象 -->
          <li v-for='(value, key, index) in obj'>{{value + "---" + key + "---" + index}}</li>
          <!-- 为了提高vue的性能添加key -->
          <li :key='item.id' v-for="item in fruits">{{item}}</li>
          <!-- v-for与v-if结合使用 -->
          <li v-if='key != "username"' v-for='(value, key, index) in obj'>{{value + "---" + key + "---" + index}}</li>
      </ul>
  </div>
  <script src="js/vue.js"></script>
  <script>
      var vm = new Vue({
          el: '#app',
          data: {
              fruits: ['apple', 'orange', 'banana'],
              obj: {
                  username: 'zhangsan',
                  password: '123456'
              }
          }
      })
  </script>
  ```

## 2. Vue常用特性

### 2.1 表单操作

- 表单处理

  > 1. 通过v-model实现表单双向数据绑定
  > 2. 对于多选和下拉多行框数据用数组
  > 3. 表单提交时用 `@click.prevent`阻止无默认值提交

  ```html
  <div id="app">
      <div class="form">
          <form action="http://www.baidu.com" method="post">
              <span>用户名： </span>
              <input type="text" name="username" id="" v-model="username"><br>
              <span>性别： </span>
              <span>男： </span>
              <input type="radio" name="gender" id="male" value="1" v-model="gender">
              <span>女： </span>
              <input type="radio" name="gender" id="female" value="2" v-model="gender"><br>
              <span>爱好： </span>
              <span>篮球： </span>
              <input type="checkbox" name="hoby" id="basketball" value="1" v-model="hoby">
              <span>足球： </span>
              <input type="checkbox" name="hoby" id="football" value="2" v-model="hoby"><br>
              <span>职业： </span>
              <!-- multiple指定下拉框多选 -->
              <select name="work" id="work" v-model="occupation" multiple>
                  <option value="0">请选择职业</option>
                  <option value="1">程序员</option>
                  <option value="2">测试员</option>
              </select><br>
              <span>个人简介</span>
              <textarea name="desc" id="desc" cols="10" rows="3" v-model="desc"></textarea><br>
              <input type="submit" value="提交" @click.prevent='handle'>
          </form>
      </div>
  </div>
  <script src="js/vue.js"></script>
  <script>
      var vm = new Vue({
          el: '#app',
          data: {
              username: '',
              gender: '1',
              hoby: ['2'],
              occupation: [],
              desc: ''
          },
          methods: {
              handle: function() {
                  console.log(this.username)
                  console.log(this.gender)
                  console.log(this.hoby)
                  console.log(this.occupation)
                  console.log(this.desc)
              }
          }
      })
  </script>
  ```

- 表单域修饰符

  ```html
  <!-- number age直接可以参数算术运算 不再用parseInt(age)转换 -->
  <input type="number" v-model.number="age">
  <!-- trim 去掉开始和结尾空格 -->
  <input type="text" v-model.trim="username">
  <!-- lazy 将input事件切换为change事件 (input输入内容即触发事件，change失去焦点时才触发事件)-->
  <input type="text" v-model.lazy="msg">
  ```

### 2.2 自定义指令

- 无参数的自定义指令

  ```html
  <div id="app">
      <div class="form">
          <input type="text" v-focus>
      </div>
  </div>
  <script src="js/vue.js"></script>
  <script>
      // 自定义指令 参数1focus为指令名称，参数2实现指令的逻辑
      Vue.directive('focus', {
          inserted: function(el) {
              // inserted为触发的勾子函数 el表示所绑定的元素
              el.focus();
          }
      })
  </script>
  ```

- 有参数的自定义指令

  ```html
  <div id="app">
      <div class="form">
          <input type="text" v-color="msg">
      </div>
  </div>
  <script src="js/vue.js"></script>
  <script>
      // 自定义带参数的指令
      Vue.directive('color', {
          inserted: function(el, binding) {
              //el为绑定的元素，binding为一个对象内容可以打印查看
              console.log(binding)
              el.style.backgroundColor = binding.value;
          }
      })
      var vm = new Vue({
          el: '#app',
          data: {
              msg: "red"
          }
      })
  </script>
  ```

- 自定义局部指令（仅组件中可使用）

  ```html
  <script>
      var vm = new Vue({
          el: '#app',
          data: {
              msg: "red"
          },
          // 局部指令
          directive: {
              color: {
                  //指令定义
                  inserted: function(el, binding) {}
              }
          })
      })
  </script>
  ```

  

### 2.3 计算属性

- 语法

  > 函数亦可实现计算功能，同函数的区别：computed有缓存功能，同一个结果不会计算多次

  ```html
  <div id="app">
      <div>{{reversedMessage}}</div>
  </div>
  <script src="js/vue.js"></script>
  <script>
      var vm = new Vue({
          el: '#app',
          data: {
              msg: "hello vue"
          },
          methods: {},
          computed: {
              reversedMessage: function () {
                  return this.msg.split(' ').reverse().join('')
              }
          }
      })
  </script>
  ```

### 2.4 侦听器

- 数据一旦发生变化就通知侦听器所绑定方法

  > 场景：数据变化时执行异步或开销较大的操作，如用户名input失去焦点(v-model.lazy)时验证用户名是否存在

  ```html
  <div id="app">
      lastname: <input type="text" v-model="lastName">
      firstname: <input type="text" name="" id="" v-model="firstName">
      <div>{{fullName}}</div>
  </div>
  <script src="js/vue.js"></script>
  <script>
      var vm = new Vue({
          el: '#app',
          data: {
              firstName: "Jim",
              lastName: "Green",
              fullName: "Jim Green"
          },
          // 侦听器
          watch: {
              firstName: function (val) {
                  this.fullName = val + ' ' + this.lastName;
              },
              lastName: function (val) {
                  this.fullName = this.firstName + ' ' + val;
              }
          }
      })
  </script>
  ```

### 2.5 过滤器

- 自定义过滤器

  ```html
  <div id="app">
      <!--场景1:数据展示-->
      <div>{{msg | upper}}</div>
      <!--场景2：属性绑定-->
      <div :id="msg | upper"></div>
      <!--过滤器携带参数-->
      <div :id="msg | format('yyyy-MM-dd')"></div>
  </div>
  <script src="js/vue.js"></script>
  <script>
      // 定义全局过滤器
      Vue.filter('upper', function (value) {
          // 过滤器业务逻辑
          return value.charAt(0).toUpperCase() + value.slice(1);
      });
      var vm = new Vue({
          el: '#app',
          data: {
              msg: "abc",
          },
          // 定义局部过滤器
          filters: {
              format: function (value, arg1) {
                  return //自行百度日期格式化过滤逻辑
              }
          }
      })
  </script>
  ```

### 2.6 生命周期

- 挂载
  1. beforeCreate
  2. created
  3. beforeMount
  4. **mounted**
- 更新
  1. beforeUpdate
  2. updated
- 销毁
  1. beforeDestroy
  2. destroy

### 2.7 数组相关API

![image-20200219082038335](D:\note\FE\vue\media\image-20200219082038335.png)

![image-20200219082126381](D:\note\FE\vue\media\image-20200219082126381.png)









































