---
title: Webpack-high-optimize
date: 2016-12-21 02:11:02
tags:
- webpack
---

# webpack 高级优化

### 优化
#### 1.如何区分开发及生产环境
在package.json里面的script设置环境变量，注意mac与windows的设置方式不一样

```
"scripts": {
    "publish-mac": "export NODE_ENV=prod&&webpack -p --progress --colors",
    "publish-win":  "set NODE_ENV=prod&&webpack -p --progress --colors"
}
```
在webpack.config.js使用process.env.NODE_ENV进行判断

功能标识

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

#### 2.使用代码热替换
参考webpack-dev-server: https://webpack.github.io/docs/webpack-dev-server.html#hot-module-replacement

#### 3.import react导致文件变大，编译速度变慢，乍办?
方法
- A 如果你想将react分离，不打包到一起，可以使用externals。然后用<script>单独将react引入
- B 如果不介意将react打包到一起，请在alias中直接指向react的文件。可以提高webpack搜索的速度。准备部署上线时记得将换成react.min，能减少文件大小(减少约600kb)
- C 使用module.noParse针对单独的react.min.js这类没有依赖的模块，速度会更快

#### 4.将模块暴露到全局
如果想将report数据上报组件放到全局，有两种办法：
方法一：
在loader里使expose将report暴露到全局，然后就可以直接使用report进行上报

```
{
    test: path.join(config.path.src, '/js/common/report'),
    loader: 'expose?report'
}
```

方法二：
如果想用R直接代表report，除了要用expose loader之外，还需要用ProvidePlugin帮助，指向report，这样在代码中直接用R.tdw， R.monitor这样就可以

```
new webpack.ProvidePlugin({
    "R": "report",
}),
```

#### 5. 合并公共代码
有些类库如utils, bootstrap之类的可能被多个页面共享，最好是可以合并成一个js，而非每个js单独去引用。这样能够节省一些空间。这时我们可以用到CommonsChunkPlugin，我们指定好生成文件的名字，以及想抽取哪些入口js文件的公共代码，webpack就会自动帮我们合并好
```
new webpack.optimize.CommonsChunkPlugin('vendor', 'vendor.js')
```

```
 new webpack.optimize.CommonsChunkPlugin({
            name:      'main', // 把依赖移动到主文件
            children:  true, // 寻找所有子模块的共同依赖
            minChunks: 2, // 设置一个依赖被引用超过多少次就提取出来
        }),
```

第一个参数是entry中的vendor的名字

```
entry: {
    jsx: './src/index.js',
    html: './src/index.html',
    vendor: ['jquery']       //Array
  }
```

  
#### 6.善用alias
resolve里面有一个alias的配置项目，能够让开发者指定一些模块的引用路径。对一些经常要被import或者require的库，如react,我们最好可以直接指定它们的位置，这样webpack可以省下不少搜索硬盘的时间。
```
alias: {
      'react': path.join('node_modules','react/dist/react.min')
    }
```

#### 7.React-hot-loader组件级热更新 
虽然实现了代码的热替换，只要在编辑器中保存我们编辑的代码，浏览器即可实时刷新。但同时也有一个烦恼，如果我们的项目开发中用到了几十个组件，为了测试某个组件我们需要一步步操作到固定的步骤去实现，一旦保存编辑器中修改的一行代码，从入口文件开始的所有代码都全部刷新了一次，这样很不利于调试。

```
{
   test: /\.jsx|js$/,
   exclude: /node_modules/,
   loaders: ['react-hot', 'babel?presets[]=react,presets[]=es2015']
   //原先的配置
   // loader: 'babel-loader',
   // query: {
   //   presets: ['react', 'es2015']
   // }
}
```

#### 8.如何压缩代码
在生产环境中我们需要将代码压缩，尽可能的减少代码的体积，所以需要在生产环境的webpack中加入以下插件
```
{
  plugins: [
    // 代码混淆压缩
    new webpack.optimize.UglifyJsPlugin({
      compress: {
        warnings: false
      }
    })
  ]
}
```

加入了这个插件之后，编译的速度会明显变慢，所以一般只在生产环境启用。

另外，服务器端还可以开启 gzip 压缩，优化的效果更明显。


#### 9. 代码运行前添加eslint的检测

新建一个eslint.rc的配置文件
安装eslint-loader

```
preLoaders: [
      {
        test: /\.js$/,
        loader: "eslint-loader",
        exclude: /node_modules/
      }
],
```

#### 10.build前先清理build目录

```
npm install clean-webpack-plugin --save-dev
```

## 参考资料
- webpack使用优化： http://www.alloyteam.com/2016/01/webpack-use-optimization/
- 如何十倍提升你的webpack构建效率： http://gold.xitu.io/entry/5768e6ab207703006b310f95
- 让 Webpack 来帮你打包吧： https://gold.xitu.io/entry/5767a975df0eea0062ffe193
- webpackForSPA:  https://github.com/huangshuwei/webpackForSPA/blob/master/webpack.config.js
