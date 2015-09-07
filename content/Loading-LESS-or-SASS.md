If you want to use compiled CSS, there are two loaders available for you. The **less-loader** and the **sass-loader**. Depending on your preference, this is how you set it up.

如果你想使用編譯 CSS，這裡有兩種可用的加載器：**less-loader** 和 **sass-loader**，看你喜歡哪種。下面是如何設置。

## 安裝和設置加載器

`npm install less-loader` or `npm install sass-loader`.

**webpack.config.js**

```javascript
var path = require('path');
var config = {
  entry: path.resolve(__dirname, 'app/main.js')
  output: {
    path: path.resolve(__dirname, 'build'),
    filename: 'bundle.js'
  },
  module: {
    loaders: [{
      test: /\.jsx$/,
      loader: 'babel'
    },

    // LESS
    {
      test: /\.less$/,
      loader: 'style!css!less'
    },

    // SASS
    {
      test: /\.scss$/,
      loader: 'style!css!sass'
    }]
  }
};
```

## LESS 和 SASS 中的 imports 怎麼辦?

If you import one LESS/SASS file from an other, use the exact same pattern as anywhere else. Webpack will dig into these files and figure out the dependencies.

如果你從另外一個檔案中導入一個 LESS/SASS 檔案，像其他地方一樣使用準確的路徑，Webpack 會找出那些檔案，然後識別裡面的依賴。

```less
@import "./variables.less";
```

You can also load LESS files directly from your node_modules directory.

你也可以直接從你的 node_modules 檔案夾中加載 LESS 檔案。

```less
$import "~bootstrap/less/bootstrap";
```
