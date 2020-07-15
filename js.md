## javaScript

### 1. 什么是javaScript

javaScript是一门世界上最流行的脚本语言。

```一个合格的java程序员必须精通javaScript```

### 2. 快速入门

1. 内部标签

   ```js
   <script>
       alert("Hello World!")
   </script>
   ```

2. 外部引入

   ```html
   <scritp src="../js/index.js"></scritp>
   <!--注意：scrip标签必须成对出现-->
   ```

#### 2.1 基本语法

1. 定义变量

   ```js
   // javaScript变量没有类型，统一用var 变量名=xx;
   var num = 1;
   var name = "bai";
   ```

2.  数据类型

   `number` js不区分小数和整数，Number

   ```javascript
   123 //整数123
   123.1 // 浮点数
   1.23e3 //科学计数法
   -99 //负数
   NaN //no a number
   Infinity //表示无限大
   ```

   `字符串`

   ```javascript
   'abc'
   "abc"
   ```

   `布尔值`

   ```javascript
   true, false
   ```

   `逻辑运算`

   ```javascript
   && 与
   || 或
   !	非
   ```

   `比较运算符`

   ```javascript
   = 赋值
   == 等于
   === 绝对一样（类型一样，值一样，结果为true）(重要)
   ```

   这是一个js的缺陷，坚持不要使用==比较

   须知：

   - NaN===NaN，与所有的数值都不相等，包括自己
   - 只能通过isNaN(NaN)来判断是否为NaN

   浮点数问题：

   ```javascript
   console.log((1/3)===(1-2/3)) //false
   ```

   尽量避免使用浮点数进行运算，存在精度问题！

   `null和undefined`

   - null 空
   - undefined 未定义

   `数组`

   javaScript数组的内容可以是不同类型

   ```javascript
   var arr = [1, 2, 3, 4, 'hello', null, true]
   var arr = new Array(1, 2, 3, 4, 'hello', null, true);
   
   ```

   `对象`

   对象是大括号，数组是中括号

   ```javascript
   var person{
       name:"bai",
       age:3,
       tags:['js', 'java', 'web']
   }
   // 访问类变量
   person.name
   person.age
   ```

#### 2.2 严格检查模式

   ```javascript
'use strict'; // 严格检查模式，预防javaScript的随意性导致的一些问题
// 必须写在javaScript的第一行
//局部变量建议都使用let去定义
let a = 2;
   ```

### 3. 数据类型

#### 3.1 字符串

1. 正常字符串使用单引号或者双引号包裹

2. 注意转义字符  \n \t  

3. 字符串是不可变的

4. 多行字符串编写

   ```javascript
   // 这里不是单引号，是esc键下面的那个点
   var msg = `hello world
   你好阿`;
   ```

5. 模板字符串

   ```javascript
   let name = "bai";
   let age = "18";
   //此处的引号为esc键下方的点
   let msg = `你好阿，${name}`;
   console.log(msg);
   ```

6. 字符串长度

   ```javascript
   let srt = "hello world";
   let len = srt.length;
   ```

#### 3.2 数组

Array可以包含任意的数据类型

```javascript
var arr = [1,2,3,4,5,6];
```

1. 长度

   ```javascript
   arr.length
   //注意给arr.length赋值，数组大小就会发生变化，如果赋值过小，元素就会丢失
   ```

2. slice() 截取数组的一部分，返回一个新数组 ，类似于String中的substring。

3. push(), pop() 将数据压入数组尾部和弹出数组尾部数据

4. unshift(), shift() 将数据压入数组头部和弹出数组头部数据

#### 3.3 对象

若干个键值对

```javascript
var 对象名 = {
    属性名：属性值，
    属性名：属性值，
    属性名：属性值，
    属性名：属性值
}

var person = {
    name:"Bai",
    age:20,
    email:"xxxxx",
    score:0
}
```

js中对象，{...}表示一个对象 ，键值对描述属性xxxx:xxx，多个属性之间使用逗号隔开，最后一个属性不加逗号！

1. 对象赋值

   ```javascript
   person.name = "haha";
   ```

2. 使用一个不存在的对象 属性，不会报错！只会说undefined

3. 动态的删减属性

   ```javascript
   delete person.name;
   // 通过delete删除name属性
   ```

