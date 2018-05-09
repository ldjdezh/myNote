# Vue

## 绑定Html class

```html
<div v-bind:class="{css类名: boolean值, css类名: boolean值, ...}"></div>
```



## v-bind和v-on缩写

v-bind缩写

```html
<a v-bind:href="url"></a>

<!-- 缩写 -->
<a :href="url"></a>
```



v-on缩写

```html
<a v-on:click="dosomething"></a>

<a @click="dosomething"></a>
```



## 事件对象获取值

在input标签中可以通过注册方法的事件对象获取标签上的值

```javascript
a.onkeydown = function(e) {
    // 可以通过e.target.value获取标签上的值
    console.log(e.target.value)
}
```



## 按键修饰符

在监听键盘事件时，我们经常需要检查常见的键值。Vue允许为`v-on`在监听键盘事件时添加按键修饰符：

```html
<!-- 只有 keyup 是 13 时调用 -->
<input v-on:keyup.13="submit">
```



记住所有的 `keyCode` 比较困难，所以Vue为常用的按键提供了别名：

```html
<input v-on:keyup.enter="submit">

<!-- 缩写 -->
<input @keyup.enter="submit">
```



全部的按键别名：

+ `.enter`
+ `.tab`
+ `.delete` (捕获“删除”和“退格”键)
+ `.esc`
+ `.space`
+ `.up`
+ `.down`
+ `.left`
+ `.right`



可以通过全局 `config.keyCodes` 对象自定义按键修饰符别名：

```html
// 可以使用 v-on:keyup.f1
Vue.config.keyCodes.f1 = 112
```



## $event变量

有时需要在内联语句处理器中访问原始的DOM事件。可以用特殊变量`$event`把它传入方法：

```html
// 通过$event传入DOM事件对象
<button v-on:click="warn($event)">
    
</button>
```

```javascript
methods: {
    warn: function(event) {
        console.log(event)
    }
}
```



## 数组的every方法

### Array.prototype.every()

这个`every`方法测试数组的所有元素是否都通过了指定函数的测试

### 语法

```javascript
arr.every(callback[, thisArg])
```



### 参数

`callback`

用来测试每个元素的函数

`thisArg`

执行`callback`时使用的`this`值

### 描述

`every`方法为数组中的每个元素执行一次`callback`函数，直到它找到一个使`callback`返回`false`的元素，一旦找到，`every`函数将会立即返回`false`，如果找不到就返回`true`

简单来说，就是有一个是`false`就返回`false`

### callback函数的`3`个参数

+ 元素值
+ 元素的索引
+ 原数组

`every`不会改变原数组



### 实例

例子：检测所有数组元素的大小

下例检测数组中的所有元素是否都大于10

```javascript
function isBigEnough(element, index, array) {
    return (element >= 10)
}
// 这里返回false，因为在数组中有元素小于10，不符合条件
var passed = [12,5,8,130,44].every(isBigEnough)

// 这里返回true，因为在数组中所有元素都大于10
passed = [12,54,18,130,44].every(isBigEnough)
```



## 数组的some方法

### Array.prototype.some()

`some()`方法测试数组中的某些元素是否通过由提供的函数实现的测试

### 语法

```javascript
arr.some(callback[, thisArg])
```

### 参数

`callback`

用来测试每个元素的函数

`thisArg`

执行`callback`时使用的`this`值

### 描述

`some`为数组中的每一个元素执行一次`callback`函数，直到找到一个使`callback`返回`true`，一旦找到，立即返回`true`，否则返回`false`

简单来说，只要有一个是`true`，整个返回值就是`ture`

callback函数的`3`个参数

+ 元素值
+ 元素的索引
+ 原数组

如果为`some`提供一个`thisArg`参数，将会把它传给被调用的`callback`，作为`this`值。否则，在非严格模式下将会是全局对象，严格模式下是`undefined`

`some`被调用时不会改变数组



## 数组的splice方法

### Array.prototype.splice()

`splice()`方法通过删除现有元素或添加新元素来更改一个数组的内容



### 语法

```javascript
array.splice(start)

array.splice(start, deleteCount)

array.splice(start, deleteCount, item1, item2, ...)
```



### 参数

`start`

