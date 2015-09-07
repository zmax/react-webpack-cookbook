Lets have a look at the simplest setup you can create for your application. Use a single bundle when:

讓我們看一下為應用創建的最簡單的配置，只有在下面的情況下才使用單入口模式：

- You have a small application
- You will rarely update the application
- You are not too concerned about perceived initial loading time


- 應用很小
- 很少會更新應用
- 你不太關心初始加載時間


*webpack.production.config.js*
```javascript
var path = require('path');
var config = {
  entry: path.resolve(__dirname, 'app/main.js'),
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js'
  },
  module: {
    loaders: [{
      test: /\.js$/,
      loader: 'babel'
    }]
  }
};

module.exports = config;
```