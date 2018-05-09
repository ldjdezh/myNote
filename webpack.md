# webpack

## 安装

全局安装

```shell
npm install --global webpack
```



本地安装（推荐）

```shell
npm install --save-dev webpack
```



## 初次使用

不知道为什么安装webpack4.x以上的版本，我不会用，只好按教程的webpack版本来，教程的webpack的版本是**3.8.1**，一开始全局安装，然后进入到项目目录下

html文件

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    
    <!-- bundle.js是webpack编译生成的 -->
    <script src="js/bundle.js"></script>
</body>
</html>
```



两个js文件，里面使用了模块化的引入方式，在浏览器中是不行的

```javascript
// foo.js
module.exports = function() {
    console.log('hello webpack')
}

// -------------------------

// main.js
var foo = require('./foo')

foo()

// -------------------------
```



使用webpack编译命令，生成bundle.js

```shell
webpack js\main.js js\bundle.js
```

生成了bundle.js后，就可以在浏览器使用了，有了webpack我们就可以愉快的在浏览器使用模块化编程了

### 总结

准备目录结构

打包：

```shell
webpack 模块入口文件路径 模块出口文件路径
```

最后记得把index.html文件中的脚本引用改为打包之后的结果文件路径



## 划分src和dist目录

+ 把源码存储到src目录中
+ 把打包的结构存储到dist目录中



## 配置webpack.config.js

最基本的配置项

```javascript
// 该文件最终要在node环境下执行，可以使用node模块
const path = require('path')