指定修改的开始位置（从0计数）。如果超出了数组的长度，则从数组末尾开始添加内容；如果是负值，则表示从数组末位开始的第几位（从-1计数）；若只使用`start`参数而不使用`deleteCount`、`item`，如：`array.splice(start)`，表示删除`[start, end]`的元素

`deleteCount` 可选

整数，表示要移除的数组元素的个数。如果`deleteCount`是0，则不移除元素。这种情况下，至少应添加一个新元素。如果`deleteCount`大于`start`之后的元素的总数，则从`start`后面的元素都将被删除（含第`start`位）。

`item1, item2, ...`可选

要添加进数组的元素，从`start`位置开始，如果不指定，则`splice()`将只删除数组元素



### 返回值

由被删除的元素组成的一个数组。如果只删除了一个元素，则返回只包含一个元素的数组。如果没有删除元素，则返回空数组



### 描述

如果添加进数组的元素个数不等于被删除的元素个数，数组的长度会发生相应的改变



## 数组的slice方法

### Array.prototype.slice()

`slice()`方法返回一个从开始到结束（不包括结束）选择的数组的一部分浅拷贝到一个新数组对象。且原始数组不会被修改。



### 语法

```javascript
arr.slice()
// [0, end]

arr.slice(begin)
// [begin, end]，这里包括end

arr.slice(begin, end)
// [begin, end) ，这里不包括end
```



### 参数

`begin`可选

从该索引处开始提取原数组中的元素（从0开始）

如果该参数为负数，则表示从原数组中的倒数第几个元素开始提取，`slice(-2)`表示提取原数组中的倒数第二个元素到最后一个元素（包含最后一个元素）。

如果省略`begin`，则`slice`从索引0开始

`end`可选

在该索引处结束提取原数组元素（从0开始）。`slice`会提取原数组中索引从`begin`到`end`的所有元素（包含`begin`，但不包含`end`)。

如果该参数为负数，则它表示在原数组中的倒数第几个元素结束抽取。

如果`end`被省略，到`slice`会一直提取到原数组末尾

如果`end`大于数组长度，`slice`也会一直提取到原数组末尾



### 返回值

一个含有提取元素的新数组

### 描述

`slice`不修改原数组，只会返回一个浅复制了原数组中的元素的一个新数组。原数组的元素会按照下述规则拷贝：

+ 如果该元素是个对象引用（不是实际的对象），`slice`会拷贝这个对象引用到新的数组里。两个对象引用都引用了同一个对象。如果被引用的对象发生改变，则新的和原来的数组中的这个元素也会发生改变。
+ 对于字符串、数组及布尔值来说，`slice`会拷贝这些值到新的数组里。在别的数组里修改这些字符串或数组或布尔值，将不会影响另一个数组

如果向两个数组任一个添加了新元素，则另一个不会受到影响



## 在<template>元素上使用 v-if 条件渲染分组

因为`v-if`是一个指令，所以必须将它添加到一个元素上，但是如果想切换多个元素呢？此时可以把一个`<template>`元素当做不可见的包裹元素，并在上面使用`v-if`。最终的渲染结果将不包含`<template>`元素

```html
<template v-if="ok">
    <h1>Title</h1>
	<p>
        Paragraph 1
    </p>
    <p>
        Paragraph 2
    </p>
</template>
```



## 自定义指令

当需要对普通DOM元素进行底层操作，需要用到自定义指令

例子：聚焦输入框

```javascript
// 注册一个全局自定义指令 v-focus
Vue.directive('focus', {
    // 当被绑定的元素插入到DOM中时...
    inserted: function(el) {
        // 聚焦元素
        el.focus()
    }
})
```



如果想注册局部指令，组件中也接受一个`directives`的选项

```javascript
directives: {
    focus: {
        inserted: function(el) {
            el.focus()
        }
    }
}
```



然后你可以在模板中任何元素上使用新的`v-focus`属性，如下

```html
<input v-focus>
```



钩子函数

一个指令定义对象可以提供如下几个钩子函数（均为可选）

+ `bind`：只调用一次，指令第一次绑定到元素时调用。在这里可以进行一次性的初始化设置
+ `inserted`：被绑定元素插入父节点时调用（仅保证父节点存在，但不一定已被插入文档中）。
+ `update`：所在组件的`VNode`更新时调用，但是可能发生在其子`VNode`更新之前。指令的值可能发生了改变，也可能没有。但是你可以通过比较更新前后的值来忽略不必要的模板更新
+ `componentUpdated`：指令所在组件的`VNode`及其子`VNode`全部更新后调用
+ `unbind`：只调用一次，指令与元素解绑时调用

