Maybe you are building an application that has multiple urls. An example of this would be a solution where you have two, or more, different URLs responding with different pages. Maybe you have one user page and one admin page. They both share a lot of code, but you do not want to load all the admin stuff for normal users. That is a good scenario for using multiple entry points. A list of use cases could be:

你的應用可能有多個路徑， 就是應用中有兩個或者多個 URL 相應不同的頁面，這裡就是提供這樣的解決方案。可能你有一個普通用戶頁和一個管理員頁，他們共享了很多代碼，但是不想在普通用戶頁中加載所有管理員頁的代碼，所以好方案是使用多重入口。使用緣由有下面幾條：

- You have an application with multiple isolated user experiences, but they share a lot of code
- You have a mobile version using less components
- You have a typical user/admin application where you do not want to load all the admin code for a normal user


- 你的應用有多種不同的用戶體驗，但是他們共享了很多代碼。
- 你有一個使用更少組件的手機版本
- 你的應用是典型的權限控制，你不想為普通用戶加載所有管理用戶的代碼。

Let us create an example with a mobile experience using less components:

讓我們創建一個使用更少組件的手機頁面的例子：

*webpack.production.config.js*
```javascript
var path = require('path');
var webpack = require('webpack');
var node_modules_dir = path.resolve(__dirname, 'node_modules');

var config = {
  entry: {
    app: path.resolve(__dirname, 'app/main.js'),
    mobile: path.resolve(__dirname, 'app/mobile.js'),
    vendors: ['react'] // 其他庫
  },
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: '[name].js' // 注意我們使用了變數
  },
  module: {
    loaders: [{
      test: /\.js$/,
      exclude: [node_modules_dir],
      loader: 'babel'
    }]
  },
  plugins: [
    new webpack.optimize.CommonsChunkPlugin('vendors', 'vendors.js')
  ]
};

module.exports = config;
```
This configuration will create three files in the `dist/` folder. **app.js**, **mobile.js** and **vendors.js**. Most of the code in the **mobile.js** file also exists in **app.js**, but that is what we want. We will never load **app.js** and **mobile.js** on the same page.

這個配置會在 `dist/` 檔案夾下創建三個檔案：**app.js**、**mobile.js**和**vendors.js**，大部分的代碼在**mobile.js**檔案中，也有一部分在 **app.js** 中，不過這是我們需要的，我們不會在同一個頁面中同時加載 **app.js** 和 **mobile.js**。