// 导出一个具有特殊属性配置的对象
module.exports = {
    entry: './src/main.js', // 入口模块文件路径
    output: {
        // 出口文件模块所属目录
        // path必须是一个绝对路径
        path: path.join(__dirname,'./dist/'),
        // 打包的结果文件名
        filename: 'bundle.js'
    }
}
```



打包

```shell
# webpack会自动读取webpack.config.js作为默认配置文件
# 也可以通过 --config 参数来手动指定配置文件
webpack
```



## 本地安装运行的问题

本地安装的命令

```shell
// 指定下版本号
npm i -D webpack@3.8.1
```

对于本地安装的webpack必须要配置npm scripts来使用

npm scripts这个配置的作用就是给命令起别名

```json
{
  "name": "webpack-demo2",
  "version": "1.0.0",
  "description": "",
  "main": "webpack.config.js",
  "dependencies": {
    "webpack": "^3.8.1"
  },
  "devDependencies": {},
    // 配置scripts
  "scripts": {
    "build": "webpack"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}

```

然后通过以下命令来打包

```shell
npm run build
```



## webpack监视编译

在npm scripts配置

```shell
"watch-build": "webpack --watch"
```

运行

```shell
npm run watch-build
```





## EcmaScript6模块规范

导出默认成员

```javascript
// 默认成员只能有一个
export default 成员
```



加载默认成员

```javascript
import xxx from '模块标识'
```



导出多个成员

```javascript
export const a = 123
export const b = 456
export function fn() {
    console.log('fn')
}
```



按需加载指定的多个成员

```javascript
import {a,b} from '模块标识'
```



一次性加载所有的导出成员

```javascript
import * as xxx from '模块标识'
```



## 打包CSS

安装加载器

```shell
# css-loader的作用把css文件转为js模块
# style-loader的作用是动态创建style节点插入到head中

npm install --save-dev style-loader css-loader
```



在webpack.config.js中配置

```javascript
const path = require('path')

module.exports = {
    entry: './src/main.js',
    output: {
        path: path.join(__dirname, './dist/'),
        filename: 'bundle.js'
    },
    
    // 配置这些内容
    module: {
        rules: [
            {
                test: /\.css$/,
                use: [
                    // 这些是有顺序的，不能写错
                    'style-loader',
                    'css-loader'
                ]
            }
        ]
    }
}
```

测试

编写main.css

```css
body{
    background-color: pink;
}
```



main.js引入main.css

```js
import './main.css'
```



然后打包

运行原理，打包css就是把css文件变成一个js模块，在运行时，动态创建`<style>`节点插入到html文件的`<head>`标签中



## 加载图片

安装

```shell
npm install --save-dev file-loader
```



webpack.config.js配置

```javascript
 module: {
        rules: [
            {
                test: /\.(png|svg|jpg|gif)$/,
                use: [
                    'file-loader'
                ]
            }
        ]
    }
```



## HtmlWebpackPlugin

该插件的作用就是把index.html打包到你的bundle.js文件所在目录

bundle.js到哪里，index.html就到哪里

同时，还会自动引用`script`标签，引用的资源名取决于你的bundle叫什么

安装

```shell
npm i -D html-webpack-plugin
```



配置

```javascript
module.exports = {
    entry: './src/main.js',
    output: {
        path: path.join(__dirname, './dist/'),
        filename: 'bundle.js'
    },
    
    // 这里就是配置项
    plugins: [
        new htmlWebpackPlugin({
            template: './index.html'
        })
    ],
    module: {
        rules: [
            {
                test: /\.css$/,
                use: [
                    'style-loader',
                    'css-loader'
                ]
            },
            {
                test: /\.(png|svg|jpg|gif)$/,
                use: [
                    'file-loader'
                ]
            }
        ]
    }
}
```



## 打包less

安装

```shell
# 要想打包less,必须要先安装less
npm i -D less
npm install less-loader --save-dev
```



webpack.config.js配置

```javascript
module: {
        rules: [
            {
                test: /\.css$/,
                use: [
                    'style-loader',
                    'css-loader'
                ]
            },
            {
                test: /\.(png|svg|jpg|gif)$/,
                use: [
                    'file-loader'
                ]
            },
            
            // 配置less
            {
                test: /\.less$/,
                use: [
                    'style-loader',
                    'css-loader',
                    'less-loader'
                ]
            }
        ]
    }
```



## webpack-dev-server

监视代码改变，自动打包，打包完毕会自动刷新浏览器的工具

安装

```shell
npm i -D webpack-dev-server@2.9.4
```



配置

```javascript
module.exports = {
    entry: './src/main.js',
    output: {
        path: path.join(__dirname, './dist/'),
        filename: 'bundle.js'
    },
    plugins: [
        new htmlWebpackPlugin({
            template: './index.html'
        })
    ],
    
    // 配置webpack-dev-server的www目录
    devServer: {
        contentBase: './dist'
    },
    module: {
        rules: [
            {
                test: /\.css$/,
                use: [
                    'style-loader',
                    'css-loader'
                ]
            },
            {
                test: /\.(png|svg|jpg|gif)$/,
                use: [
                    'file-loader'
                ]
            },
            {
                test: /\.less$/,
                use: [
                    'style-loader',
                    'css-loader',
                    'less-loader'
                ]
            }
        ]
    }
}
```



配置num scripts

```json
{
  "name": "webpack-demo3",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "build": "webpack",
    "watch-build": "webpack --watch",
      
      // 配置webpack-dev-server
    "dev": "webpack-dev-server --open"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "css-loader": "^0.28.11",
    "file-loader": "^1.1.11",
    "html-webpack-plugin": "^3.2.0",
    "less": "^3.0.2",
    "less-loader": "^4.1.0",
    "style-loader": "^0.21.0",
    "webpack": "^3.8.1",
    "webpack-dev-server": "^2.9.4"
  }
}

```



## 转换ES6

### babel (转换语法)

安装

```shell
npm install --save-dev babel-loader babel-core babel-preset-env
```

配置

```javascript
module: {
  rules: [
    {
      test: /\.js$/,
      exclude: /(node_modules|bower_components)/, //不转node_modules里面的代码
      use: {
        loader: 'babel-loader',
        options: {
          presets: ['env']
        }
      }
    }
  ]
}
```



### babel-polyfill

用来转换ES6的API

安装

```shell
npm i -D babel-polyfill
```

配置

```javascript
entry: ['babel-polyfill','./src/main.js']
```

这样就会在打包的结果中提供一个垫脚片用来兼容低版本浏览器中不支持的API



### 配置transform-runtime来解决代码重复问题

在打包过程中，babel会在某些包提供一些工具函数，而这些工具函数可能会重复的出现在多个模块。这样的话就会导致打包体积过大，所有babel提供了一个**babel-transform-runtime**来解决这个打包体积过大的问题

安装

```shell
npm install babel-plugin-transform-runtime --save-dev