钩子函数参数

指令钩子函数会被传入以下参数：

+ `el`：指令所绑定的元素，可以用来直接操作DOM
+ `binding`：一个对象，包含以下属性：
  + `name`：指令名，不包括`v-`前缀
  + `value`：指令的绑定值，例如：`v-my-directive="1 + 1"`中，绑定值为`2`
  + `oldValue`：指令绑定的前一个值，仅在`update`和`componentUpdated`钩子中可用
  + `expression`：字符串形式的指令表达式。例如`v-my-directive="1 + 1"`中，表达式为`"1 + 1"`
  + `arg`：传给指令的参数，可选。例如`v-my-directive:foo`中，参数为`"foo"`
  + `modifiers`：一个包含修饰符的对象。例如`v-my-directive.foo.bar`中，修饰符对象为`{foo: true, bar: true}`
+ `VNode`：vue编译生成的虚拟节点
+ `oldVnode`：上一个虚拟节点，仅在`update`和`componentUpdated`钩子中可用

**除了el之外，其他参数都应该是只读的，切勿进行修改**



## window.onhashchange

当一个窗口的**哈希**(hash)改变时就会触发`hashchange`事件

### 语法

```javascript
window.onhashchange = funcRef
```

或者

```html
<body onhashchange="funcRef();">
    
</body>
```

或者

```javascript
window.addEventListener("hashchange",funcRef,false)
```



### 参数

`funcRef`

对一个函数的一个引用



## Location

`Location`接口表示其链接到的对象的位置(URL)。所做的修改反映在与之相关的对象上。`Document`和`Window`接口都有这样一个链接的`Location`，分别通过`Document.location`和`Window.location`访问。

### 属性

`Location`接口不继承任何属性，但是实现了那些来自`URLUtils`属性

`URLUtils.href`

包含整个URL的一个`DOMString`



`URLUtils.protocol`

包含URL对应协议的一个`DOMString`,最后有一个":"



`URLUtils.host`

包含了域名的一个`DOMString`,可能在该串最后带有一个":"并跟上URL的端口号



`URLUtils.hostname`

包含URL域名的一个`DOMString`



`URLUtils.port`

包含端口号的一个`DOMString`



`URLUtils.pathname`

包含URL中路径部分的一个`DOMString`，开头有一个"/"



`URLUtils.search`

包含URL参数的一个'DOMString'，开头有一个"?"



`URLUtils.hash`

包含块标识符的`DOMString`，开头有一个"#"



`URLUtils.username`

包含URL中域名前的用户名的一个`DOMString`



`URLUtils.password`

包含URL域名前的密码的一个`DMOString`



`URLUtils.origin` **只读**

包含页面来源的域名的标准形式`DOMString`



### 方法

`Location`没有继承任何方法，但实现了来自`URLUtils`的方法

`Location.assign()`

加载给定URL的内容资源到这个`Location`对象所关联的对象上



`Location.reload()`

重新加载来自当前URL的资源。它有一个特殊的可选参数，类型为`Boolean`，该参数为`true`时会导致该方法引发的刷新一定会从服务器上加载数据。如果是`false`或没有指定这个参数，浏览器可能从缓存当中加载页面



`Location.replace()`

用给定的URL替换掉当前的资源。与`assign()`方法不同的是用`replace()`替换的新页面不会被保存在会话的历史`History`中，这意味着用户将不能用后退按钮转到该页面



`URLUtils.toString()`

返回一个`DOMString`，包含整个URL。它和读取`URLUtils.href`的效果相同。但是用它是不能够修改`Location`的值的



## DOMString

`DOMString`是一个`UTF-16`字符串。由于JavaScript已经使用了这样的字符串，所以`DOMString`直接映射到一个`String`

将`null`传递给接受`DOMString`的方法或参数时通常会把其`stringifies`为`null`



## 数组的filter方法

### Array.prototype.filter()

`filter()`方法创建一个新数组，其包含通过所提供函数实现的测试的所有元素

### 语法

```javascript
var new_array = arr.filter(callback[, thisArg])
```

