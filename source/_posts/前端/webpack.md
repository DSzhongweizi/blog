---
title: webpack
cover: false
abbrlink: b9f20a20
date: 2020-10-03 14:21:30
updated:
categories:
tags:
  - webpack
  
---
在模块化之前，文件之间的依赖关系通过加载顺序来规范的。CommonJS、AMD和ES6 Module规范的出现，通过明确各个文件之间的相互引用关系，来解决文件之间依赖引用的问题。

webpack是一个模块打包工具（module bundler），重点在于打包，主要解决了文件过多的问题，webpack的核心就是提供模块化的开发方式和编译打包功能。

## 核心概念
### 模块
对于webpack，模块不仅仅是javascript模块，它包括了任何类型的源文件，不管是图片、字体、json文件都是一个个模块。

Webpack支持以下的方式引用模块：
- ES2015 import 方法
- CommonJs require() 方法
- AMD define 和 require 语法
- css/sass/less文件中的 @import 语法
- url(...) 和　`<img src=...>` 中的图片路径

### Dependency Graph（依赖关系图）
依赖关系图，是webpack根据每个模块之间的依赖关系递归生成的一张内部逻辑图，有了这张依赖关系图，webpack就能按图索骥把所有需要模块打包成一个bundle文件了。

### Entry（入口）
绘制依赖关系图的起始文件被称为entry，默认的entry为 ./src/index.js，或者我们可以在配置文件中配置。
> entry可以为一个也可以为多个。

单个entry：
```js
module.exports = {
  entry: './src/index.js'
}
或者

module.exports = {
  entry: {
    main: './src/index.js'
  }
};
```

多个entry，一个chunk
> 我们也可以指定多个独立的文件为entry，但将它们打包到一个chunk中，此种方法被称为 multi-main entry，我们需要传入文件路径的数组：
```js
module.exports = {
  entry: ['./src/index.js', './src/index2.js', './src/index3.js']
}
```
方法的灵活性和扩展性有限，因此并不推荐使用。

多个entry，多个chunk
> 如果有多个entry，并且每个entry生成对应的chunk，我们需要传入object：
```js
module.exports = {
  entry: {
    app: './src/app.js',
    admin: './src/admin.js'
  }
};
```
这种写法有最好的灵活性和扩展性，支持和其他的局部配置（partial configuration）进行合并。比如将开发环境和生产的配置分离，并抽离出公共的配置，在不同的环境下运行时再将环境配置和公共配置进行合并。

### Output（出口）
出口就是webpack打包完成的输出，output定义了输出的路径和文件名称，Webpack的默认的输出路径为 ./dist/main.js，同样，我们可以在配置文件中配置output：
```js
module.exports = {
  entry: './src/index.js',
  output: {
    path: __dirname + '/dist',
    filename: 'bundle.js'
  }
};
```

多个entry的情况
> 一个entry应该对应一个output，此时输出的文件名需要使用替换符（substitutions）声明以确保文件名的唯一性，例如使用入口模块的名称：
```js
module.exports = {
  entry: {
    app: './src/app.js',
    search: './src/search.js'
  },
  output: {
    filename: '[name].js',
    path: __dirname + '/dist'
  }
}
```
> 最终在 ./dist 路径下面会生成 app.js和search.js两个bundle文件。

### Loader
Webpack自身只支持加载js和json模块，而webpack的理念是让所有的文件都能被引用和加载并生成依赖关系图，所以loader出场了。Loader能让webpack能够去处理其他类型的文件（比如图片、字体文件、xml）。

我们可以在配置文件中这样定义一个loader:
```js
// webpack.config.js
module.exports = {
  module: {
    rules: [
      { 
        test: /\.txt$/, // 定义需要被转化的文件或者文件类型
        use: 'raw-loader' // 定义对该文件进行转化的loader的类型
      }
    ]
  }
};
```
> 该条配置相当于告诉webpack当遇到一个txt文件的引用时（使用require或者import进行引用），先用raw-loader转换一下该文件再把它打包进bundle。

还有其他各种类型的loader，比如加载css文件的css-loader，加载图片和字体文件的file-loader，加载html文件的html-loader，将最新JS语法转换成ES5的babel-loader等等，完整列表请参考 [webpack loaders](https://webpack.js.org/loaders/)。

### Plugin（插件）
Plugin和loader是两个比较混淆和模糊的概念:
- Loader是用来转换和加载特定类型的文件，所以loader的执行层面是单个的源文件。
- plugin可以实现的功能更强大，plugin可以监听webpack处理过程中的关键事件，深度集成进webpack的编译器，可以说plugin的执行层面是整个构建过程。

Plugin系统是构成webpack的主干，webpack自身也基于plugin系统搭建，webpack有丰富的内置插件和外部插件，并且允许用户自定义插件。

与loader不同，使用plugin我们必须先引用该插件，例如：
```js
const webpack = require('webpack'); // 用于引用webpack内置插件
const HtmlWebpackPlugin = require('html-webpack-plugin'); // 外部插件

module.exports = {
  plugins: [
    new webpack.HotModuleReplacementPlugin(),
    new HtmlWebpackPlugin({template: './src/index.html'})
  ]
};
```









