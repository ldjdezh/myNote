# Vue组件及Vue-router路由

## 全局组件

基本示例

```javascript
// 定义一个名为button-counter的全局组件
Vue.component('button-counter', {
    data: function() {
        return {
            count: 0
        }
    },
    template:'<button v-on:click="count++">You clicked me {{ count }} times.</button>'
})
```

组件是可复用的Vue实例，且带有一个名字，在这个例子中是`<button-counter>`，我们可以在一个通过`new Vue`创建的Vue根实例中，把这个组件作为自定义元素来使用

因为组件是可复用的Vue实例，所以它们与`new Vue`接收相同的选项，例如`data`,`computed`,`watch`,`methods`以及生命周期钩子等。仅有的例外是像`el`这样根实例特有的选项。

**data必须是一个函数**

一个组件的`data`选项必须是一个函数，因此每个实例可以维护一份被返回对象的独立的拷贝



## 局部组件

```javascript
var ComponentA = {}
var ComponentB = {}
var ComponentC = {}
```

然后在`components`选项中定义你想要使用的组件

```javascript
new Vue({
    el: '#app',
    components: {
        'component-a': ComponentA,
        'component-b': ComponentB
    }
})
```

对于`components`对象中的每个属性来说，其属性名就是自定义元素的名字，其属性值就是这个组件的选项对象。

注意**局部注册的组件在其子组件中不可用**，例如，如果你希望`ComponentA`在`ComponentB`中可以，则你需要这样写：

```javascript
var ComponentA = {}

var ComponentB = {
    components: {
        'component-a': ComponentA
    }
}
```



## template属性

这个属性用于渲染组件，在入口组件中使用，用来做到极致化组件，例如，所以组件的根，根组件可以用`template`属性渲染

```javascript
new Vue({
    el: '#app',
    template: '<app></app>',
    components: {
        App
    }
})
```



## vue-router使用

安装

```shell
npm install vue-router
```



使用

HTML

```html
<script src="https://unpkg.com/vue/dist/vue.js"></script>
<script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>

<div id="app">
  <h1>Hello App!</h1>
  <p>
    <!-- 使用 router-link 组件来导航. -->
    <!-- 通过传入 `to` 属性指定链接. -->
    <!-- <router-link> 默认会被渲染成一个 `<a>` 标签 -->
    <router-link to="/foo">Go to Foo</router-link>
    <router-link to="/bar">Go to Bar</router-link>
  </p>
  <!-- 路由出口 -->
  <!-- 路由匹配到的组件将渲染在这里 -->
  <router-view></router-view>
</div>
```



JavaScript

```javascript
// 0. 如果使用模块化机制编程，导入Vue和VueRouter，要调用 Vue.use(VueRouter)

// 1. 定义（路由）组件。
// 可以从其他文件 import 进来
const Foo = { template: '<div>foo</div>' }
const Bar = { template: '<div>bar</div>' }

// 2. 定义路由
// 每个路由应该映射一个组件。 其中"component" 可以是
// 通过 Vue.extend() 创建的组件构造器，
// 或者，只是一个组件配置对象。
// 我们晚点再讨论嵌套路由。
const routes = [
  { path: '/foo', component: Foo },
  { path: '/bar', component: Bar }
]

// 3. 创建 router 实例，然后传 `routes` 配置
// 你还可以传别的配置参数, 不过先这么简单着吧。
const router = new VueRouter({
  routes // （缩写）相当于 routes: routes
})

// 4. 创建和挂载根实例。
// 记得要通过 router 配置参数注入路由，
// 从而让整个应用都有路由功能
const app = new Vue({
  router
}).$mount('#app')

// 现在，应用已经启动了！
```



## &lt;router-link&gt;

`<router-link>`组件支持用户在具有路由功能的应用中（点击）导航。通过`to`属性指定目标地址，默认渲染成带有正确链接的`<a>`标签，可以通过配置`tag`属性生成别的标签。另外，当目标路由成功激活时，链接元素自动设置一个表示激活的`CSS`类名

### Props

+ to
  + 类型：`string|Location`
  + required

表示目标路由的链接。当被点击后，内部会立刻把`to`的值传到`router.push()`，所以这个值可以是一个字符串或者是描述目标位置的对象



```html
<!-- 字符串 -->
<router-link to="home">Home</router-link>
<!-- 渲染结果 -->
<a href="home">Home</a>

<!-- 使用 v-bind 的 JS 表达式 -->
<router-link v-bind:to="'home'">Home</router-link>

<!-- 不写 v-bind 也可以，就像绑定别的属性一样 -->
<router-link :to="'home'">Home</router-link>

<!-- 同上 -->
<router-link :to="{ path: 'home' }">Home</router-link>

<!-- 命名的路由 -->
<router-link :to="{ name: 'user', params: { userId: 123 }}">User</router-link>

<!-- 带查询参数，下面的结果为 /register?plan=private -->
<router-link :to="{ path: 'register', query: { plan: 'private' }}">Register</router-link>
```



+ tag
  + 类型：`string`
  + 默认值：`"a"`

有时候想要`<router-link>`渲染成某种标签，例如`<li>`。于是我们使用`tag`prop类指定成某种标签，同样它还是会监听点击，触发导航。

```html
<router-link to="/foo" tag="li">foo</router-link>
<!-- 渲染结果 -->
<li>foo</li>
```



+ active-class
  + 类型：`string`
  + 默认值：`"router-link-active"`

设置链接激活时使用的`CSS`类名。默认值可以通过路由的构造选项`linkActiveClass`来全局配置

```javascript
new VueRouter({
    // 通过修改默认的router-link-active样式
    linkActiveClass: css类名
})
```



+ exact

  + 类型：`boolean`
  + 默认值：`false`

  “是否激活”默认类名的依据是inclusive match（全包含匹配）。举个例子，如果当前的路径是`/a`开头的，那么`<router-link to="/a">`也会被设置CSS类名

  按照这个规则，每个路由都会激活`<router-link to="/">`！，想要链接使用“exact 匹配模式”，则使用`exact`属性

```html
<!-- 这个链接只会在地址为 / 的时候被激活 -->
<router-link to="/" exact>
```



### 将激活class应用在外层元素

有时候我们要让激活class应用在外层元素，而不是`<a>`标签本身，那么可以用`<router-llink>`渲染外层元素，包裹着内层的原生`<a>`标签

```html
<router-link tag="li" to="/foo">
  <a>/foo</a>
</router-link>
```

在这种情况下，`<a>`将作为真实的链接（它会获得正确的`href`的），而“激活时的CSS类名”则设置到外层的`<li>`。