### 参数

`callback`

用来测试数组的每个元素的函数。调用时使用参数`(element, index, array)`。返回`true`表示保留该元素（通过测试），`false`则不保留

`thisArg`

可选。执行`callback`时的用于`this`的值

### 返回值

一个新的通过测试的元素的集合的数组

### 描述

`filter`为数组中的每个元素调用一次`callback`函数，并利用所有使得`callback`返回`true`或等价于`true`的值的元素创建一个新数组。`callback`只会在已经赋值的索引上被调用，对于那些已经被删除或者从未被赋值的索引不会被调用。那些没有通过`callback`测试的元素会被跳过，不会被包含在新数组中。



`callback`被调用时传入三个参数：

+ 元素的值
+ 元素的索引
+ 被遍历的数组



如果为`filter`提供一个`thisArg`参数，则它会被作为`callback`被调用时的`this`值。否则，`callback`的`this`值在非严格模式下将是全局对象，严格模式下为`undefined`



`filter`不会改变原数组，它返回过滤后的新数组



`filter`遍历的元素范围在第一次调用`callback`之前就已经确定了。在调用`filter`之后被添加到数组中的元素不会被`filter`遍历到。如果已经存在的元素被改变了，则它们传入`callback`的值是`filter`遍历到它们那一刻的值。被删除或从来未被赋值的元素不会被遍历到



## Window.localStorage

只读的`localStorage`允许你访问一个`Document`的远端`(origin)`对象`Storage`；数据存储为跨浏览器会话。`localStorage`类似于`sessionStorage`。区别在于，数据存储在`localStorage`是无期限的，而当页面会话结束----也就是说当页面被关闭时，数据存储在`sessionStorage`会被清除。

应注意无论数据存储在`localStorage`还是`sessionStorage`，**它们都特定于页面的协议**



## Storage

作为`Web Storage API`接口，`Storage`提供了访问特定域名下的会话存储`(session storage)`或本地存储`(local storage)`的功能，例如，可以添加，修改或删除存储的数据项

如果你想要操作一个域名的会话存储`(session storage)`，可以使用`Window.sessionStorage`；如果想要操作一个域名的本地存储`(local storage)`，可以使用`Window.localStorage`



### 属性

`Storage.length`只读

返回一个整数，表示存储在`Storage`对象中的数据项数量



### 方法

`Storage.key()`

该方法接受一个数值n作为参数，并返回存储中的第n个键名



`Storage.getItem()`

该方法接受一个键名作为参数，返回键名对应的值



`Storage.setItem()`

该方法接受一个键名和值作为参数，将会把键值对添加到存储中，如果键名存在，则更新其对应的值



`Storage.removeItem()`

该方法接受一个键名作为参数，并把该键名从存储中删除



`Storage.clear()`

调用该方法会清空存储中的所有键名



## JSON

JSON是用于存储和传输数据的格式

JSON通常用于服务端向网页传递数据

| 函数             | 描述                                     |
| ---------------- | ---------------------------------------- |
| JSON.parse()     | 用于将一个JSON字符串转换为JavaScript对象 |
| JSON.stringify() | 用于将JavaScript值转换为JSON字符串       |



## 侦听器watch

vue通过`watch`选项提供了一个来响应数据变化的方法。当需要在数据变化时执行异步或开销较大的操作时，这个方式最有用

官网直接复制下来的

```javascript
var vm = new Vue({
  data: {
    a: 1,
    b: 2,
    c: 3,
    d: 4,
    e: {
      f: {
        g: 5
      }
    }
  },
  watch: {
    a: function (val, oldVal) {
      console.log('new: %s, old: %s', val, oldVal)
    },
    // 方法名
    b: 'someMethod',
    // 深度 watcher
    c: {
      handler: function (val, oldVal) { /* ... */ },
      deep: true
    },
    // 该回调将会在侦听开始之后被立即调用
    d: {
      handler: function (val, oldVal) { /* ... */ },
      immediate: true
    },
    e: [
      function handle1 (val, oldVal) { /* ... */ },
      function handle2 (val, oldVal) { /* ... */ }
    ],
    // watch vm.e.f's value: {g: 5}
    'e.f': function (val, oldVal) { /* ... */ }
  }
})
vm.a = 2 // => new: 2, old: 1
```