4. 动态的添加属性

   ```javascript
   person.haha = "haha";
   //直接给对象赋值就行
   ```

5. 判断属性值是否在这个对象中 xxx in xxx

   ```javascript
   'age' in person
   true
   ```

6. 判断一个属性是否是这个对象自身拥有的hasOwnProperty()

   ```javascript
   person.hasOwnProperty('toStrng')
   false
   person.hasOwnProperty('age')
   true
   ```

#### 3.4 流程控制

if-else语句

if-else if-else语句

while，for循环

#### 3.5 Map和Set

ES6的新特性~

Map:

```javascript
var map = new Map(['tom', 100], ['jack', 90], ['tony', 80]);
var value = map.get('tom');
map.set('admin', 123);
map.delete('tom');
```

Set: 无序不重复的集合

```javascript
var set = new Set([3,1,2,5,3,4,1]); //去重
set.add(9);
```



###  4. 函数

#### 4.1 定义函数

```javascript
// 方式1
function abs(x){
    if (x >= 0){
        return x;
    }else{
        return -x;
    }
}

// 方式2
// function(x) 这是一个匿名函数，但是可以把结果赋值给abs,通过abs就可以调用 函数
var abs = function(x){
    if (x >= 0){
        return x;
    }else{
        return -x;
    }
}

abs(10)
abs(-10)
```

`arguments`是js免费赠送的关键字，代表传递进来的所有的参数，是一个数组

`...rest`获得默认参数后的所有参数,ES6新特性

#### 4.2 变量的作用域

在javaScript中，var定义变量实际是有作用域的。

假设在函数体中声明，则在函数体外不可以使用（闭包）

```javascript
function fun(){
    var x = 1;
    x = x + 1;
}
// 此处的x会报没有定义
x = x + 1;
```

如果两个函数使用了相同的变量名，只要在函数内部，就不冲突

```javascript
function fun(){
    var x = 1;
    x = x + 1;
}

function fun1(){
    var x = 1;
    x = x + 1;
}

// 两个函数里的x是不冲突的
```

#### 4.3 解决全局变量命名冲突

```javascript
// 定义一个全局变量
var bai = {};
// 定义全局变量
bai.name = "bai";
bai.add = function(a, b){
    return a + b;
}
```



#### 4.4 方法

```javascript
var bai = {
    name:"chenzp",
    bitrh:2000,
    
    age:function(){
        var now = new Date().getFullYear();
        return now-this.bitrh;
    }
    
}

// 属性
bai.name;
//方法，一定要带()
bai.age();
```

this.代表什么？

谁调用，this就是谁。也可以用apply指定this是谁

### 5. 内部对象

#### 5.1 Date

```javascript
var now = new Date();
// 时间戳是指从1970/1/1 0:0:0到当前的毫秒数
```

#### 5.2 JSON

在javaScript一切皆为对象，任何js支持的类型都可以用JSON来表示

格式：

- 对象 都是用{}
- 数组都用[]
- 所有的键值对都是用key:value

```javascript
var user = {
    name:"bai",
    age:3,
    sex:"男"
}
//对象转化为JSON字符串
var jsonUser = JSON.stringify(user);

//json 字符串转化为对象
var obj = JSON.parse('{"name":"bai", "age":3, "sex":"男"}');
```

### 6. 面向对象编程

```javascript
// 1. 定义一个学生的类
class Student{
    constructor(name){
        this.name=name;
    }
    
    hello(){
        alert("hello");
    }
}

var bai = new Student("Bai");

// 2.继承
class XiaoStudent extends Student{
    constructor(name, grade){
        supper(name);
        this.grade = grade;
    }
    
    myGrade(){
        alert("我是一名小学生");
    }
}
```

### 7. 操作BOM对象（重点）

BOM:浏览器对象模型

`window` 代表浏览器窗口（重要）

`navigator` 封装了浏览器的信息，大多数时候我们不会使用该对象，因为会被人为修改！

`screen` 代表屏幕

`location` 代表当前页面的URL信息（重要）

`document` 代表当前的页面，HTML DOM文档树

`history` 代表浏览器的历史记录

获取具体的文档树节点

```html
<dl id="app">
    <dt>java</dt>
    <dd>javaSE</dd>
    <dd>javaEE</dd>
</dl>

<script>
	var dl = document.getElementById('app');
</script>
```

获取cookie

