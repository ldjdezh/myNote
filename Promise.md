# Promise

Promise是为了解决js中的回调地狱问题提出的解决方法



```javascript
var fs = require('fs')

// 创建一个Promise对象，这个对象要传入一个函数
// resolve函数是用来接收成功结果的
// reject函数是用来接收失败结果的
// 这个函数会自动执行
var p1 = new Promise(function (resolve, reject) {
    fs.readFile('./data/a.txt', 'utf8', function (err, data) {
        if (err) {
            reject(err)
        } else {
            resolve(data)
        }
    })
})

var p2 = new Promise(function (resolve, reject) {
    fs.readFile('./data/b.txt', 'utf8', function (err, data) {
        if (err) {
            reject(err)
        } else {
            resolve(data)
        }
    })
})

var p3 = new Promise(function (resolve, reject) {
    fs.readFile('./data/c.txt', 'utf8', function (err, data) {
        if (err) {
            reject(err)
        } else {
            resolve(data)
        }
    })
})

// then函数有两个参数
// 第一个参数是resolve函数
// 第二个参数是reject函数
// then可以链式编程
p1.then(function (data) {
    console.log(data)
    return p2
}, function (err) {
    console.log(err)
    return p2
}).then(function (data) {
    console.log(data)
    return p3
}, function (err) {
    console.log(err)
    return p3
}).then(function (data) {
    console.log(data)
}, function (err) {
    console.log(err)
})
```

