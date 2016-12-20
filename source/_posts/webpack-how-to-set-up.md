---
title: Webpack-how-to-set-up
date: 2016-12-21 02:06:51
tags:
- webpack
- react

---


# Webpack how to setup

## 实战
需求： 建立一个支持以下功能的webpack + react 配置

- ES6
- React(jsx)
- Webpack  (css/less/scss  图片  字体 html)
- 第三方依赖（jquery）
- 热加载
- 自动刷新
- 局部自动刷新


### Step by step

V1 支持
- Support `ES5` grammar
- 基本构建
- React(jsx)解析

```
$ npm install webpack babel-loader  --save-dev
$ npm install react react-dom --save
```

```
//webpack.config.js
var webpack = require('webpack')

module.exports = {
  entry: './src/index.js',
  output: {
    path: './public',
    filename: 'bundle.js'
  },
  resolve: {
    extensions: ['', '.js', '.jsx']
  },
  module: {
    loaders: [{
      test: /\.jsx|js$/,
      exclude: /node_modules/,
      loader: 'babel-loader'
    }]
  }
}

//babel-loader   依赖于babel-core和babel-loader

//package.json
{
  "name": "",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "babel-core": "^6.18.2",
    "babel-loader": "^6.2.8",
    "webpack": "^1.13.3"
  },
  "dependencies": {
    "react": "^15.4.0",
    "react-dom": "^15.4.0"
  }
}
```

V2 支持

- ES6
- React（JSX）
- Html解析
- 指定路径解析位置

安装：
```
npm install  file-loader babel-preset-es2015 babel-preset-react --save-dev
```

```
//webpack.config.js
var webpack = require('webpack')
var path = require('path')

module.exports = {
  entry: {
    jsx: './src/index.js',
    html: './src/index.html'
  },
  output: {
    path: './public',
    filename: 'bundle.js'
  },
  resolve: {
    root: [path.resolve('./src')],
    extensions: ['', '.js', '.jsx']
  },
  module: {
    loaders: [
      {
        test: /\.jsx|js$/,
        exclude: /node_modules/,
        loader: 'babel-loader',
        query: {
          presets: ['react', 'es2015']
        }
      },
      {
        test: /\.(html)$/,
        loader: 'file?name=[name].[ext]'
      },
    ]
  }
}


//file?name=[name].[ext] 依赖于file-loader
//query: 
//{
//   presets: ['react','es2015']   依赖于babel-preset-react 和 babel-preset-es2015
//}

//package.json
{
  "name": "",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "babel-core": "^6.18.2",
    "babel-loader": "^6.2.8",
    "babel-preset-es2015": "^6.18.0",
    "babel-preset-react": "^6.16.0",
    "file-loader": "^0.9.0",
    "webpack": "^1.13.3"
  },
  "dependencies": {
    "react": "^15.4.0",
    "react-dom": "^15.4.0"
  }
}
```

V3 支持

- 添加了第三方依赖
- 热加载HMR
- 自动刷新
- 配置npm 命令
- 合并公共代码

```
$ npm install react-hot-loader webpack-dev-server --save
$ npm install jquery --save
```

```
//webpack.config.js
var webpack = require('webpack')
var path = require('path')

module.exports = {
  entry: {
    jsx: './src/index.js',
    html: './src/index.html',
    vendor: ['jquery']       //Array
  },
  output: {
    path: __dirname + '/public',
    filename: 'bundle.js'
  },
  resolve: {
    root: [path.resolve('./src')],
    extensions: ['', '.js', '.jsx']
  },
  module: {
    loaders: [
      {
        test: /\.jsx|js$/,
        exclude: /node_modules/,
        loaders: ['react-hot', 'babel?presets[]=react,presets[]=es2015']
      },
      {
        test: /\.(html)$/,
        loader: 'file?name=[name].[ext]'
      }
    ]
  },
  plugins: [
    new webpack.ProvidePlugin({
      $: "jquery",
      jQuery: "jquery",
      "window.jQuery": "jquery"
    }),
    new webpack.optimize.CommonsChunkPlugin('vendor', 'vendor.js'),
  ]
}

//将第三方依赖注入到vendor.js中


//package.json
{
  "name": "",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "start": "webpack-dev-server  --inline --hot --display-error-details  --history-api-fallback  --progress --colors --port 5000 --host 0.0.0.0"
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "babel-core": "^6.18.2",
    "babel-loader": "^6.2.8",
    "babel-preset-es2015": "^6.18.0",
    "babel-preset-react": "^6.16.0",
    "file-loader": "^0.9.0",
    "react-hot-loader": "^1.3.1",
    "webpack": "^1.13.3",
    "webpack-dev-server": "^1.16.2"
  },
  "dependencies": {
    "jquery": "^3.1.1",
    "react": "^15.4.0",
    "react-dom": "^15.4.0"
  }
}

```


V4 完整版
支持:
- ES6
- React(jsx)
- Webpack  (css/less/scss  图片  字体 html)
- Html解析
- 指定路径解析位置
- 第三方依赖（jquery）
- 热加载
- 自动刷新
- 配置npm 命令
- eslint(pre-loader 不是很实用)
- 生产模式webpack
- 开发模式webpack

