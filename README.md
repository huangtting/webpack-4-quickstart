# webpack-4-quickstart
> Webpack 4 tutorial: All You Need to Know, from 0 Conf to Production Mode
> Webpack 4 的简介，从0到Production Mode

[![Donate](https://img.shields.io/badge/donate-patreon-orange.svg)](https://www.patreon.com/valentinogagliardi)
[![Software License](https://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat)](LICENSE)

## Development

```bash
npm i && npm run start
```

## Meta

Valentino Gagliardi - [valentinog.com](https://www.valentinog.com) - valentino@valentinog.com

## License

This project is open-sourced software licensed under the [MIT license](http://opensource.org/licenses/MIT).

## 博客写的很好，总结一下
Webpack4提倡零配置，所以约定默认的入口文件是./src/index.js，约定默认的出口文件是./dist/main.js.

Webpack4提供了两种mode，在package.json中添加如下代码：
```
"scripts": {
  "dev": "webpack --mode development",
  "build": "webpack --mode production"
}
```
然后使用
```
npm run dev 
npm run build
```
就可以使用这两种模式，production（生产模式）比development（开发模式）相比，生产模式
开启所有的优化代码
更小的bundle大小
去除掉只在开发阶段运行的代码
Scope hoisting和Tree-shaking
自动启用uglifyjs对代码进行压缩

当然也可以覆盖默认的出入口文件,可以自己在script中约定出入口文件
```
"scripts": {
  "dev": "webpack --mode development ./foo/src/js/index.js --output ./foo/main.js",
  "build": "webpack --mode production ./foo/src/js/index.js --output ./foo/main.js"
}
```

### 安装loaders
Webpack4的零配置只包括
默认的出入口文件
生产模式和开发模式

如果你需要使用其他的loaders就需要自己写配置文件(= =)
所以还是需要创建webpack.config.js

这里直接放配置文件，顺便写点注释
```
const HtmlWebPackPlugin = require("html-webpack-plugin");
const MiniCssExtractPlugin = require("mini-css-extract-plugin");

module.exports = {
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        exclude: /node_modules/,
        use: {
         <!-- 对以.js和.jsx结尾的文件，使用babel-loader，将ES6的代码转换成ES5 -->
         <!-- 安装babel-preset-react使得Webpack可以处理jsx文件 -->
          loader: "babel-loader"
        }
      },
      {
        test: /\.html$/,
        use: [
          {
            <!--使用HTML loader使得Webpack可以处理.html文件  -->
            loader: "html-loader",
            options: { minimize: true }
          }
        ]
      },
      {
        test: /\.css$/,
        use: [MiniCssExtractPlugin.loader, "css-loader"]
      }
    ]
  },
  plugins: [
    <!--HtmlWebPackPlugin插件会以./src/index.html为模板生成./index.html，同时会帮你自动引入静态文件，比如css，js，图片，有时候Webpack打包后的文件名会添加hash，使用这个插件可以自动引入，否则引入图片路径可能不对  -->
    new HtmlWebPackPlugin({
      template: "./src/index.html",
      filename: "./index.html"
    }),
    <!-- Webpack4推荐使用MiniCssExtractPlugin来打包css，基本配置如下 -->
    new MiniCssExtractPlugin({
      filename: "[name].css",
      chunkFilename: "[id].css"
    })
  ]
};

```
其实只是一个小入门，各种各样的插件就够喝一壶了。





