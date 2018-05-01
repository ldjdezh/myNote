# node.js操作mongodb

用node.js操作mongdb可以使用mongoose这个模块，可以很方便的调用mongodb



## 1. 安装mongoose

``` shell
npm i mongoose
```



## 2. 引入mongoose模块

```javascript
var mongoose = require('mongoose')
```



## 3. 进行一些配置

```javascript
// 连接数据库
mongoose.connect('mongodb://localhost/数据库名称')

// 创建图表
var Schema = mongoose.Schema

// 创建集合，设置里面的内容
var userSchema = new Schema({
    username: {
        type: String,
        required: true
    },
    password: {
        type: String,
        required: true
    },
    email: {
        type: String
    }
})

// 创建模型，model函数的第一个参数，要是首字母大写的单数单词
// 这样就会在mongodb中生成一个集合，这个集合的名称就是小写复数的形式
// 例如：User -> users
var User = mongoose.model('User', userSchema)
```



## 4.保存

### 1. 保存数据

```javascript
// 根据模型创建文档，mongodb里面的文档相当于mysql中的记录
var admin = new User({
    username: 'zs',
    password: '123',
    email: '123@qq.com'
})

// 调用文档对象的save方法存储数据，回调函数接受两个参数，错误参数err,返回值ret
admin.save(function(err,ret) {
    if (err) {
        console.log('保存失败')
    } else {
    	console.log('保存成功')
        console.log(ret)
    }
})
```



## 5.查询

### 1. 查询所有数据

```javascript
// 通过模型的find方法就可以查询所有数据
// 返回的是一个数组
User.find(function(err,ret) {
    if(err) {
        console.log('查询失败')
    }else {
        console.log(ret)
    }
})
```



### 2. 根据条件查询数据

```javascript
// 也是使用find方法
// 第一个参数传入一个对象，里面写查询条件
// 第二个参数是一个回调函数
// 返回的查询结果是一个数组
User.find({
    username: 'ls'
},function(err,ret) {
    if(err) {
        console.log('查询失败')
    }else {
        console.log(ret)
    }
})
```



### 3. 查询一条数据

```javascript
// 第一个参数插入查询条件
// 返回结果是一个对象
User.findOne({
    username: 'ls'
},function(err,ret) {
    if(err) {
        console.log('查询失败')
    }else {
        console.log(ret)
    }
})
```



## 6. 删除

### 1.删除数据

```javascript
// 根据传入条件删除数据
User.remove({
    username: 'ls'
},function(err,ret) {
    if(err) {
        console.log('删除失败')
    }else {
        console.log('删除成功')
        console.log(ret)
    }
})
```



## 7.更新
### 1.更新数据

```javascript
// 第一个参数要更新数据的id值
// 第二个参数更新的内容
// 第三个参数回调函数，ret返回值是未更新前的文档
User.findByIdAndUpdate('5ad4b1063141a31ad8499af4',{
    password: 'abc'
},function(err,ret) {
    if(err) {
        console.log('更新失败')
    }else {
        console.log('更新成功')
        console.log(ret)
    }
})
```