```
//开发模式
//webpack.dev.config.js

var webpack = require('webpack');
var path = require('path');

module.exports = {
  entry: {
    jsx: './src/index.js',
    html: './src/index.html',
    ico: './src/favicon.ico',
    vendor: ['jquery']       //Array
  },
  output: {
    path: path.resolve('./build'),
    filename: 'bundle.js'
  },
  resolve: {
    root: [path.resolve('./src')],
    publicPath: '/',
    extensions: ['', '.js', '.jsx'],
    modulesDirectories: ['node_modules']
  },
  module: {
    preLoaders: [
      {
        test: /\.js$/,
        loader: "eslint-loader",
        exclude: /node_modules/
      }
    ],
    loaders: [
      {
        test: /\.jsx|js$/,
        exclude: /node_modules/,
        loaders: ['react-hot', 'babel?presets[]=react,presets[]=es2015']
      },
      {
        test: /\.css$/,
        loaders: [
          'style-loader',
          'css-loader?importLoaders=1',
          'postcss-loader'
        ]
      },
      {
        test: /\.scss$/,
        loaders: [
          'style',
          'css',
          'postcss',
          'sass'
        ]
      },
      {
        test: /\.(png|jpg)$/,
        exclude: /node_modules/,
        loader: 'url-loader?limit=8192&name=/images/[name].[ext]'
      },
      {
        test: /\.(eot|svg|ttf|woff)$/,
        exclude: /node_modules/,
        loader: 'url-loader?limit=8192&name=/fonts/[name].[ext]'
      },
      {
        test: /\.(html|ico)$/,
        loader: 'file?name=[name].[ext]'
      }
    ]
  },
  plugins: [
    new webpack.ProvidePlugin({
      $: 'jquery',
      jQuery: 'jquery',
      'window.jQuery': 'jquery'
    }),
    new webpack.optimize.CommonsChunkPlugin('vendor', 'vendor.js')
  ],
  devtool: 'eval-source-map'
};
```

```
//生产模式
//webpack.prod.config.js
var webpack = require('webpack');
var path = require('path');
var CleanPlugin = require('clean-webpack-plugin');

module.exports = {
  entry: {
    jsx: './src/index.js',
    html: './src/index.html',
    ico: './src/favicon.ico',
    vendor: ['jquery']       //Array
  },
  output: {
    path: path.resolve('./build'),
    filename: 'bundle.js'
  },
  resolve: {
    root: [path.resolve('./src')],
    publicPath: '/',
    extensions: ['', '.js', '.jsx'],
    modulesDirectories: ['node_modules']
  },
  module: {
    preLoaders: [
      {
        test: /\.js$/,
        loader: "eslint-loader",
        exclude: /node_modules/
      }
    ],
    loaders: [
      {
        test: /\.jsx|js$/,
        exclude: /node_modules/,
        loaders: ['react-hot', 'babel?presets[]=react,presets[]=es2015']
      },
      {
        test: /\.css$/,
        loaders: [
          'style-loader',
          'css-loader?importLoaders=1',
          'postcss-loader'
        ]
      },
      {
        test: /\.scss$/,
        loaders: [
          'style',
          'css',
          'postcss',
          'sass'
        ]
      },
      {
        test: /\.(png|jpg)$/,
        exclude: /node_modules/,
        loader: 'url-loader?limit=8192&name=/images/[name].[ext]'
      },
      {
        test: /\.(eot|svg|ttf|woff)$/,
        exclude: /node_modules/,
        loader: 'url-loader?limit=8192&name=/fonts/[name].[ext]'
      },
      {
        test: /\.(html|ico)$/,
        loader: 'file?name=[name].[ext]'
      }
    ]
  },
  plugins: [
    new CleanPlugin('build'),
    new webpack.optimize.CommonsChunkPlugin('vendor', 'vendor.js'),
    new webpack.DefinePlugin({
      'process.env': {
        NODE_ENV: JSON.stringify('production')
      }
    }),
    new webpack.optimize.UglifyJsPlugin({
      compress: {
        warnings: false
      }
    })
  ],
};

```

```
//package.json
{
  "name": "",
  "version": "0.1.0",
  "devDependencies": {
    "autoprefixer": "^6.5.3",
    "autoprefixer-loader": "^3.2.0",
    "babel-core": "^6.18.2",
    "babel-eslint": "^7.1.1",
    "babel-jest": "^17.0.2",
    "babel-loader": "^6.2.8",
    "babel-plugin-transform-decorators-legacy": "^1.3.4",
    "babel-plugin-transform-es3-property-literals": "^6.8.0",
    "babel-plugin-transform-runtime": "^6.15.0",
    "babel-polyfill": "^6.16.0",
    "babel-preset-es2015": "^6.18.0",
    "babel-preset-jest": "^17.0.2",
    "babel-preset-react": "^6.16.0",
    "babel-preset-stage-0": "^6.16.0",
    "babel-runtime": "^6.18.0",
    "clean-webpack-plugin": "^0.1.14",
    "css-loader": "^0.26.0",
    "extract-text-webpack-plugin": "^1.0.1",
    "file-loader": "^0.9.0",
    "node-sass": "^3.13.0",
    "postcss-loader": "^1.2.1",
    "react-hot-loader": "^1.3.1",
    "react-scripts": "0.7.0",
    "sass-loader": "^4.0.2",
    "style-loader": "^0.13.1",
    "url-loader": "^0.5.7",
    "webpack": "^1.13.3",
    "webpack-dev-server": "^1.16.2"
  },
  "dependencies": {
    "jquery": "^3.1.1",
    "postscribe": "^2.0.8",
    "react": "^15.4.0",
    "react-dom": "^15.4.0",
    "react-modal": "^1.5.2",
    "react-router": "^3.0.0",
  },
  "scripts": {
    "start": "webpack-dev-server  --config ./config/webpack.dev.config.js --inline --hot --display-error-details  --history-api-fallback  --progress --colors --port 5000 --host 0.0.0.0",
    "build": "webpack --config ./config/webpack.prod.config.js"
  }
}

```