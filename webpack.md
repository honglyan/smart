# webpack 入门

## 目录

### 1 安装 webpack  
ebpack 是一个 npm 包，所以我们通过 npm 命令来全局安装：
npm install webpack -g
安装完成后，命令行下就有 webpack 命令可用，执行 webpack --help 可以查看 webpack 提供的各种命令。
安装 webpack
w
### 2 初始化项目  

### 3 webpack 配置  
### 4 自动刷新  
### 5 第三方库  
### 6 模块化  
### 7 打包、构建  
### 8 webpack 模板  


初始化项目
grunt.js 一类工具可以借助 yeoman 来初始化项目，目前我并没有看到 webpack 有类似方法，所以当 node.js 项目来初始化。

npm init 创建一个 package.json 文件
npm install webpack --save-dev 在当前目录下安装局域的 webpack
完成以上两个步骤后，我们的项目下有一个 package.json 文件，一个 node_modules 文件夹，我们还需要一个 index.html 文件，内容如下：

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>webpack 教程</title>
</head>
<body>
</body>
</html>
webpack 配置
我们的代码将组织在 JavaScript 模块中，项目会有一个入口（entry）文件，比如 main.js，我们需要通过 webpack 的配置文件指明它的位置。

在根目录新建一个 webpack.config.js 文件，添加如下内容：

module.exports = {
  entry: './main.js'
};
因为我们在项目部署前需要打包合并 js 文件，所以还需要在 webpack.config.js 中配置一个 output：

module.exports = {
    entry: './main.js',
    output: {
        path: __dirname,
        filename: 'bundle.js'
    }
}
output 定义我们打包出来的文件位置及名称。

完成以上后，试着在项目根目录下执行 webpack 命令，我们的根目录下会多出一个 bundle.js 文件：

 webpack build
自动刷新
到现在为止，我们还没在浏览器中打开 index.html 文件，实际上，我们连 bundle.js 文件都还没加入 index.html 文件中。现在且先加入：

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>webpack 教程</title>
</head>
<body>
  <script src="./bundle.js"></script> <!-- 在 index.html 文件中添加这一行代码 -->
</body>
</html>
这是 webpack 异于其它工具的地方，它在 HTML 文件中直接引用了 build 后的 js 文件，而不是原始的 main.js 文件。这就会有几个问题：

Q: main.js 或它所引用的模块的变化如何实时编译到 bundle.js？

A: 非常简单，在根目录下执行 webpack --watch 就可以监控文件变化并实时编译了

Q: 上面只是实时编译了 js 文件，我们该如何把变化同步到浏览器页面上，实现实时刷新？

A: webpack 提供了 webpack-dev-server 解决实时刷新页面的问题，同时解决上面一个问题。

​

安装 webpack-dev-server

执行 npm install webpack-dev-server -g 在全局环境中安装 webpack-dev-server
在项目根目录下执行命令：

$ webpack-dev-server
这样，我们就可以在默认的 http://localhost:8080 网址上打开我们的项目文件了。

此时，我们可能会认为，

js 文件修改
webpack-dev-server 监控到变化
webpack 重新编译
实时更新浏览器中的页面
但不幸的是，我们「自以为是」了。http://localhost:8080 这个网站对 js 文件的变化无动于衷。

我们可以启用 webpack-dev-server 的 hot 模式，在代码中插入如下一行：

<script src="http://localhost:8080/webpack-dev-server.js"></script>
这样 http://localhost:8080/ 这个网址也可以根据 js 的变化相应地自动刷新了。

第三方库
webpack 并不是包管理器，所以如果我们要使用第三方库，则需要借助 npm 或其它工具。比如，在项目里安装 jQuery：

npm install jquery --save
模块化
webpack 自称 module bundler，在它的定义中，CSS、图片、文件等等，都可以通过相应的 loader 加载，变成 JavaScript模块，以下具体展开说明。

模块化 JavaScript

如果我想使用 ES6 的方式引入某个 es6 模块，比如：

