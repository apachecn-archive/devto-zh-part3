# 使用 React、Webpack 4、Babel 7、Express 和 Sass 创建节点应用程序

> 原文：<https://dev.to/kedar9/creating-a-node-app-with-react-webpack-4-babel-7-express-and-sass-3mae>

[![By your powers combined...](img/fc2fe460e1d9db973b2fa993d0a5f6c4.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--z9DieRHX--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/jrcn8xq3e7h2cmx54qjq.jpg)

## 🏁序言

🆕创建一个新目录。让我们称之为反应样板。
`mkdir react-boilerplate`

还有 *cd* 放进去。
`cd react-boilerplate`

确保您安装了节点和 npm。运行这些命令来确认:
`node -v`
`npm -v`
如果你没有其中任何一个，点击[这里](https://www.npmjs.com/get-npm)并首先安装它们。

🎬现在，初始化节点项目。
`npm init`

✨你会被提示输入一些基本信息。输入并完成后，您的文件夹中应该有一个 **package.json** 文件。

# 🌲第一章:生命之树

### 1.1 快递

首先，我们做一个服务器。我们正在使用 **Express.js** 框架，这样我们就可以设计我们的服务器，处理我们的路线并构建 RESTful APIs。

如果处理路由和 API 不是您的需求，您仍然可以使用 Express，或者为了使事情更简单，您可以查看[*web pack-dev-server*](https://github.com/webpack/webpack-dev-server)。

📦安装 Express.js:
`npm install --save express`

✨应该会自动创建一个名为 **node_modules** 的文件夹。我们在项目中安装的任何东西都将驻留在该文件夹中。

🆕是时候写服务器了。创建一个名为 **server** 的新文件夹。在这个新文件夹中，创建一个文件 **index.js** 。将这个基本的最小代码添加到文件中:

```
const express = require('express');
const app = express();
const port = process.env.PORT || 3000;
const mockResponse = {
  foo: 'bar',
  bar: 'foo'
};
app.get('/api', (req, res) => {
  res.send(mockResponse);
});
app.get('/', (req, res) => {
 res.status(200).send('Hello World!');
});
app.listen(port, function () {
 console.log('App listening on port: ' + port);
}); 
```

Enter fullscreen mode Exit fullscreen mode

这允许应用程序在指定的端口上运行。
代码还告诉应用程序 home route:“/”应该返回状态 200(成功)并发送文本:Hello World。够基础！
它还有一个 route“/API”，返回一个虚拟 JSON 对象。它展示了获取数据是如何工作的。

⭐️:记住路线的顺序很重要。当请求通过时，Express 从顶部开始匹配路由。当它匹配某个路由时，将返回响应，并且不会检查进一步的路由。

✏️现在服务器已经设置好了，在 package.json 文件中，在`"scripts"`下，添加一个`start`命令如下:

```
"scripts": {
  "start": "node server/index.js",
  "test": "echo \"Error: no test specified\" && exit 1"
} 
```

Enter fullscreen mode Exit fullscreen mode

这里，我们告诉 Node，要启动项目，从 server/index.js 开始。

🏃🏻‍♂️If:你现在运行`npm run start`命令，你应该在终端上得到一条消息:
“app listening on port:3000”。

🎉现在，在你的浏览器上进入 [http://localhost:3000](http://localhost:3000) ，这里应该会显示**“你好，世界”**消息。转到[http://localhost:3000/API](http://localhost:3000/api)，伪 JSON 应该会出现。

### 1.2 网络包

📦安装时间

*   网络包(捆绑器)
*   webpack-cli(能够运行 webpack 命令的命令行界面)

`npm install --save-dev webpack webpack-cli`

在 **package.json** 文件中的`"scripts"`下，添加一个`build`命令:

```
"scripts": {
  "start": "node server/index.js",
  "build": "webpack --mode production",
  "test": "echo \"Error: no test specified\" && exit 1"
} 
```

Enter fullscreen mode Exit fullscreen mode

🆕现在，创建一个名为 **src** 的文件夹。这是我们项目的大部分源代码存在的地方。在这个新文件夹 src 中，创建一个文件 **index.js** 。
暂时将文件留空。

🏃🏻‍♂️If 你运行`npm run build`命令，它会创建一个 **dist** 文件夹，里面有一个捆绑的 **main.js** 文件。其中的代码将被精简以供生产使用。

# 🛰️第二章:暮色臭氧

### 2.1 通天塔

🤗反应过来拥抱 JSX。(尽管 JS 也能很好地工作)。
Babel 帮助将 JSX 语法编译成 JS。
♻️不仅如此，而且还是常客。js 文件，我们现在可以使用 ES6 语法，Babel 会把它编译成等价的 ES5 格式。

📦安装

*   @babel/core(将 ES6+代码转化为 ES5)
*   @babel/preset-env(预设为允许多填充)
*   @babel/preset-react(为 react 和 JSX 预设)
*   babel loader(web pack helper)

`npm install --save-dev @babel/core @babel/preset-env @babel/preset-react babel-loader`

这 4 个新包中，有 2 个是*预置*。巴别塔核心需要知道自己有这些插件。需要对它们进行详细说明。

🆕在项目的根级别，创建一个**。babelrc** 文件。并将预置指定为一个数组，如下所示:

```
{
  "presets": ["@babel/preset-env", "@babel/preset-react"]
} 
```

Enter fullscreen mode Exit fullscreen mode

如果您的代码需要多填充，您还需要这些节点包:`core-js`和`regenerator-runtime`。更多关于[这里](https://github.com/zloirock/core-js/blob/master/docs/2019-03-19-core-js-3-babel-and-a-look-into-the-future.md)。

### 2.2 巴别塔+网络包

基于 Babel 的功能，Webpack 需要知道这一点。js 和。jsx 文件在捆绑之前需要经过 Babel。因此，我们需要为该规则配置 Webpack。

🆕在项目的根级别，创建一个 **webpack.config.js** 文件。这将是所有 webpack 配置的文件。像这样添加规则:

```
module.exports = {
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader"
        }
      }
    ]
  }
}; 
```

Enter fullscreen mode Exit fullscreen mode

⭐️记住:由于 webpack 捆绑了所有的东西并创建了一个简单的浏览器可读代码，所有你安装的包、预置和插件都需要在 webpack 中配置。

# 🏖️第三章:乌托邦

### 3.1 反应过来

是时候安装 react 和 react-dom 了。
`npm install --save react react-dom`

🆕在文件夹 **src/** 中，新建一个文件**index.html**。这将是我们的项目中的主要和唯一的 HTML 文件。它就像你制作的任何常规 HTML 文件一样，只有一点不同:它需要在`<body>`中有一个`<div>`，React 可以*填充*。
🔍它需要 React 可以查找的某种形式的标识符。我们将`id="root"`设定在`div`上。您可以将 id 设置为您想要的任何值。
下面是一个简单的 index.html 和`<div id="root"></div>`的样子:

```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  React Boilerplate
</head>
<body>
  <div id="root"></div>
</body>
</html> 
```

Enter fullscreen mode Exit fullscreen mode

✏️还记得我们在第 1.2 节创建的那个空的 **src/index.js** 文件吗？
添加代码的时间:

```
import React from 'react';
import ReactDOM from 'react-dom';
const Index = () => {
  return <div>Welcome to React!</div>;
};
ReactDOM.render(<Index />, document.getElementById('root')); 
```

Enter fullscreen mode Exit fullscreen mode

⚡️Here，`Index`是一个返回 React 元素的函数组件。ReactDOM 在来自 index.html**的`<div id="root"></div>`内渲染。**

### 3.2 试剂+网页包

类似于我们所做的。js 和。jsx 文件，我们需要告诉 Webpack 如何处理新的。html 文件。Webpack 需要将其捆绑到 **dist** 文件夹中。

📦为此，我们安装 html-webpack-plugin。
`npm install --save-dev html-webpack-plugin`

✏️需要更新 webpack 配置文件来处理这个插件。我们还告诉 webpack，现在编码的 **src/index.js** 是入口点。
这是添加后的配置文件的样子:

```
const HtmlWebPackPlugin = require("html-webpack-plugin");
const path = require('path');
const htmlPlugin = new HtmlWebPackPlugin({
  template: "./src/index.html", 
  filename: "./index.html"
});
module.exports = {
  entry: "./src/index.js",
  output: { // NEW
    path: path.join(__dirname, 'dist'),
    filename: "[name].js"
  }, // NEW Ends
  plugins: [htmlPlugin],
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader"
        }
      }
    ]
  }
}; 
```

Enter fullscreen mode Exit fullscreen mode

当实例化`htmlPlugin`时，`template`选项告诉 webpack 选择什么文件，而`filename`选项告诉捆绑包的名称。dist 文件夹中的 html 文件。

### 3.3 反应+表达

🏃🏻‍♂️:如果你现在运行`npm run start`，我们仍然会在本地主机上收到**“hello world”**消息。这是因为 Express 服务器不知道这些新文件。

✏️在 package.json 中，`start`脚本只是启动服务器。
我们现在需要 webpack 来打包我们的文件，然后启动服务器。
在`"scripts"`下，增加一个新的`dev`命令。

```
"scripts": {
  "start": "node server/index.js",
  "dev": "webpack --mode development && node server/index.js",
  "build": "webpack --mode production",
  "test": "echo \"Error: no test specified\" && exit 1"
} 
```

Enter fullscreen mode Exit fullscreen mode

我们现在应该更新 Express 并更改路由“/”返回的内容。它应该返回 **dist/index.html** 文件。

✏️在 server/index.js 中进行更新(代码的新部分以注释结尾:`// NEW` ):

```
const express = require('express');
const path = require('path'); // NEW

const app = express();
const port = process.env.PORT || 3000;
const DIST_DIR = path.join(__dirname, '../dist'); // NEW
const HTML_FILE = path.join(DIST_DIR, 'index.html'); // NEW
const mockResponse = {
  foo: 'bar',
  bar: 'foo'
};
app.use(express.static(DIST_DIR)); // NEW
app.get('/api', (req, res) => {
  res.send(mockResponse);
});
app.get('/', (req, res) => {
 res.sendFile(HTML_FILE); // EDIT
});
app.listen(port, function () {
 console.log('App listening on port: ' + port);
}); 
```

Enter fullscreen mode Exit fullscreen mode

🎉现在运行`npm run dev`，在浏览器上进入 [http://localhost:3000](http://localhost:3000) 。**“欢迎反应过来！”**来自 **src/index.js** 的消息应该会显示在那里。“/api”路径仍然像以前一样工作。

# 🍵第 4 章:底线绿色

### 4.1 萨斯

是时候让东西看起来更好了。安装 node-sass 和所需加载程序的时间:样式加载程序、css 加载程序和 sass 加载程序。

`npm install --save-dev node-sass style-loader css-loader sass-loader`

🆕在 **src/** 文件夹下新建一个文件 **styles.scss** 。向该文件添加一些样式。

下面是一个在页面上使用系统字体的基本(也是流行的)代码。我们还设置了它的颜色属性。

```
body {
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI",
  Helvetica, Roboto, Arial, sans-serif;
  color: brown;
} 
```

Enter fullscreen mode Exit fullscreen mode

这将是一个添加顶级和/或全局样式的好文件。

✏️导入新的 **styles.scss** 文件。你可以把它添加到 index.html 的**或者 index.js 文件中。为了计划一致性，我们将其导入到 **index.js** 文件:** 

```
import React from 'react';
import ReactDOM from 'react-dom';
import './styles.scss';
const Index = () => {
  return <div>Welcome to React!</div>;
};
ReactDOM.render(<Index />, document.getElementById('app')); 
```

Enter fullscreen mode Exit fullscreen mode

### 4.2 Sass + Webpack

像其他事情一样，Webpack 需要知道如何处理**。scss** 文件，将它们正确地捆绑到浏览器可理解的代码中。

✏️在 **webpack.config.js** 中，添加了新规则，就像我们为 babel-loader 添加的规则一样。所以，在`module.exports`中的`module`对象的`rules`数组中，加上:

```
{
  test: /\.s?css$/,
  use: ['style-loader', 'css-loader', 'sass-loader']
} 
```

Enter fullscreen mode Exit fullscreen mode

🎉现在运行`npm run dev`，在浏览器上进入 [http://localhost:3000](http://localhost:3000) 。“欢迎反应过来的**！消息"**应该在系统中以棕色字体显示。

## ⌛后记

### 🖇️反应过来的成分

React 项目由许多较小的组件组成。 **src/index.js** 中的`Index`就是这样一个组件。您将创建更多这样的组件并将它们导入(到另一个中🤨).

📂我建议在 **src/** 文件夹中创建一个名为 **components/** 的文件夹。并将所有其他组件存储在该文件夹中。

理想情况下，在 **components/** 中，为每个组件创建一个子文件夹。
但这取决于个人喜好！

💡不要忘记:React 组件文件应该导出组件`Class`或`function`。
一旦你给 **src/index.js** 添加一些组件，它看起来会像这样:

```
import React from 'react';
import ReactDOM from 'react-dom';
import Header from './components/Header/index.jsx';
import Content from './components/Content/index.jsx';
const Index = () => {
  return (
    <div className="container">
      <Header />
      <Content />
    </div>
  );
};
ReactDOM.render(<Index />, document.getElementById('app')); 
```

Enter fullscreen mode Exit fullscreen mode

### 🔧附加 Webpack 配置

像其他文件一样，图像或任何其他静态文件也需要捆绑。Webpack 需要知道这一点。
📦为所有这样的文件安装`file-loader`作为 devDependency ( `--save-dev`)。
并在 **webpack.config.js** 中添加以下规则:

```
{
  test: /\.(png|svg|jpg|gif)$/,
  loader: "file-loader",
  options: { name: '/static/[name].[ext]' }
} 
```

Enter fullscreen mode Exit fullscreen mode

在上面的代码中，测试正则表达式只指定了一般的图像扩展。但是，您也可以为其他文件添加任何扩展名(在同一个规则对象中)。

要在组件中使用图像或任何其他资产，需要先在 that.js/.jsx 文件中导入它。因此，Webpack 可以正确地捆绑它，并使它在捆绑的文件夹中可用。您可以使用文件的实际`[name]`或`[hash]`来创建它。有没有文件`[ext]`。

```
// Import
import LOGO from '<path-to-file>/logo.png';

...

// Usage
<img src={LOGO} alt="Page Logo" /> 
```

Enter fullscreen mode Exit fullscreen mode

### 🙅🏼‍♂️饭桶，别理他！

对于部署，Heroku 或 Netlify 等节点兼容平台在您的应用程序中运行`build`命令。这将安装所有的 **node_modules** ，并生成 **dist** 文件夹及其内容。
所以，我们不需要将本地文件夹: **node_modules** 和 **dist** 推送到 remote。

🆕为了让 Git 知道这一点，创建一个新文件**。项目根级别上的 gitignore** 。
你想让 Git 忽略的任何东西都可以在这里添加。这里有一个基本版本:

```
# Deployment directories
node_modules
dist
# Optional npm cache directory
.npm
# Mac
.DS_Store 
```

Enter fullscreen mode Exit fullscreen mode

🍺这就结束了设置。这个项目可以作为任何未来 React w/ server 应用程序甚至独立 Express 项目的样板。

👍🏼感谢你一路看完这篇长文。用 Webpack 和 Babel 和 Express 设置一个无错误的 Node app 绝对不是一件轻而易举的事情。但希望这篇文章对你有所帮助。

🌏加油星球！