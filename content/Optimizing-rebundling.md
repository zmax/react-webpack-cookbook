You might notice after requiring React JS into your project that the time it takes from a save to a finished rebundle of your application takes more time. In development you ideally want from 200-800 ms rebundle speed, depending on what part of the application you are working on.

你可能注意到在引入 React JS 到你的項目之後，給你的應用重新合併會花費太多的時間。在開發環境中，最理想的是編譯最多 200 到 800 毫秒的速度，取決於你在開發的應用。

> IMPORTANT! This setup a minified, production version of React. As a result you will lose `propTypes` based type validation!

## 在開發環境中使用壓縮檔案

Instead of making Webpack go through React JS and all its dependencies, you can override the behavior in development.

為了不讓 Webpack 去遍歷 React JS 及其所有依賴，你可以在開發中重寫它的行為。

**webpack.config.js**

```javascript
var path = require('path');
var node_modules = path.resolve(__dirname, 'node_modules');
var pathToReact = path.resolve(node_modules, 'react/dist/react.min.js');

config = {
    entry: ['webpack/hot/dev-server', path.resolve(__dirname, 'app/main.js')],
    resolve: {
        alias: {
          'react': pathToReact
        }
    },
    output: {
        path: path.resolve(__dirname, 'build'),
        filename: 'bundle.js',
    },
    module: {
        loaders: [{
            test: /\.jsx?$/,
            loader: 'babel'
        }],
        noParse: [pathToReact]
    }
};

module.exports = config;
```

We do two things in this configuration:

我們在配置中做了兩件事：

1. Whenever "react" is required in the code it will fetch the minified React JS file instead of going to *node_modules*
2. Whenever Webpack tries to parse the minified file, we stop it, as it is not necessary

1. 每當 "react" 在代碼中被引入，它會使用壓縮後的 React JS 檔案，而不是到 *node_modules* 中找。
2. 每當 Webpack 嘗試去解析那個壓縮後的檔案，我們阻止它，因為這不必要。

Take a look at [Optimizing development](Optimizing-development) for more information on this.

可以到 [優化開發](Optimizing-development) 看到更多這方面的信息。