npm install babel-runtime --save
```

配置

```javascript
module.exports = {
    entry: ['babel-polyfill','./src/main.js'],
    output: {
        path: path.join(__dirname, './dist/'),
        filename: 'bundle.js'
    },
    plugins: [
        new htmlWebpackPlugin({
            template: './index.html'
        })
    ],
    devServer: {
        contentBase: './dist'
    },
    module: {
        rules: [
            {
                test: /\.css$/,
                use: [
                    'style-loader',
                    'css-loader'
                ]
            },
            {
                test: /\.(png|svg|jpg|gif)$/,
                use: [
                    'file-loader'
                ]
            },
            {
                test: /\.less$/,
                use: [
                    'style-loader',
                    'css-loader',
                    'less-loader'
                ]
            },
            {
                test: /\.js$/,
                exclude: /(node_modules|bower_components)/, //不转node_modules里面的代码
                use: {
                    loader: 'babel-loader',
                    options: {
                        presets: ['env'],
                        
                        // 这里配置
                        plugins: ['transform-runtime']
                    }
                }
            }
        ]
    }
}
```



## vue-loader

安装

```shell
npm i -D vue-loader@13.5.0 vue-template-compiler
```



## 配置cacheDirectory提升编译事件

可以提高打包效率

```javascript
module.exports = {
    entry: ['babel-polyfill','./src/main.js'],
    output: {
        path: path.join(__dirname, './dist/'),
        filename: 'bundle.js'
    },
    plugins: [
        new htmlWebpackPlugin({
            template: './index.html'
        })
    ],
    devServer: {
        contentBase: './dist'
    },
    module: {
        rules: [
            {
                test: /\.css$/,
                use: [
                    'style-loader',
                    'css-loader'
                ]
            },
            {
                test: /\.(png|svg|jpg|gif)$/,
                use: [
                    'file-loader'
                ]
            },
            {
                test: /\.less$/,
                use: [
                    'style-loader',
                    'css-loader',
                    'less-loader'
                ]
            },
            {
                test: /\.js$/,
                exclude: /(node_modules|bower_components)/, //不转node_modules里面的代码
                use: {
                    loader: 'babel-loader',
                    options: {
                        // 在这里配置就可以了
                        cacheDirectory: true,
                        presets: ['env'],
                        plugins: ['transform-runtime']
                    }
                }
            },
            {
                test: /\.vue$/,
                use: [
                    'vue-loader'
                ]
            }
        ]
    }
}
```



## 加载使用第三方包

```javascript
module.exports = {
    entry: ['babel-polyfill','./src/main.js'],
    output: {
        path: path.join(__dirname, './dist/'),
        filename: 'bundle.js'
    },
    plugins: [
        new htmlWebpackPlugin({
            template: './index.html'
        })
    ],
    devServer: {
        contentBase: './dist'
    },
    // 在这里声明，不加载第三方包
    externals: {
        // key 就是包名
        // value 是全局的jquery导出的接口
        jquery: 'jQuery'
    },
    module: {
        rules: [
            {
                test: /\.css$/,
                use: [
                    'style-loader',
                    'css-loader'
                ]
            },
            {
                test: /\.(png|svg|jpg|gif)$/,
                use: [
                    'file-loader'
                ]
            },
            {
                test: /\.less$/,
                use: [
                    'style-loader',
                    'css-loader',
                    'less-loader'
                ]
            },
            {
                test: /\.js$/,
                exclude: /(node_modules|bower_components)/, //不转node_modules里面的代码
                use: {
                    loader: 'babel-loader',
                    options: {
                        cacheDirectory: true,
                        presets: ['env'],
                        plugins: ['transform-runtime']
                    }
                }
            },
            {
                test: /\.vue$/,
                use: [
                    'vue-loader'
                ]
            }
        ]
    }
}
```



## --save 和 --save-dev的区别

我们把开发工具相关的依赖信息保存到`devDependencies`选项中。把核心依赖的依赖信息保存到`dependencies`选项中。

这样做的话，就把开发依赖和核心依赖分开了

最后项目上线时，我们只需要安装`dependencies`选项中的包

我们可以使用命令只安装`dependencies`中的包

```shell
npm i --production
```



## contentBase配置

webpack-dev-server打包后的东西其实是存储在内存中的虚拟目录的，所有把`contentBase: './'`，这个虚拟目录就是当前目录了，这样就可以解决一下资源的加载问题

配置

```javascript
module.exports = {
    entry: ['babel-polyfill','./src/main.js'],
    output: {
        path: path.join(__dirname, './dist/'),
        filename: 'bundle.js'
    },
    plugins: [
        new htmlWebpackPlugin({
            template: './index.html'
        })
    ],
    devServer: {
        // 在这里配置
        contentBase: './'
    },
    externals: {
        jquery: 'jQuery'
    },
    module: {
        rules: [
            {
                test: /\.css$/,
                use: [
                    'style-loader',
                    'css-loader'
                ]
            },
            {
                test: /\.(png|svg|jpg|gif)$/,
                use: [
                    'file-loader'
                ]
            },
            {
                test: /\.less$/,
                use: [
                    'style-loader',
                    'css-loader',
                    'less-loader'
                ]
            },
            {
                test: /\.js$/,
                exclude: /(node_modules|bower_components)/, //不转node_modules里面的代码
                use: {
                    loader: 'babel-loader',
                    options: {
                        cacheDirectory: true,
                        presets: ['env'],
                        plugins: ['transform-runtime']
                    }
                }
            },
            {
                test: /\.vue$/,
                use: [
                    'vue-loader'
                ]
            }
        ]
    }
}
```



## 使用vue的常见问题

```javascript
1、vue.runtime.esm.js:574 [Vue warn]: You are using the runtime-only build of Vue where the template compiler is not available. Either pre-compile the templates into render functions, or use the compiler-included build.

