---
title: webpack
date: 2016-12-21 01:39:05
tags: 
- webpack
- react
---


# Webpack

-------------------------------

## webpack

webpack 是一款模块打包的工具，它将根据模块的依赖关系进行`静态分析`，然后将这些模块按照指定的规则生成对应的静态资源。

webpack 是加强版的browserify，webpack解决的是`大型单页应用`的打包问题，同时解决了这些资源的依赖

webpack 将项目中的所有资源，js，png，.css，等等所有的东西可以视为模块

如下图所示，经过webpack 的处理成为静态的资源文件

![Alt text](http://obqvt6b56.bkt.clouddn.com/blog-webpack-what-is-webpack.png)

总结：
> Webpack 是当下最热门的前端资源模块化管理和打包工具。它可以将许多松散的模块按照依赖和规则打包成符合生产环境部署的前端资源。还可以将按需加载的模块进行代码分隔，等到实际需要的时候再异步加载。通过 loader 的转换，任何形式的资源都可以视作`模块`，比如 CommonJs 模块、 AMD 模块、 ES6 模块、CSS、图片、 JSON、Coffeescript、 LESS 等                                                                                                                                                                                                                                                                                                                                                                                                        

对多种模块方案的支持与视一切资源为可管理模块的思路让他天然的适合React项目的开发，成为React官方退推荐的打包工具

## webpack 特点
### code splitting （代码拆分）
code splitting 也就是entry point ，webpack会自动的完成.

对于较大规模的web应用 （特别是单页应用），把所有代码合并到单个文件是比较低下的做法，单个文件体积过大导致应用初始加载缓慢。尤其如果其中很多逻辑只在特定情况下需要执行，每次都完整的加载所有的模块就变得浪费。webpack提供了代码拆分的方案，可将应用代码拆分为多个块（chunk），每个块包含一个或多个模块，块可以按需被异步加载。
webpack 的依赖树中有同步和异步两种依赖方式。其中，异步模块将会被拆成一个新的块，并且在被优化后，生成一个对应的文件。

### loader（加载器）

loader可以处理各种类型的静态文件，并且支持串联操作。webpack 本身只支持处理 JavaScript，但可以通过加载器来把别的资源转为 JavaScript。因此，每个资源都被当作一个模块。

使用loader时注意用多个loaders用`!`隔开即可，每个部分的loader的解析都相对于当前路径

### plugins（插件）
Webpack 还有一个功能丰富的插件系统。大多数内容功能都是基于这个插件系统运行的，还可以开发和使用开源的 Webpack 插件，来满足各式各样的需求

### 智能解析
Webpack 有一个智能解析器，几乎可以处理任何第三方库，无论它们的模块形式是 CommonJS、 AMD 还是普通的 JS 文件。甚至在加载依赖的时候，允许使用动态表达式 require("./templates/" + name + ".jade")

### 快速运行
Webpack 使用异步 I/O 和多级缓存提高运行效率，这使得 Webpack 能够以令人难以置信的速度快速增量编译。



## 使用
有两种方式使用webpack：
- 一种是使用命令行的方式 （在项目中不推荐使用，本文不介绍）
- 另一种写一个名为 webpack.config.js 的文件

###  简单的配置

```
module.exports = {
    entry: './app.jsx',
    output: {
        path: './',
        filename: 'app.js'
    },
    module: {
        loaders: [{
            test: /\.jsx$/,
            loader: 'jsx-loader'
        }]
    }
}
```

## 属性解释
### entry
```
entry: {
        page1: "./page1",
        //支持数组形式，将加载数组中的所有模块
        page2: ["./entry1", "./entry2"]
    },
```

entry：指定打包的入口文件，每有一个键值对，就是一个入口文件
如果不在这里指定，而且又不在项目代码的引用链中，那么webpack不会加载。

### output
```
output: {
		path: __dirname,
		filename: '[name].js'
	},
```
output：配置打包结果，path定义了输出的文件夹，filename则定义了打包结果文件的名称，filename里面的 [name] 会由entry中的键值替换

其中有一个最重要的但容易被忽略的配置是publicPath，它表示构建结果最终被真正访问的路径

#### PublicPath


### resolve
定义了解析模块路径时的配置

```
resolve: {
        root： '~/workspace/index.js'  //绝对路径
		
		// 现在你require文件的时候可以直接使用require('file')，不用使用require('file.js')
		extensions: ['', '.js', '.jsx'] 
	},
```
- root 绝对路径，从此路径开始加载
- extensions 用来指定模块的后缀，这样在引入模块时就不需要写后缀了，会自动补全

### module
```
module: {
		loaders: [{
			test: /\.js$/,
			loader: 'babel-loader'
		}, {
			test: /\.jsx$/,
			loader: 'babel-loader!jsx-loader?harmony'
		}]
	},
```

定义了对模块的处理逻辑，这里可以用loaders定义了一系列的加载器，以及一些正则。当需要加载的文件匹配test的正则时，就会调用后面的loader对文件进行处理，这正是webpack强大的原因

注意：
"-loader"其实是可以省略不写的，多个loader之间用“!”连接起来

#### css处理

#### less处理

#### sass处理

#### 图片处理

图片的处理需要安装url-loader.   limit= xx 的意思是当图片文件小于xx时，转换为base64的格式。同时指定生成到`/images/`的目录下

```
{
    test: /\.(png|jpg)$/,
    exclude: /node_modules/,
    loader: 'url-loader?limit=8192&name=/images/[name].[ext]'
}

```

#### 字体处理



### plugins
这里定义了需要使用的插件

常用的组件：
- commonsPlugin在打包多个入口文件时会提取出公用的部分，生成common.js 【eg1】
- [react-hot-loader](https://github.com/gaearon/react-hot-loader) 
- [extract-text-webpack-plugin](https://github.com/webpack/extract-text-webpack-plugin)  webpack 执行后会把样式文件单独提取出来，不打包到脚本中，作为单独的css文件 【eg2】
- webpack.optimize.DedupePlugin      // 这个插件搜索相似的块与文件并合并它们     【使用时出错】
- webpack.optimize.OccurenceOrderPlugin    // 这个插件通过计算子块和模块的使用次数进行优化   【使用时出错】
- [Htmlwbpackplugin](https://github.com/michael-ciniawsky/postcss-load-config) //可以自动生成html文件



eg1：
```
var webpack = require('webpack');
var commonsPlugin = new webpack.optimize.CommonsChunkPlugin('common.js');

module.exports = {
	plugins: [commonsPlugin]
}

或者支持自定义的提取
var CommonsChunkPlugin = require("webpack/lib/optimize/CommonsChunkPlugin");

module.exports = {
    entry: {
        p1: "./page1",
        p2: "./page2",
        p3: "./page3",
        ap1: "./admin/page1",
        ap2: "./admin/page2"
    },
    output: {
        filename: "[name].js"
    },
    plugins: [
        new CommonsChunkPlugin("admin-commons.js", ["ap1", "ap2"]),
        new CommonsChunkPlugin("commons.js", ["p1", "p2", "admin-commons.js"])
    ]
};
// <script>s required:
// page1.html: commons.js, p1.js
// page2.html: commons.js, p2.js
// page3.html: p3.js
// admin-page1.html: commons.js, admin-commons.js, ap1.js
// admin-page2.html: commons.js, admin-commons.js, ap2.js

```


eg2:
```
var ExtractTextPlugin = require("extract-text-webpack-plugin");
module.exports = {
        plugins: [new ExtractTextPlugin("[name].css")],
      }
```

eg3:

```
module.exports = {
        plugins: [
        // 这个插件搜索相似的块与文件并合并它们
        new webpack.optimize.DedupePlugin(),
        
        // 这个插件通过计算子块和模块的使用次数进行优化
        new webpack.optimize.OccurenceOrderPlugin()
        ],
      }
```




## webpack 运行参数

### webpack

- webapck 执行一次开发的变异


### webpack-dev-server 
```
$ webpack-dev-server --display-error-details --history-api-fallback --hot --inline --progress --colors --port 4000 --host 0.0.0.0 
```
- --display-error-details 推荐加上的，方便出错时能查阅更详尽的信息
- --progress 带有进度
- --colors 带有颜色
- --profile 输出性能数据，可以看到每一步的耗时
- --display-modules 默认情况下 node_modules 下的模块会被隐藏，加上这个参数可以显示这些被隐藏的模块


## 设置缓存
对于静态文件，第一次获取之后，文件内容没改变的话，浏览器直接读取缓存文件即可。那如果缓存设置过长，文件要更新怎么办呢？嗯，以文件内容的 MD5 作为文件名就是一个不错的解决方案。

```
 output: {
    path: __dirname + '/build',
    filename: '[bundle]-[chunkhash:6].js'
  },
```
 
output:
```
[bundle]-6519c9.js
```

## 搭建脚手架需要解决的问题
- ES6支持
- React JSX支持
- css/scss/less支持
- 第三方例如jquery和fontawesome的支持
- webpack-dev-server
- 热替换
- 自动刷新
- 局部自动刷新

## 功能标识
项目中有些代码我们只为在开发环境（例如日志）或者是内部测试环境（例如那些没有发布的新功能）中使用，那就需要引入下面这些魔法全局变量（magic globals）：

```
if (__DEV__) {
  console.warn('Extra logging');
}
// ...
if (__PRERELEASE__) {
  showSecretFeature();
}
```

同时还要在webpack.config.js中配置这些变量，使得webpack能够识别他们。
```
// webpack.config.js

// definePlugin 会把定义的string 变量插入到Js代码中。
var definePlugin = new webpack.DefinePlugin({
  __DEV__: JSON.stringify(JSON.parse(process.env.BUILD_DEV || 'true')),
  __PRERELEASE__: JSON.stringify(JSON.parse(process.env.BUILD_PRERELEASE || 'false'))
});

module.exports = {
  entry: './main.js',
  output: {
    filename: 'bundle.js'
  },
  plugins: [definePlugin]
};
```
配置完成后，就可以使用 BUILD_DEV=1 BUILD_PRERELEASE=1 webpack来打包代码了。 值得注意的是，webpack -p 会删除所有无作用代码，也就是说那些包裹在这些全局变量下的代码块都会被删除，这样就能保证这些代码不会因发布上线而泄露。

## Webpack-dev-server
webpack-dev-server 是运行在`内存中`的，所以不会像执行webpack一样，编译出真实的文件



## SourceMaps

## Hot Module Replacement
热模块替换改变，添加，移除模块而不需要页面的刷新
使用**webpack-dev-server** 有两种方式打开HMR
方法1：在命令行中加入 `--hot`和 `--inine`
```
webpack-dev-server --hot --inline
```
- `--hot` 自动添加HotModuleReplacementPlugin插件
- `--inline` 将webpack-dev-server运行时嵌入到bundle中

注意：在命令行中指定`--hot`参数，这种开启方式只适合inline模式，也就是说，你必须同时结合inline模式使用：`webpack-dev-server --inline --hot`。

方法2：修改webpack.config.js文件

- add new webpack.HotModuleReplacementPlugin() to the plugins field
- add webpack/hot/dev-server and webpack-dev-server/client?http://localhost:8080 to the entry field

Eg：
```
var webpack = require('webpack');
var path = require('path');

module.exports = {
  entry: [
    'webpack/hot/dev-server',
    'webpack-dev-server/client?http://localhost:8080',
    './index.js'
  ],
  output: {
    filename: 'bundle.js',
    publicPath: '/static/'
  },
  plugins: [
    new webpack.HotModuleReplacementPlugin()
  ],
  module: {
    loaders: [{
      test: /\.jsx?$/,
      exclude: /node_modules/,
      loader: 'babel-loader',
      query: {
        presets: ['es2015', 'react']
      },
      include: path.join(__dirname, '.')
    }]
  }
};
```

## 自动刷新
webpack-dev-server支持两种模式的自动刷新。

**iframe模式**

使用iframe模式并不需要多余的配置，直接访问http://[host]:[port]/webpack-dev-server/[path]即可，iframe模式的特征如下：
> ✦ 无需额外的配置

> ✦ 顶部条可以显示编译信息

> ✦ 浏览器的地址不会跟着页面URL变动


**inline模式**

简单配置可以开启，然后直接访问http://[host]:[port]/[path]即可，inline模式的特征如下：
> ✦ 需要额外的配置

> ✦ 编译信息只能在命令行和浏览器console中查看

> ✦ 浏览器的地址和页面URL同步

前文提到的简单配置到底有多简单呢？如下两种方式均可：
- ➙ 在命令行中指定`--inline`参数，比如：`webpack-dev-server --inline`
- ➙ 在webpack.config.js配置文件中添加devServer: {inline: true}

## 问题
### 不能自动刷新
场景：发现一个问题就是在一个项目中的是可以自动刷新的，但是相同的代码在另一个项目中就不能自动的刷新
分析：起初以为是代码的问题，结果尝试将两个代码的配置项改为相同的还是不行，尝试无果，在论坛上发现有人是因为在osx
（mac）上的文件夹的名字大写导致的，所以尝试将项目的文件夹的名字更改
解决：更改不能work的文件夹的名字为简单的英文即可，很坑。

### 添加第三方依赖失败
场景：尝试在webpack中添加jquery，参考了官网的写法但是一直不能work
分析：
解决：将webpack的config的文件中的entry中的vendor的值由 string 改为 array即可  



## 学习资料
视频：

书：
- Webpack 中文指南 http://zhaoda.net/webpack-handbook/


## 参考资料

官网：http://webpack.github.io/docs/

文章：
- WebPack实例与前端性能优化：https://segmentfault.com/a/1190000004577578
- WEBPACK DEV SERVER：http://www.jianshu.com/p/b95bbcfc590d
- http://www.infoq.com/cn/articles/react-and-webpack/
- https://rhadow.github.io/2015/05/30/webpack-loaders-and-plugins/

代码：
阮一峰webpack-demo：https://github.com/ruanyf/webpack-demos




