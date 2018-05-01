# Vue

## 绑定Html class

```html
<div v-bind:class="{css类名: boolean值}"></div>
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

Array.prototype.every()

这个`every`方法测试数组的所有元素是否都通过了指定函数的测试

语法

```javascript
arr.every(callback[, thisArg])
```

参数

`callback`

用来测试每个元素的函数

`thisArg`

执行`callback`时使用的`this`值

