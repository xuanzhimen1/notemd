## 1. 组件注册

- 注册

  > 注意：
  >
  > 1. 组件参数的data值必须是函数
  > 2. 组件模板必须是单个跟元素 如错误定义：`template: '<button></button><br>'`
  > 3. 组件模板的内容可以是模板字符串 使用`键 引起template中的内容  提高代码的可读性
  > 4. 组件命名：如果使用驼峰式命名组件，在字符串模板中可用驼峰的方式使用组件，在普通的标签模板中，必须使用短横线的方式使用组件 如 组件名称`HelloWorld`，则字符串模板中可能`<HelloWorld>`调用，而在普通标签模板中要使用`<hello-world>`调用

  ```html
  <div id="app">
      <button-counter></button-counter>
      <button-counter></button-counter>
      <button-counter></button-counter>
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">
      // 组件注册
      Vue.component('button-counter', {
          data: function () {
              return {
                  count: 0
              }
          },
          template: '<button @click="handle">点击了{{count}}次</button>',
          methods: {
              handle: function () {
                  this.count += 2;
              }
          }
      })
      var vm = new Vue({
          el: '#app',
          data: {}
      });
  </script>
  ```

- 局部组件注册

  > 局部组件只能在注册他的父组件中使用

  ```html
  <script type="text/javascript">
      // 局部组件的定义
      var HelloWorld = {
          data: function () {
              return {
                  msg: 'HelloWorld'
              }
          },
          template: '<div>{{msg}}</div>'
      };
      var vm = new Vue({
          el: '#app',
          data: {},
          components: {
              'hello-world': HelloWorld
          }
      });
  </script>
  ```

## 2. Vue调试工具

- 下载

  https://cn.vuejs.org/  => 生态系统 => 工具 => Devtools

- 安装

  在Readme.md中点击  [Get the Chrome Extension](https://chrome.google.com/webstore/detail/vuejs-devtools/nhdogjmejiglipccpnnnanhbledajbpd) / ([beta channel](https://chrome.google.com/webstore/detail/vuejs-devtools/ljjemllljcmogpfapbkkighbhhppjdbg)) 安装

- 使用

  f12在调试面板中使用

## 3. 组件间数据交互

- 父组件向子组件传值

  - 组件中通过props接收传递过来的值  `props: ['title', 'content'],`

  - 父组件通过属性将值传递给子组件  `<menu-item :title='ptitle' content='hello'>`

  - 语法

    > props属性名规则：尽量都使用 menu-item 这种形式，避免作用menuItem驼峰式
    >
    > - 在props中使用驼峰形式，模板中需要使用短横线的形式
    >
    > - 字符串形式的格析中没有这个限制
    >
    >   ![image-20200219104430498](D:\note\FE\vue\media\image-20200219104430498.png)

    ```html
    <div id="app">
        <div>{{pmsg}}</div>
        <menu-item title='来自父组件的值'></menu-item>
        <menu-item :title='ptitle' content='hello'></menu-item>
    </div>
    <script type="text/javascript" src="js/vue.js"></script>
    <script type="text/javascript">
        /*
          父组件向子组件传值-基本使用
        */
        Vue.component('menu-item', {
            props: ['title', 'content'],
            data: function() {
                return {
                    msg: '子组件本身的数据'
                }
            },
            template: '<div>{{msg + "----" + title + "-----" + content}}</div>'
        });
        var vm = new Vue({
            el: '#app',
            data: {
                pmsg: '父组件中内容',
                ptitle: '动态绑定属性'
            }
        });
    </script>
    ```

- 子组件向父组件传值

  - props传递数据原则：单向数据流（不推荐在子组件中修改props值）

  - 子组件通过自定义事件向父组件传递信息

  - 父组件监听子组件的事件

  - 语法

    ```html
    <div id="app">
        <div :style='{fontSize: fontSize + "px"}'>{{pmsg}}</div>
        <menu-item :parr='parr' @enlarge-text='handle($event)'></menu-item>
    </div>
    <script type="text/javascript" src="js/vue.js"></script>
    <script type="text/javascript">
        // 子组件向父组件传值-携带参数
        Vue.component('menu-item', {
            props: ['parr'],
            template: `
            <div>
              <ul>
                <li :key='index' v-for='(item,index) in parr'>{{item}}</li>
              </ul>
              <button @click='$emit("enlarge-text", 5)'>扩大父组件中字体大小</button>
              <button @click='$emit("enlarge-text", 10)'>扩大父组件中字体大小</button>
            </div>
          `
        });
        var vm = new Vue({
            el: '#app',
            data: {
                pmsg: '父组件中内容',
                parr: ['apple','orange','banana'],
                fontSize: 10
            },
            methods: {
                handle: function(val){
                    // 扩大字体大小
                    this.fontSize += val;
                }
            }
        });
    </script>
    ```

- 同级组件传值

  - 单独的事件中心管理组件间的通信 `var eventHub = new Vue()`

  - 监听事件与销毁事件 `eventHub.$on('add-todo', addTodo)` `eventHub.$off('add-todo')`

  - 触发事件`eventHub.$emit('add-todo', id)`

  - 语法

    ```html
    <div id="app">
        <div>父组件</div>
        <div>
            <button @click='handle'>销毁事件</button>
        </div>
        <test-tom></test-tom>
        <test-jerry></test-jerry>
    </div>
    <script type="text/javascript" src="js/vue.js"></script>
    <script type="text/javascript">
        /*
          兄弟组件之间数据传递
        */
        // 提供事件中心
        var hub = new Vue();
    
        Vue.component('test-tom', {
            data: function () {
                return {
                    num: 0
                }
            },
            template: `
            <div>
              <div>TOM:{{num}}</div>
              <div>
                <button @click='handle'>点击</button>
              </div>
            </div>
          `,
            methods: {
                handle: function () {
                    hub.$emit('jerry-event', 2);
                }
            },
            mounted: function () {
                // 监听事件
                hub.$on('tom-event', (val) => {
                    this.num += val;
                });
            }
        });
        Vue.component('test-jerry', {
            data: function () {
                return {
                    num: 0
                }
            },
            template: `
            <div>
              <div>JERRY:{{num}}</div>
              <div>
                <button @click='handle'>点击</button>
              </div>
            </div>
          `,
            methods: {
                handle: function () {
                    // 触发兄弟组件的事件
                    hub.$emit('tom-event', 1);
                }
            },
            mounted: function () {
                // 监听事件
                hub.$on('jerry-event', (val) => {
                    this.num += val;
                });
            }
        });
        var vm = new Vue({
            el: '#app',
            data: {},
            methods: {
                handle: function () {
                    hub.$off('tom-event');
                    hub.$off('jerry-event');
                }
            }
        });
    </script>
    ```

## 4. 组件插槽

- 组件插槽

  > `<slot></slot>`为vue的api固定不变

  ```html
  <div id="app">
      <alert-box>有bug发生</alert-box>
      <alert-box>有一个警告</alert-box>
      <alert-box></alert-box>
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">
      /*
        组件插槽：父组件向子组件传递内容
      */
      Vue.component('alert-box', {
          template: `
          <div>
            <strong>ERROR:</strong>
            <slot>默认内容</slot>
          </div>
        `
      });
      var vm = new Vue({
          el: '#app',
          data: {}
      });
  </script>
  ```

- 具名插槽

  ```html
  <div id="app">
      <base-layout>
          <p slot='header'>标题信息</p>
          <p>主要内容1</p>
          <p>主要内容2</p>
          <p slot='footer'>底部信息信息</p>
      </base-layout>
      <!--方法二：template名称固定-->
      <base-layout>
          <template slot='header'>
              <p>标题信息1</p>
              <p>标题信息2</p>
          </template>
          <p>主要内容1</p>
          <p>主要内容2</p>
          <template slot='footer'>
              <p>底部信息信息1</p>
              <p>底部信息信息2</p>
          </template>
      </base-layout>
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">
      /*
        具名插槽
      */
      Vue.component('base-layout', {
          template: `
          <div>
            <header>
              <slot name='header'></slot>
            </header>
            <main>
              <slot></slot>
            </main>
            <footer>
              <slot name='footer'></slot>
            </footer>
          </div>
        `
      });
      var vm = new Vue({
          el: '#app',
          data: {}
      });
  </script>
  ```

- 作用域插槽

  - 应用：父组件对子组件的内容进行加工处理

  ```html
  <div id="app">
      <fruit-list :list='list'>
          <template slot-scope='slotProps'>
              <strong v-if='slotProps.info.id==3' class="current">{{slotProps.info.name}}</strong>
              <span v-else>{{slotProps.info.name}}</span>
          </template>
      </fruit-list>
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">
      /*
        作用域插槽
      */
      Vue.component('fruit-list', {
          props: ['list'],
          template: `
          <div>
            <li :key='item.id' v-for='item in list'>
              <slot :info='item'>{{item.name}}</slot>
            </li>
          </div>
        `
      });
      var vm = new Vue({
          el: '#app',
          data: {
              list: [{
                  id: 1,
                  name: 'apple'
              }, {
                  id: 2,
                  name: 'orange'
              }, {
                  id: 3,
                  name: 'banana'
              }]
          }
      });
  </script>
  ```

  





















































