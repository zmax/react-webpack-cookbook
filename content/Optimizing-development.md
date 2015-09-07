We talked about how you could use the minified versions of your dependencies in development to make the rebundling go as fast as possible. Let us look at a small helper you can implement to make this a bit easier to handle.

之前介紹了如何在開發中使用依賴的壓縮版本來讓合併儘可能加速，讓我們看一下這個小的例子來讓你更加輕鬆去處理：

*webpack.config.js*
```javascript
var webpack = require('webpack');
var path = require('path');
var node_modules_dir = path.join(__dirname, 'node_modules');

var deps = [
  'react/dist/react.min.js',
  'react-router/dist/react-router.min.js',
  'moment/min/moment.min.js',
  'underscore/underscore-min.js',
];

var config = {
  entry: ['webpack/hot/dev-server', './app/main.js'],
  output: {
    path: path.resolve(__dirname, './build'),
    filename: 'bundle.js'
  },
  resolve: {
    alias: {}
  },
  module: {
    noParse: [],
    loaders: []
  }
};

// Run through deps and extract the first part of the path, 
// as that is what you use to require the actual node modules 
// in your code. Then use the complete path to point to the correct
// file and make sure webpack does not try to parse it
// 通過在第一部分路徑的依賴和解壓
// 就是你像引用 node 模組一樣引入到你的代碼中
// 然後使用完整路徑指向當前檔案，然後確認 Webpack 不會嘗試去解析它

deps.forEach(function (dep) {
  var depPath = path.resolve(node_modules_dir, dep);
  config.resolve.alias[dep.split(path.sep)[0]] = depPath;
  config.module.noParse.push(depPath);
});

module.exports = config;
```
Not all modules include a minified distributed version of the lib, but most do. Especially with large libraries like React JS you will get a significant improvement.

不是所有的模組需要一個壓縮的版本，不過大多數需要，尤其是像 React JS 這種大型庫，之後你會有明顯的提升。

## 把 React 暴露到全局中
You might be using distributed versions that requires React JS on the global scope. To fix that you can install the expose-loader by `npm install expose-loader --save-dev` and set up the following config, focusing on the *module* property:

你可能在全局中使用了一個壓縮版本的 React，為了修復你可以安裝這個暴露全局加載器 `npm install expose-loader --save-dev`，然後像下面這樣配置，注意 *module* 屬性：

```javascript
var webpack = require('webpack');
var path = require('path');
var node_modules_dir = path.join(__dirname, 'node_modules');

var deps = [
  'react/dist/react.min.js',
  'react-router/dist/react-router.min.js',
  'moment/min/moment.min.js',
  'underscore/underscore-min.js',
];

var config = {
  entry: ['webpack/hot/dev-server', './app/main.js'],
  output: {
    path: path.resolve(__dirname, './build'),
    filename: 'bundle.js'
  },
  resolve: {
    alias: {}
  },
  module: {
    noParse: [],

    // Use the expose loader to expose the minified React JS
    // distribution. For example react-router requires this
    // 使用暴露全局加載器來暴露壓縮版的 React JS，比如 react-router 需要這個。
    loaders: [{
      test: path.resolve(node_modules_dir, deps[0]),
      loader: "expose?React"
    }]
  }
};

deps.forEach(function (dep) {
  var depPath = path.resolve(node_modules_dir, dep);
  config.resolve.alias[dep.split(path.sep)[0]] = depPath;
  config.module.noParse.push(depPath);
});

module.exports = config;
```