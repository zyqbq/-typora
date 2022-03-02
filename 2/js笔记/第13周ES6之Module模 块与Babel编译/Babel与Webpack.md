# Babel 是什么

### 1.认识 Babel

   官网：https://babeljs.io/

   在线编译：https://babeljs.io/repl

Babel 是 JavaScript 的编译器，用来将 ES6 的代码，转换成 ES6 之前的代码

### 2.使用 Babel

try it out

### 3.解释编译结果

   Babel 本身可以编译 ES6 的大部分语法，比如 let、const、箭头函数、类

   但是对于 ES6 新增的 API，比如 Set、Map、Promise 等全局对象，以及一些定义在全局对象上的方法（比如 Object.assign/Array.from）都不能直接编译，需要借助其它的模块

   Babel 一般需要配合 Webpack 来编译模块语法

# 使用Babel前的准备工作

### 1.什么是 Node.js 和 npm

   Node.js 是个**平台或者工具**，对应浏览器

   后端的 JavaScript = ECMAScript + IO + File + ...等服务器端的操作



   npm：node 包管理工具



   npm install

node中文官网: https://nodejs.org/zh-cn/

使用长期稳定版

### 2.安装 Node.js

安装完成，然后测试

   node -v

   npm -v



### 3.初始化项目

   npm init -> 得到package.json



### 4.安装 Babel 需要的包

最新版

   npm install --save-dev @babel/core @babel/cli

老版

   *npm install --save-dev @babel/core@7.11.0 @babel/cli@7.10.5

删除后重新安装 

   **npm install**

# 使用 Babel 编译 ES6 代码

**详细教程：**https://babeljs.io/setup

1.执行编译的命令

在 package.json 文件中添加执行 babel 的命令

```js
"scripts": {
    "build": "babel src -d dist"
  },
```



创建src：源目录 

放入要解析的文件

   babel src -d dist

   babel src --out-dir dist





2.Babel 的配置文件

   .babelrc



cmd**输入**

**npm install @babel/preset-env@7.11.0 --save-dev**



在src文件中创建配置文件 .babelrc，并写入

```js
   {
 "presets": ["@babel/preset-env"]
   }
```

**执行命令**

   npm run build

# Webpack 是什么

### 1.认识 Webpack

   webpack 是**静态模块打包器**，当 webpack 处理应用程序时，会将所有这些模块打包成一个或多个文件



   import './module.js'

   require('./module.js')



### 2.什么是 Webpack

   **模块**

   webpack 可以处理 js/css/图片、图标字体等单位



   **静态**

   开发过程中存在于**本地**的 js/css/图片/图标字体等文件，就是静态的

   动态的内容，webpack没办法处理，**只能处理静态**的

# Webpack 初体验

### 1.初始化项目

   npm init

​	wabpage2：和包名不能冲突



### 2.安装 webpack 需要的包

cmd执行

```js
npm install --save-dev webpack-cli@3.3.12 webpack@4.44.1
```

### 3.配置 webpack

webpack官网地址: https://www.webpackjs.com/

   创建文件：webpack.config.js

   放入

```js
const path = require('path');

module.exports = {
  mode: 'development',
  entry: './src/index.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: '[name].js'
  }
};
```

package.json中修改

```js
"scripts": {
    "webpack": "webpack --config webpack.config.js"
  },
```

创建src目录：放入index和module文件

### 4.编译并测试

   cmd中执行

npm run webpack

# entry 和 output

### 1安装包：本身有json文件的情况

cmd=>npm install

### 2.entry



```js
const path = require('path');

module.exports = {
  mode: 'development',
  entry: './src/index.js',//单入口文件
    entry: {
        main: './src/index.js',
        search: './src/search.js'
  },//多入口文件
  output: {
      //输出目录路径：绝对路径
    path: path.resolve(__dirname, 'dist'),
    filename: '[name].js'//按路径原名称打包：main，search
  }
};
```

# loader

## 1.什么是loader

webpack js/css/图片
loader让webpack能够去处理那些非JS|文件

## 2.babel-loader

联通webpack和babel

webpack（大，模块兼容问题）中使用babel（小，es6兼容问题）

先babel在文本pack打包

### 1安装webpack的包

npm init

wabpage2

npm install --save-dev webpack-cli@3.3.12 webpack@4.44.1

创建文件：webpack.config.js

```js
const path = require('path');
module.exports = {
  mode: 'development',//开发模式查看，一般不写**删除
  entry: './src/index.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js'
  }
};
```

