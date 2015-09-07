When your application is depending on other libraries, especially large ones like React JS, you should consider splitting those dependencies into its own vendors bundle. This will allow you to do updates to your application, without requiring the users to download the vendors bundle again. Use this strategy when:

當你的應用依賴其他庫尤其是像 React JS 這種大型庫的時候，你需要考慮把這些依賴分離出去，這樣就能夠讓用戶在你更新應用之後不需要再次下載第三方檔案。當滿足下面幾個情況的時候你就需要這麼做了：

- When your vendors reaches a certain percentage of your total app bundle. Like 20% and up 
- You will do quite a few updates to your application
- You are not too concerned about perceived initial loading time, but you do have returning users and care about optimizing the experience when you do updates to the application
- Users are on mobile


- 當你的第三方的體積達到整個應用的 20% 或者更高的時候。
- 更新應用的時候只會更新很小的一部分
- 你沒有那麼關注初始加載時間，不過關注優化那些回訪用戶在你更新應用之後的體驗。
- 有手機用戶。

*webpack.production.config.js*
```javascript
var path = require('path');
var webpack = require('webpack');
var node_modules_dir = path.resolve(__dirname, 'node_modules');

var config = {
  entry: {
    app: path.resolve(__dirname, 'app/main.js'),
    
    // Since react is installed as a node module, node_modules/react,
    // we can point to it directly, just like require('react');
    // 當 React 作為一個 node 模組安裝的時候，
    // 我們可以直接指向它，就比如 require('react')
    vendors: ['react']
  },
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'app.js'
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
This configuration will create two files in the `dist/` folder. **app.js** and **vendors.js**.

這些配置會在 `dist/` 檔案夾下創建兩個檔案：**app.js** 和 **vendors.js**。

#### 重要的事情！
Remember to add both files to your HTML file, or you will get the error: `Uncaught ReferenceError: webpackJsonp is not defined`.

記住要把這些檔案都加入到你的 HTML 代碼中，不然你會得到一個錯誤：`Uncaught ReferenceError: webpackJsonp is not defined`。