## 安裝 React JS

`npm install react --save`

There is really nothing more to it. You can now start using React JS in your code.

沒什麼好講的，接下來就可以在你的代碼中使用 React JS 了。

## 在代碼中使用 ReactJS

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

## 轉換 JSX

To use the JSX syntax you will need webpack to transform your JavaScript. This is the job of a loader. We'll use [Babel](https://babeljs.io/) as it's nice and has plenty of features.

為了能夠使用 JSX 語法，你需要用 Webpack 來轉碼你的 JavaScript，這是加載器的工作，我們可以使用一個很好用也有很多功能的 [Babel](https://babeljs.io/)。

`npm install babel-loader --save-dev`

Now we have to configure webpack to use this loader.

現在我們需要去配置 Webpack 來使用加載器。

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
      test: /\.jsx?$/, // 用正則來匹配檔案路徑，這段意思是匹配 js 或者 jsx
      loader: 'babel' // 加載模組 "babel" 是 "babel-loader" 的縮寫
    }]
  }
};

module.exports = config;
```

Webpack will test each path required in your code. In this project we are using ES6 module loader syntax, which means that the require path of `import MyComponent from './Component.jsx';` is `'./Component.jsx'`.

Webpack 會在你的項目中測試所有路徑，如果我們項目中使用 ES6 模組加載器語法，比如 `import MyComponent from './Component.jsx';` 是會去匹配 `'./Component.jsx'`。

Run `npm run dev` in the console and refresh the page to see something.

在命令行中運行 `npm run dev`，然後刷新頁面就可以看到修改。