import $ from 'whatever';
怎么办？浏览器并不提供原生支持，webpack 通过各种 loader 来解决这类问题。比如这 ES6 的语法，可以借助 babel-loader：

安装 babel-loader

npm install --save-dev babel-loader
配置 webpack.config.js

在 module.exports 值中添加 module：

module.exports = {
entry: {
    app: ['webpack/hot/dev-server', './main.js']
},
output: {
    filename: 'bundle.js'
},
module: {
    loaders: [
        { test: /\.js$/, loader: 'babel', exclude: /node_modules/ }
    ]
}
}

这样我们就可以在我们的 js 文件中使用 ES6 语法，babel-loader 会负责编译。

上面的方法，是在 webpack.config.js 文件中定义某一类型文件的加载器，我们也可以在代码中直接指定：

import $ from 'babel!whatever'
因为 babel-loader 允许我们使用 ES6 语法，于是我们的模块完全可以用 ES6 的 module 来组织代码： export 一个简单的模块：

// script/log.js 文件
export default (param) => {
    console.log('你好啊', param);
}
然后在 main.js 中导入使用：

// main.js 文件
import log from "./script/log.js";
CSS 加载器

我们可以按传统方法使用 CSS，即在 HTML 文件中添加：

<link rel="stylesheet" href="style/app.css">
但 webpack 里，CSS 同样可以模块化，然后使用 import 导入。

因此我们不再使用 link 标签来引用 CSS，而是通过 webpack 的 style-loader 及 css-loader。前者将 css 文件以 <style></style> 标签插入 <head> 头部，后者负责解读、加载 CSS 文件。

安装 CSS 相关的加载器

npm install style-loader css-loader --save-dev
配置 webpack.config.js 文件

{
// ...
module: {
    loaders: [
        { test: /\.css$/, loaders: ['style', 'css'] }
    ]
}
}
在 main.js 文件中引入 css

import'./style/app.css';
重启 webpack-dev-server

模块化 CSS

上一步里，我们 import 到 JavaScript 文件中的 CSS 文件中的 CSS 类打包时是 export 到全局环境的，也就是说，我们只是换了种加载 CSS 的方式，在书写 CSS 的时候，还是需要注意使用命名规范，比如 BEM，否则全局环境 CSS 类的冲突等问题不会消失。

这里，webpack 做了一个模块化 CSS 的尝试，真正意思上的「模块化」，即 CSS 类不会泄露到全局环境中，而只会定义在 UI 模块内 – 比如 react.js 这类模块，或者 web components。类似的尝试还有 ember-component-css 与 jspm 的 plugin css。

autoprefixer

我们在书写 CSS 时，按规范写，构建时利用 autoprefixer 可以输出 -webkit、-moz 这样的浏览器前缀，webpack 同样是通过 loader 提供该功能。

安装 autoprefixer-loader

npm install autoprefixer-loader --save-dev
配置 webpack.config.js

loaders: [{
loader: 'style!css!autoprefixer?{browsers:["last 2 version", "> 1%"]}',
//...
}]
重启 webpack-dev-server

假如我们在 CSS 中写了 body { display: flex; } 规则，再查看 bundle.js 文件的话，我们能看到类似如下的代码：

body {\n\tdisplay: -webkit-box;\n\tdisplay: -webkit-flex;\n\tdisplay: -ms-flexbox;\n\tdisplay: flex;\n}
图片

图片同样可是是模块，但使用的是 file loader 或者 url loader，后者会根据定义的大小范围来判断是否使用 data url。

import loadingIMG from 'file!../img/loading.gif'

React.render(<img src={loadingIMG} />, document.getElementById('app'));
打包、构建
项目结束后，代码要压缩、混淆、合并等，只需要在命令行执行：

webpack
即可，webpack 根据 webpack.config.js 文件中的配置路径、构建文件名生成相应的文件。

webpack 模板
上面说的是一步一步使用 webpack 搭建开发环境，当然，实际应用中，大可以借用一些模板，比如 react hot boilerplate 这样的库。