package.json中修改

```js
"scripts": {
    "webpack": "webpack --config webpack.config.js"
  },
```

创建src目录：放入index和module文件

2配置bable

```js
npm install --save-dev babel-loader@8.1.0 @babel/core@7.11.0 @babel/preset-env@7.11.0
```
在创建配置文件 .babelrc，并写入

```js
   {
 "presets": ["@babel/preset-env"]
   }
```

3.配置 babel-loader

   官网：https://www.webpackjs.com/loaders/

webpack.config.js

```js
const path = require('path');

module.exports = {
  mode: 'development',
  entry: {
    index: './src/index.js'
  },
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: '[name].js'
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        loader: 'babel-loader'
      }
    ]
  }
};
```

5.引入 core-js：bable垫片（帮助实现一些不能转换低版本es的代码

   编译新增 API

npm install --save-dev core-js@3.6.5

js最前面加

   ```js
   import "core-js/stable";
   ```

npm run webpack

# plugins

### 1.什么是 plugins

插件

   loader 被用于帮助 webpack 处理各种模块，而插件则可以用于执行范围更广的任务



插件官网：https://www.webpackjs.com/plugins/

### 2.html-webpack-plugin

先配置好webpack-plugin

   npm install --save-dev html-webpack-plugin@4.3.0

### 3.配置 html-webpack-plugin 插件

插件引入

webpack.config.js第二行

```js
const HtmlWebpackPlugin = require('html-webpack-plugin');
```

webpack.config.js中加入

js文件都放在src目录下

### 单入口

```js
 ,
plugins: [
    //单入口
    new HtmlWebpackPlugin({
      template: './index.html',
        filename: 'index.html'
    })
    ]
```

### 多入口

```js
const path = require('path');

module.exports = {
  mode: 'development',
  entry: './src/index.js',//单入口文件
    entry: {
        index: './src/index.js',
        search: './src/search.js'
  },//多入口文件
  output: {
      //输出目录路径：绝对路径
    path: path.resolve(__dirname, 'dist'),
    filename: '[name].js'//按路径原名称打包：main，search
  }
};
```

```js
 ,
plugins: [
    new HtmlWebpackPlugin({
    //文件复制
      template: './index.html',
    //命名，不指定默认都是index
      filename: 'index.html',
    //绑定js
      chunks: ['index'],
      minify: {
        // 删除 index.html 中的注释
        removeComments: true,
        // 删除 index.html 中的空格
        collapseWhitespace: true,
        // 删除各种 html 标签属性值的双引号
        removeAttributeQuotes: true
      }
    }),
    new HtmlWebpackPlugin({
    //文件复制
      template: './search.html',
    //命名
      filename: 'search.html',
    //绑定js
      chunks: ['search']
    })
  ]
```

### 执行

npm run webpack

给html匹配js文件

# 处理css样式

html中外链式引入css文件

```js
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <link rel="stylesheet" href="./index.css">
</head>
```

### 1.文件位置

src中放入css和js文件，html放在外面

js中

```js
import './index.css';
```

### 安装webpack

npm init

。。。



### 安装plugin

### 安装css-loader

用来识别css的

用来导入css文件

```js
npm install --save-dev css-loader@4.1.1
```

webpack.config.js中配置

```js
 module: {
    rules: [
      {
        test: /\.css$/,
        // loader: 'css-loader'
        use: ['style-loader', 'css-loader']//引入多个loader，css-loader用来查找css文件，从右向左执行
      }
    ]
  },
```

配置style-loader

内嵌式

![image-20220115154518174](C:\Users\钟彦\AppData\Roaming\Typora\typora-user-images\image-20220115154518174.png)

```js
npm install --save-dev style-loader@1.2.1
```

## 方法二

link外链引入

![image-20220115154530416](C:\Users\钟彦\AppData\Roaming\Typora\typora-user-images\image-20220115154530416.png)

css提取引入

```js
npm install --save-dev mini-css-extract-plugin@0.9.0
```

```js
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
```

```js
plugins: [
    new HtmlWebpackPlugin({
      template: './index.html',
      filename: 'index.html'
    }),
    //css提取
    new MiniCssExtractPlugin({
       // 和entry目录一致
      filename: 'css/[name].css'
    })
  ]
```

