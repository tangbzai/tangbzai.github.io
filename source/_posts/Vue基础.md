---
title: Vue基础
date: 2021-01-10 23:01:56
tags: Vue
mermaid: true
---

## Vue 基础

### Vue 概述

> [vue2 中文官方文档](https://cn.vuejs.org/)

渐进式 JavaScript 框架<br>
{% mermaid %}
graph LR
声明式渲染-->组件系统
组件系统-->客户端路由
客户端路由-->集中式状态管理
集中式状态管理-->项目构建
{% endmermaid %}
特点：

- 易用：熟悉 HTML、CSS、JavaScript 知识后，可快速上手 Vue
- 灵活：在一个库和一套完整框架之间自如伸缩
- 高效：20kB 运行大小，超快虚拟 DOM

### Hello Vue

```html
<!-- body -->
<div id="app">{{msg}}</div>

<!-- 引入vue文件 -->
<script src="js/vue.js"></script>
<script>
  var vm = new Vue({
    el: '#app',
    data: {
      msg: 'Hello Vue'
    }
  })
</script>
<!-- bodyEnd -->
```

| 参数 | 数据类型                               | 说明           |
| ---- | -------------------------------------- | -------------- |
| el   | css 选择器(String)或 DOM 元素(Element) | 元素的挂载位置 |
| data | Object                                 | 模型数据       |

### 指令

**指令的本质就是自定义属性。**<br>
Vue 的指令格式为：`v-`开头（例如`v-cloak`）

#### `v-cloak`指令解决插值表达式存在的闪动

`v-cloak`指令会保持在元素上直到关联实例结束编译。

```
/* css */
[v-cloak]{
    display: none;
}
```

```html
<!-- 入口标签添加v-cloak -->
<div id="app" v-cloak></div>
```

#### 数据绑定指令

##### `v-text` 填充文本

更新元素的文本内容。

```html
<!-- 插值表达式 -->
<div>{{msg}}</div>

<!-- 上下两种方法一样的 -->

<!-- v-text指令 -->
<div v-text="msg"></div>
```

##### `v-html` 填充 HTML 片段

更新元素的`innerHTML`。**内容按普通的 HTML 插入——不会作为 Vue 模板进行编译**。<br>

> 在网站上动态渲染任意 **HTML** 是非常危险的，因为容易导致 **XSS** 攻击。只在可信内容上使用 **v-html**，**永不用在==用户提交==的内容上。**

```html
<div v-html="html"></div>
```

##### `v-pre` 填充原始信息

**显示原始信息**，跳过这个元素和它的子元素的编译过程。可以用来显示原始 Mustache 标签（花括号包裹的这些模板语法）。**跳过大量没有指令的节点会加快编译。**

```html
<div v-pre>{{ this will not be compiled }}</div>
```

#### `v-once` 只填充一次

**只渲染元素和组件一次**。随后的重新渲染，元素/组件及其所有的子节点将被视为静态内容并跳过。这可以用于**优化更新性能。**

```html
<!-- 单个元素 -->
<span v-once>This will never change: {{msg}}</span>
<!-- 有子元素 -->
<div v-once>
  <h1>comment</h1>
  <p>{{msg}}</p>
</div>
<!-- 组件 -->
<my-component v-once :comment="msg"></my-component>
<!-- `v-for` 指令-->
<ul>
  <li v-for="i in list" v-once>{{i}}</li>
</ul>
```

### 样式绑定

#### `class`

- 对象语法

```html
<div v-bind:class="{active: isActive}"></div>
```

- 数组语法

```html
<div v-bind:class="[activeClass, errorClass]"></div>
```

#### `style`

- 对象语法

```html
<div v-bind:style="{color: activeColor, fontSize: fontSize}"></div>
```

- 数组语法

```html
<div v-bind:style="[baseStyles, overridingStyles]"></div>
```

### 条件渲染

- `v-if`
- `v-else`
- `v-else-if`
- `v-show`
  > 不推荐同时使用`v-if`和`v-for`。<br>
  > 当`v-if`与`v-for`一起使用时，`v-for`具有比`v-if` 更高的优先级。请查阅列表渲染指南以获取详细信息。

#### `v-if` vs `v-show`

`v-if` 是“真正”的条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建。

`v-if` 也是**惰性的**：如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。

相比之下，`v-show` 就简单得多——不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 进行切换。

一般来说，`v-if` 有更高的切换开销，而 `v-show` 有更高的初始渲染开销。**因此，如果需要非常频繁地切换，则使用 `v-show` 较好；如果在运行时条件很少改变，则使用 `v-if` 较好。`**

### 循环渲染

- `v-for`

```
<!-- 遍历数组 -->
v-for="(item, index) in list"
<!-- 遍历对象 -->
v-for="(value, name, index) in object"
```

参数说明：

1. 第一个参数为循环的当前值
2. 第二个参数为循环的当前 property 名称 (也就是键名)
3. 第三个参数为索引

- `:key`<br>
  建议尽可能在使用 v-for 时提供 key attribute，除非遍历输出的 DOM 内容非常简单，或者是刻意依赖默认行为以获取性能上的提升。

> 因为它是 Vue 识别节点的一个通用机制，key 并不仅与 v-for 特别关联。后面我们将在指南中看到，它还具有其它用途。<br> >**不要使用对象或数组之类的非基本类型值作为 v-for 的 key。请用字符串或数值类型的值。**

### 计算属性

```html
<!-- html -->
<div id="app">
  {{reverse}}
</div>
```

```js
// script
var vm = new Vue({
  el: "#app",
  data: {
     msg: "Hello",
  }
  computed: {
    reverse: function(){
        return this.msg.split('').reverse().join("");
    }
  }
})

```

#### 计算属性与方法的区别

- 计算属性是是根据依赖进行缓存的。
- 方法不存在缓存。

### 监听器

用于数据变化时执行异步或开销较大（耗时）的操作。

```js
watch: {
  firstName: function(val){
      // val 表示变化后的值
      this.fullName = val + this.lastName;
  }
  lastName: function(val){
      // val 表示变化后的值
      this.fullName = this.firstName + val;
  }
}
```

### 过滤器

#### 语法

两种定义方式:

```js
// 全局过滤器
Vue.filter('lower', function (val) {
  return val.charAt(0).toLowerCase() + val.slice(1);
})


var vm = new Vue({
  el: "#app",
  data: {
    msg: "",
    time: new Date()
  },
  // 局部过滤器
  filters: {
    upper: function (val) {
      return val.charAt(0).toUpperCase() + val.slice(1);
    },
  }
})
```

使用时：

- 可级联操作会按顺序应用过滤器。
- 可属性值（`v-bind`）绑定.
- 可带参数。

```html
<input type="text" v-model="msg">
<!-- 支持级联操作 {{msg | upper | lower}} 会按顺序应用 -->
<div>首字母大写：{{msg | upper}}</div>
<div>首字母小写：{{msg | lower}}</div>
```

#### 综合例子

```html
<!-- html -->
<input type="text" v-model="msg">
<!-- 支持级联操作 {{msg | upper | lower}} 会按顺序应用 -->
<div>首字母大写：{{msg | upper}}</div>
<div>首字母小写：{{msg | lower}}</div>
<div :abc="msg | upper">属性值绑定</div>
<!-- 带参数的过滤器 -->
<div>{{time | format("yyyy-MM-dd")}}</div>
```

```js
// script
// 全局过滤器
Vue.filter('lower', function (val) {
  return val.charAt(0).toLowerCase() + val.slice(1);
})
var vm = new Vue({
  el: "#app",
  data: {
    msg: "",
    time: new Date()
  },
  // 局部过滤器
  filters: {
    upper: function (val) {
      return val.charAt(0).toUpperCase() + val.slice(1);
    },
    // 带参数的过滤器
    format: function (val, arg) {
      if (arg == "yyyy-MM-dd") {
        return `${val.getFullYear()}-${(val.getMonth() + 1)}-${val.getDate()}`;
      }
    }
  }
})
```

### 生命周期

- 挂载
  - `beforeCreate`： 在实例初始化之后，数据观测和事件配置之前调用。
  - `created`： 在实例创建完成洪湖被立即调用。
  - `beforeMount`： 在挂载开始前被调用。
  - `mounted`： 被新创建的`vm.$el`替换，并挂载到实例上去之后调用。
- 更新
  - `beforeUpdate`：数据更新时调用，发生在虚拟 DOM 打补丁之前。
  - `updated`：数据更新时调用，发生在虚拟 DOM 打补丁之后。
- 销毁
  - `beforeDestroy`：实例销毁之前调用。
  - `destroyed`：实例销毁之后调用。

![官方生命周期说明图](https://cn.vuejs.org/images/lifecycle.png)

### 修改响应式数据

```
Vue.set(vm.item, indexOfItem, newValue);
// 两者效果相同
vm.$set(vm.item, indexOfItem, newValue);
```

| 参数          | 说明             |
| ------------- | ---------------- |
| `vm.item`     | 要处理的数组名称 |
| `indexOfItem` | 要处理的数组索引 |
| `newValue`    | 要处理的数组的值 |

### 组件

#### 组件化规范 Web Components

[规范文档](https://developer.mozilla.org/zh-CN/docs/web/web_components)<br>
目前并未完全被浏览器支持。

#### 组件注册

```js
<!-- 全局组件 -->
Vue.compoent(组件名称，{
    data: 组件数据,
    template: 组件模版内容
})
<!-- 局部组件 只能在注册它的父组件中使用 -->
new Vue({
    el: "#app",
    components: {
        'component-a': {
            data: 组件数据,
            template: 组件模版内容
        },
        'component-b': {
            data: 组件数据,
            template: 组件模版内容
        }
    }
})
```

注意事项：

- `data`必须是一个函数返回的对象。
- 组件模版内容必须是单个根元素
- 组件模版内容可以是模板字符串（需要浏览器支持 ES6 语法）

#### 组件命名方式

短横线方式

```js
Vue.component('my-component',{/*...*/})
```

驼峰方式(该命名放式只能在字符串的模板里使用，否则得==转换成短横线方式再使用==)

```js
Vue.component('MyComponent',{/*...*/})
```

#### 组件之间的数据交互

##### 父组件向子组件传值

1. 组件内部通过`props`接收传递过来的值。

```js
Vue.component('menu-item',{
    props: ['title'],
    template: '<div>{{ title }}</div>'
})
```

2. 父组件通过属性将值传给子组件。

```html
<menu-item title="来自父组件的数据"></menu-item>
<menu-item :title="title"></menu-item>
```

3. props 属性名规则

- 在`props`中使用驼峰形式，在模版中需要使用短横线的形式
- 在字符串的模版中可以不用转换成短横线的形式。

```js
Vue.component('menu-item',{
    // 在JavaScript中是
    props: ['menuTitle'],
    template: '<div>{{ menuTitle }}</div>'
})
<!-- 在html中是短横线方式的 -->
<menu-item menu-title="nihao"></menu-item>
```

4. props 属性值类型

- 字符串 `String`
- 数值 `Number`
- 布尔值 `Boolean`
- 数组 `Array`
- 对象 `Object`

```html
<menu-item :num1="12" num2="12"></menu-item>

Vue.component('menu-item', {
    props: ["num1", "num2"],
    template: `<div>
        <div>{{ typeof num1}}</div>
        <div>{{ typeof num2}}</div>
    </div>`,
})

// 输出
// Number
// String
```

\*注意区分`v-bind:`的属性与 html 自定义属性的区别。自定义属性的值都是`String`类型。

##### 子组件向父组件传值

1. 子组件通过自定义事件向父组件传递信息

```html
<button v-on:click="$emit("enlarge-text")"> 扩大字体 </button>
```

2. 父组件监听子组件的事件

```html
<menu-item v-on:enlarge-text="fontSize += 0.1"></menu-item>
```

3. 子组件通过自定义事件向父组件传递信息

```html
<button v-on:click="$emit(enlarge-text, 0.1)">扩大字体</button>
```

4. 父组件监听子组件的事件

```html
<menu-item v-on:elarge-text="fontSize += $event"></menu-item>
```

\*`$event`的名字是固定的

##### 非父子组件之间的传值

1. 单独的是中心管理组件间的通信

```js
var eventHub = new Vue()
```

2. 监听事件与销毁事件

```js
// 监听事件
eventHub.$on('add-todo', addTodo)
// 销毁事件
eventHub.$off(‘add-todo’)
```

3. 触发事件

```js
eventHhub.$emit('add-todo', id)
```

##### 组件插槽

###### 一般用法

1. 在组件模板中添加`slot`（里面可以填写默认内容）

```js
Vue.component('alert-box', {
    template: `<div>
        <strong>Error:</strong>
        <slot>默认内容</slot>
    </div>`,
})
```

2. 在使用组件标签中添加内容

```html
<alert-box>有bug发生</alert-box>
<alert-box></alert-box>

<!-- 输出 -->
<!-- Error: 有bug发生 -->
<!-- Error: 默认内容 -->
```

###### 具名插槽

1. 插槽定义

```html
 <!-- template内 -->
<div>
    <header>
        <slot name="header"></slot>
    </header>
    <main>
        <slot></slot>
    </main>
    <footer>
        <slot name="footer"></slot>
    </footer>
</div>
```

2. 插槽使用

```html
<!-- 使用组件时的内容位置 -->
<h1 slot="header">标题内容</h1>
<p>主要内容1</p>
<p>主要内容2</p>
<p slot="footer">底部内容</p>
```

\*使用`<template>`标签可以插入多个标签到一个插槽位置。

```html
<!-- 使用组件时的内容位置 -->
<template slot="header">
    <p>标题信息1</p>
    <p>标题信息2</p>
</template>
<p>主要内容1</p>
<p>主要内容2</p>
<template slot="footer">
    <p>标题信息1</p>
    <p>标题信息2</p>
</template>
```

###### 作用域插槽

作用：父组件对子组件的内容进行加工处理。<br/>
插槽声明：

```js
Vue.component('fruit-list', {
    props: ['list'],
    template: `<ul>
        <li v-for="item in list" :key="item.id">
            <slot :info="item">{{item.name}}</slot>
        </li>
    </ul>`,
})
```

插槽使用：

```html
<fruit-list :list="list">
    <template slot-scope="slotProps">
        <strong>{{slotProps.info.name}}</strong>
    </template>
</fruit-list>
```

其中`slot-scope`为获取插槽属性列表，上面代码将列表命名为`slotProps`，使用列表获取插槽绑定的`info`属性即循环的内的`item`。
