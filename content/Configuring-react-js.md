## 安装 React JS

`npm install react --save`

没什么好讲的，接下来就可以在你的代码中使用 React JS 了。

## 在代码中使用 ReactJS

**component.jsx**

```javascript
import React from 'react';

export default class Hello extends React.Component {
  render() {
    return <h1>Hello world</h1>;
  }
}
```

**main.js**

```javascript
import React from 'react';
import Hello from './component';

main();

function main() {
    React.render(<Hello />, document.getElementById('app'));
}
```

**build/index.html**

```javascript
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8"/>
  </head>
  <body>
    <div id="app"></div>

    <script src="http://localhost:8080/webpack-dev-server.js"></script>
    <script src="bundle.js"></script>
  </body>
</html>
```

## 转换 JSX

为了能够使用 JSX 语法，你需要用 Webpack 来转码你的 Javascript，这是加载器的工作，我们可以使用一个很好用也有很多功能的 [Babel](https://babeljs.io/)。

`npm install babel-loader --save-dev`

现在我们需要去配置 Webpack 来使用加载器。

*webpack.config.js*
```javascript
var path = require('path');
var config = {
  entry: path.resolve(__dirname, 'app/main.js'),
  output: {
    path: path.resolve(__dirname, 'build'),
    filename: 'bundle.js'
  },
  module: {
    loaders: [{
      test: /\.jsx?$/, // 用正则来匹配文件路径，这段意思是匹配 js 或者 jsx
      loader: 'babel' // 加载模块 "babel" 是 "babel-loader" 的缩写
    }]
  }
};

module.exports = config;
```
Webpack 会在你的项目中测试所有路径，如果我们项目中使用 ES6 模块加载器语法，比如 `import MyComponent from './Component.jsx';` 是回去匹配 `'./Component.jsx'`。

在命令行中运行 `npm run dev`，然后刷新页面就可以看到修改。