答：在webpack中添加如下配置就可以解决：
module.export{
    ...
    resolve:{
        alias:{
            'vue$':'vue/dist/vue.js'
        }
    }
}
```



## 热更新

配置，加上下面的4条语句就行了

```javascript
const path = require('path')
const htmlWebpackPlugin = require('html-webpack-plugin')
+++const webpack = require('webpack')

module.exports = {
    entry: ['babel-polyfill','./src/main.js'],
    output: {
        path: path.join(__dirname, './dist/'),
        filename: 'bundle.js'
    },
    plugins: [
        new htmlWebpackPlugin({
            template: './index.html'
        }),
        +++new webpack.NamedModulesPlugin(),
        +++new webpack.HotModuleReplacementPlugin()
    ],
    devServer: {
        contentBase: './',
        +++hot: true
    },
    externals: {
        // jquery: 'jQuery',
        vue: 'Vue'
    },
    // resolve:{
    //     alias:{
    //         'vue$':'vue/dist/vue.js'
    //     }
    // },
    module: {
        rules: [
            {
                test: /\.css$/,
                use: [
                    'style-loader',
                    'css-loader'
                ]
            },
            {
                test: /\.(png|svg|jpg|gif)$/,
                use: [
                    'file-loader'
                ]
            },
            {
                test: /\.less$/,
                use: [
                    'style-loader',
                    'css-loader',
                    'less-loader'
                ]
            },
            {
                test: /\.js$/,
                exclude: /(node_modules|bower_components)/, //不转node_modules里面的代码
                use: {
                    loader: 'babel-loader',
                    options: {
                        cacheDirectory: true,
                        presets: ['env'],
                        plugins: ['transform-runtime']
                    }
                }
            },
            {
                test: /\.vue$/,
                use: [
                    'vue-loader'
                ]
            }
        ]
    }
}
```



## vue组件的样式独立问题

```html
<template>
    <div id="app">
        <h1>{{ message }}</h1>
        <foo></foo>
    </div>
</template>

<script>
import Foo from "./Foo.vue";

//默认要导出一个对象
export default {
  data() {
    return {
      message: "你好啊"
    };
  },
  components: {
    Foo
  }
};
</script>

<style scoped>
/* scoped 组件样式独立 不影响其他组件 */
h1 {
  color: #96e418;
}
</style>
```

