### 1. Vue

#### 1. 1 UI框架

- Ant-Design: 阿里，基于React的UI框架
- ElementUI、iview、ice: 饿了么，基于Vue的UI框架
- Bootstrap: Twitter 用于前端开发的开源工具包
- AmazeUI: 又叫“妹子UI”一款HTML5跨屏前端框架

#### 1.2 javaScript构建工具

- Babel: js编译工具，主要用于浏览器不支持的ES新特性，比如用于编译TypeScript
- WebPack： 模块打包器，主要作用是打包，压缩，合并及按序加载

#### 1.3 vue使用

Vue是一套用于构建用户界面的渐进式框架，Vue被设计为可以自底向上逐层应用。

- idea上安装vue.js插件

- 导入Vue.js

  ```html
  <script src="static/vue/vue.js" type="text/javascript"></script>
  ```

- 编写测试

  ```html
  <div id="app">
      <!-- 双{}表示引用message的值，相当于jsp的${}-->
      {{message}}
  </div>
  
  <script>
      var vm = new Vue({
          el: "#app",
          data: {
              message: "hello,Vue!"
          },
      });
  </script>
  ```

### 2. Vue基本语法

- v-bind : 用于绑定数据和元素属性的

	```js
var app = new Vue({
    el:'.app',
    data:{
        url:"https://www.baidu.com",
    }
});
	```

	```html
	<div class="app">
	    <a v-bind:href="url">click me</a>
	</div>  
	```

- v-if , v-else

  ```html
  <div id="app">
      <h1 v-if="ok">Yes</h1>
      <h1 v-else>No</h1>
  </div>
  
  <script>
      var vm = new Vue({
          el: "#app",
          data: {
              ok: true
          }
      });
  </script>
  ```

- v-else-if

  ```html
  <div id="app">
      <h1 v-if="type === 'A'">A</h1>
      <h1 v-else-if="type === 'B'">B</h1>
      <h1 v-else>C</h1>
  </div>
  
  <script>
      var vm = new Vue({
          el: "#app",
          data: {
              type:'A'
          }
      });
  </script>
  ```

- v-for

  ```html
  <div id="app">
     <ul>
         <li v-for="item in items">{{item.message}}</li>
     </ul>
  </div>
  
  <script>
      var vm = new Vue({
          el: "#app",
          data: {
              items:[
                  {message:"张三"},
                  {message: "李四"},
                  {message: "王五"}
              ]
          }
      });
  </script>
  ```

### 3. 绑定事件（v-on）

```html
<div id="app">
    <button v-on:click="sayHello">点击</button>
</div>

<script>
    var vm = new Vue({
        el: "#app",
        data:{
            message: "Hello Vue!"},
        methods:{ // 方法必须定义在vue的methods对象中
            sayHello: function () {
                alert(this.message);
            }
        }
    });
</script>
```

### 4. 数据双向绑定(v-model)

```html
<div id="app">
   输入的文本 <input type="text" v-model="message">{{message}}
   性别： <input
</div>
<!-- 将message与input -->
<script>
    var vm = new Vue({
        el: "#app",
        data:{
          message: "123"
        },
    });
</script>
```

### 5. Vue组件

```html
<div id="app">
    <my v-for="item in items" v-bind:item="item"></my>
</div>

<script>
    // 定义组件
    Vue.component("my", {
        props: ["item"], // 用于接收数据
        template: '<li>{{item}}</li>'
    });

    var vm = new Vue({
        el: "#app",
        data: {
            items: ["java", "Linux", "前端"]
        },
    });
</script>
```

### 6. 网络通信

#### 6.1 Axios异步通信

导包：

```html
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
```

```html
<div id="app">
    <div>{{info.name}}</div>
    <div>{{info.address.city}}</div>
</div>

<script>
    var vm = new Vue({
        el: "#app",
        data() {
            return {
                // 请求的返回参数数一样，必须和json字符串一样
                info: {
                    name: null,
                    address: {
                        street: null,
                        city: null,
                        country: null
                    }
                }
            }
        },
        mounted() { //钩子函数 链式函数
            axios.get(url).then(response => (this.info=response.data));

        },
    });
```

### 7. 计算属性

```html
<div id="app">
    {{currentTime()}}
    {{currentTime1}}
</div>

<script>
    var vm = new Vue({
        el: "#app",
        data: {
            message: "hello",
        },
        methods: {
            currentTime: function () {
                return Date.now(); //返回一个时间戳
            }
        },
        computed: {  // 计算属性：methods,computed 方法名不能重名，同名只会调用methods的方法
            currentTime1: function () {
                this.message;
                return Date.now();
            }
        }
    });
</script>
```

计算属性可以理解为缓存，methods与computed调用方式不一样

### 8. 插槽slot

```html
<div id="app">
    <p>列表书籍</p>
    <todo>
        <todo-title  slot="todo-title" v-bind:title="title"></todo-title>
        <todo-item slot="todo-items" v-for="item in items" v-bind:item="item"></todo-item>
    </todo>
</div>

<script>
    // 定义插槽
    Vue.component("todo", {
        props:[],
        template:   '<div>' +
                        '<slot name="todo-title"></slot>' +
                        '<ul>' +
                            '<slot name="todo-items"></slot>'+
                        '</ul>' +
                    '</div>'
    });

    Vue.component("todo-title",{
        props:['title'],
        template: '<div>{{title}}</div>'
    });

    Vue.component("todo-item", {
        props:['item'],
        template: '<li>{{item}}</li>'
    });

    let vm = new Vue({
        el: "#app",
        data:{
            title:"列表标题",
            items:['java', 'linux', 'mysql']
        }
    });
</script>
```

### 9. 自定义事件

```html
<div id="app">
    <p>列表书籍</p>
    <todo>
        <todo-title  slot="todo-title" v-bind:title="title"></todo-title>
        <todo-item slot="todo-items" v-for="(item,index) in items" v-bind:item="item"
                   v-bind:index="index" v-on:remove="removeItem(index)">

        </todo-item>
    </todo>
</div>

<script>

    // 定义插槽
    Vue.component("todo", {
        props:[],
        template:   '<div>' +
                        '<slot name="todo-title"></slot>' +
                        '<ul>' +
                            '<slot name="todo-items"></slot>'+
                        '</ul>' +
                    '</div>'
    });

    Vue.component("todo-title",{
        props:['title'],
        template: '<div>{{title}}</div>'
    });

    Vue.component("todo-item", {
        props:['item', 'index'],
        template: '<li v-bind:value="index">{{item}}&nbsp;<button v-on:click="remove">删除</button></li>',
        methods:{
            remove:function (index) {
                this.$emit('remove', index)
            }
        }
    });

    var vm = new Vue({
        el: "#app",
        data:{
            title:"列表标题",
            items:['java', 'linux', 'mysql']
        },
        methods: {
            removeItem:function (index) {
                console.log("删除了" + this.items[index] + "ok")
                this.items.splice(index, 1);
            }
        }
    });
</script>
```
### 10. vue-cli

#### 10.1 安装node.js

- 官网下载安装包安装

- 安装node.js淘宝镜像加速器

  ```shell
  npm install cnpm -g  # -g全局安装
  ```

#### 10.2 安装vue-cli

```shell
cnpm install vue-cli -g
vue list
```

### 创建项目

管理员命令窗口，进入项目目录执行下面的命令

```shell
vue init webpack myvue
```

