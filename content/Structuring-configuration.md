There are two things you want to do preparing for a production build.

這裡有兩件事你需要為生產發佈做準備。

1. Configure a script to run in your package.json file
2. Create a production config


1. 配置你的 package.json 裡的腳本
2. 創建一個生產的配置


### 創建腳本
We have already used *package.json* to create the `npm run dev` script. Now let us set up `npm run deploy`.

我們已經使用過 *package.json* 來創建 `npm run dev` 的腳本，現在讓我們設置 `npm run deploy`。

```json
{
  "name": "my-project",
  "version": "0.0.0",
  "description": "My awesome project!",
  "main": "app/main.js",
  "scripts": {
    "dev": "webpack-dev-server --devtool eval --progress --colors --hot --content-base build",
    "deploy": "NODE_ENV=production webpack -p --config webpack.production.config.js"
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "webpack": "^1.4.13",
    "webpack-dev-server": "^1.6.6"
  },
  "dependencies": {}
}
```

As you can see we are just running webpack with the production argument and pointing to a different configuration file. We also use the environment variable "production" to allow our required modules to do their optimizations. Lets us create the config file now.

正如你所見，我們只是用生產參數運行 Webpack 來指向另一個配置檔案。我們也使用了環境變數 “production” 來讓我們的模組自動去優化。讓我們開始來創建配置檔案。

### 創建生產配置
So there really is not much difference in creating the dev and production versions of your webpack config. You basically point to a different output path and there are no workflow configurations or optimizations. What you also want to bring into this configuration is cache handling.

可以看到，其實生產環境的配置和開發的配置沒有太大的不同，主要的不同是指向了一個不同的輸出路徑，然後也沒有了 workflow 的配置和優化，可以看到新加入到配置裡的是處理緩存的配置。

```javascript
var path = require('path');
var node_modules_dir = path.resolve(__dirname, 'node_modules');

var config = {
  entry: path.resolve(__dirname, 'app/main.js'),
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js'
  },
  module: {
    loaders: [{
      test: /\.js$/,
      
      // There is not need to run the loader through
      // vendors
      // 這裡再也不需通過任何第三方來加載
      exclude: [node_modules_dir],
      loader: 'babel'
    }]
  }
};

module.exports = config;
```

### 發佈
Run `npm run deploy` in the root of the project. Webpack will now run in production mode. It does some optimizations on its own, but also React JS will do its optimizations. Look into caching for even more production configuration.

在項目根目錄處運行 `npm run deploy`，Webpack 現在會運行生產模式，他會自動做一些優化，不過，React Js 也會做自己的優化。可以深入瞭解緩存處理來做更多的生產配置。