```javaschtmript
document.cookie
```

历史

```javascript
history.back() // 后退
history.forward() // 前进
```

### 8. 操作DOM（重点）

浏览器网页就是一个DOM树形结构！

- 更新：更新Dom节点
- 遍历dom节点：得到Dom节点 
- 删除：删除一个Dom节点
- 添加：添加一个新的节点

要操作一个Dom节点，就必须要先获得这个Dom节点

#### 8.1 获得DOM节点

```html
<div id="father">
    <h1>标题1</h1>
    <p id="p1">p1</p>
    <p class="p2">p2</p>
</div>
<srcitp>
    var father = document.getElementById('father');
	var h1 = document.getElementsByTagName("h1");
	var p1 = document.getElementById('p1')
	var p2 = document.getElementsByClassName("p2")
</srcitp>

```

#### 8.2 更新DOM节点

```html
// innerText不会解析html标签，只能解析为文本
document.getElementById('father').innerText="123"
// innerHTML可以解析html标签
document.getElementById('father').innerHTML="<strong>123</strong>"
// style可以设置css样式
document.getElementById('father').style.color="red";
```

#### 8.3 删除DOM节点

删除节点的步骤：

- 先获取父节点
- 通过父节点删除自己

```html
let p1 = document.getElementById('p1');
let father = p1.parentElement;
father.removeChild("p1");
```

#### 8.4 创建和插入一个DOM节点

我们获得了某个DOM节点，假设这个dom节点是空的，我们通过innerHTML就可以增加一个元素，但是这个DOM节点已经存在元素了，我们就不能这么干了，会产生覆盖。应该用追加

```html
<p id="js">JavaScript</p>
<div id="list">
    <p id="se">JaveSE</p>
    <p id="ee">JavaEE</p>
    <p id="me">JavaME</p>
</div>

// 追加
<script>
    var js = document.getElementById("js"); //已经存在的节点
    var list = document.getElementById("list")
    list.append(js);
    
     var newp = document.createElement('p'); //新创建一个节点
    newp.innerText="bai,bai";
    newp.setAttribute("style", "color:red"); // 设置属性
    list.appendChild(newp);
</script>

// 插入某一节点的前面
list.insertBefore(js, newp);
```

### 9. 操作表单

```
表单是什么 form DOM树一个节点
```

- 文本框 text
- 下拉框 <select>
- 单选框 radio
- 多选框 checkbox
- 隐藏域 hidden
- 密码框 password
- ...

表单的目的：提交信息

#### 9.1 获得表单信息

```html
<form>
    <span>用户名：</span><input type="text" id="userName">
    <p>
        <span>性别：</span>
        <input type="radio" name="sex" value="男" id="boy">男
        <input type="radio" name="sex" value="女" id="girl">女
    </p>
</form>

<script>
    // 文本框取值
	var userName = document.getElementById('userName');
    // 得多值
    userName.value
    // 修改值
    userName.value="bai";
    
    // 单先框取值
    var boy = document.getElementById("boy");
    var girl = document.getElementById("girl");
    // 取值
    boy.checked;
    girl.checked;
    // 修改
    boy.checked=true;
</script>
```

#### 9.2 提交表单

```html
// onsubmit=绑定一个提交的函数， 返回true或false
// 将这个结果返回给表单，使用onsubmit接收
<form action="https://www.baidu.com" method="post" onsubmit="return checkForm()">
    <p><span>用户名：</span><input type="text" id="userName" name="userName"></p>
    <p>
        <span>密码：</span>
<!--        <input type="radio" name="sex" value="男" id="boy">男-->
<!--        <input type="radio" name="sex" value="女" id="girl">女-->
        <input type="password" name="password" id="pwd">
    </p>
    <button type="submit" onclick="checkForm()">提交</button>
</form>

function checkForm() {
        var userName = document.getElementById('userName');
        var pwd = document.getElementById('pwd');
        console.log(userName.value);
        console.log(pwd.value);
        return true;
}
```

### 10. jQuery

jQuery库，里面存在大量的javaScript函数 

 jQuery公式：

```javascript
公式： $(selector).action()
selector就是css的选择器
action（）就是事件
```

```javascript
$('p').click() //标签选择器
$('#id').click() //id选择器
$('.class').click() //类选择器

//网页加载完再执行
$(function(){
    // TODO
});
```