```js
module: {
    rules: [
      {
        test: /\.css$/,
        // loader: 'css-loader'
        // use: ['style-loader', 'css-loader']
          //下方代码为minicss的，替换了style，loader
        use: [MiniCssExtractPlugin.loader, 'css-loader']
      }
    ]
  },
```

# 功能总结



webpack

打包js文件到dis目录下

bable-loader：

让高版本的es转换为低版本的

html-webpack-plugin

复制html到dist目录中同时匹配js文件

css-loader：

识别导入css文件的

loader：

把不认识的变成认识的

# file-loader

## 处理 CSS 文件中的图片

**如果是外部的资源，是不需要考虑 webpack 的，只有本地的图片才需要被 webpack 处理**

处理的逻辑

1，复制图片从源文件中复制到dist目录中

2，修改css中的图片地址到新地址

### 安装file-loader

npm install --save-dev file-loader@6.0.0



```js
module.exports = {
  mode: 'development',
  entry: './src/index.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: '[name].js'
  },
    //使用loader
  module: {
    rules: [
        //css-loader并且修改minicss默认路径
      {
        test: /\.css$/,
        use: [
          {
            loader: MiniCssExtractPlugin.loader,//配置minicss时修改一下参数
            options: {
              publicPath: '../'//他以为路径和minicss的一样，实际因为创建了css的文件夹，需要回退
            }
          },
          'css-loader'
        ]
      },
      {
        test: /\.(jpg|png|gif)$/,
        use: {
          loader: 'file-loader',
          options: {
            name: 'img/[name].[ext]'//使用图片本身的文件名和后缀以及放在img里面
          }
        }
      }
    ]
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: './index.html',
      filename: 'index.html'
    }),
    new MiniCssExtractPlugin({
      filename: 'css/[name].css'
    })
  ]
};
```



# html-withimg-loader

## 处理 HTML 中的图片

npm install --save-dev html-withimg-loader@0.1.16

```js
module: {
    rules: [
      {//css
        test: /\.css$/,
        use: [
          {
            loader: MiniCssExtractPlugin.loader,
            options: {
              publicPath: '../'
            }
          },
          'css-loader'
        ]
      },
      {//file-loader
        test: /\.(jpg|png|gif)$/,
        use: {
          loader: 'file-loader',
          options: {
            name: 'img/[name].[ext]',
            esModule: false//不按es6模块导入
          }
        }
      },
      {//html-withimg-loader
        test: /\.(htm|html)$/,
        loader: 'html-withimg-loader'
      }
    ]
  },
```

# 使用js引入图片

index.js中

./当前目录

```js
import img from './img/截图.jpg';
console.log(img);
```

使用fileloader引入图片，和css一样的方式

# url-loader

结合fileloader处理图片，区别是可以转换为base64位图片，减少文件

### 注意

( 1 ) url-loader配置了**limit属性**时,需要同时安装file-loader ,那么，当图片大于limit
属性值时,就可以使用file-loader处理图片,避兔代码报错
( 2 ) url-loader没有配置limit属性时，可以不用安装file-loader

| file-loader         | css img  |
| ------------------- | -------- |
| html-withimg-loader | html img |
| file-loader         | js img   |



   npm install --save-dev url-loader@4.1.0

```js
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');

module.exports = {
  mode: 'development',
  entry: './src/index.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: '[name].js'
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          {
            loader: MiniCssExtractPlugin.loader,
            options: {
              publicPath: '../'
            }
          },
          'css-loader'
        ]
      },
      {
        test: /\.(jpg|png|gif)$/,
        use: {
          loader: 'url-loader',//这里由file-loader修改为url-loader
          options: {
            name: 'img/[name].[ext]',
            esModule: false,
            limit: 3000//这里加入图片限制单位1000位1kb
          }
        }
      }
    ]
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: './index.html',
      filename: 'index.html'
    }),
    new MiniCssExtractPlugin({
      filename: 'css/[name].css'
    })
  ]
};

```

# 慕课网总结

https://climg.mukewang.com/607a960709df9e4114022510.png



# webpack-dev-server

## 使用 webpack-dev-server 搭建开发环境

npm install --save-dev webpack-dev-server@3.11.0

官网：https://www.webpackjs.com/configuration/dev-server/



```js
"scripts": {
    "webpack": "webpack",
    "dev": "webpack-dev-server --open chrome"
  },
```

npm run dev

# 总结

bale

编译es6代码

webpack

处理模块语法

bale不能编译es6新增的api，需要其他辅助
