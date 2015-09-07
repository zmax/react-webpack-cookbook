It is also possible to lazy load entry points. This means that you load parts of your application as they are requested. A typical scenario for this would be that your users only visits specific parts of the application. And an example of that would be twitter.com. You do not always visit your profile page, so why load the code for that? Here is a summary of requirements:

Webpack 也可以實現懶加載入口檔案，意味著應用的一部分只在需要的時候加載，一個典型的例子是用戶只有訪問一些應用特定的部分，典型的例子是 Twitter.com，你不會一直訪問你的個人頁，所以為什麼要加載那部分的代碼？這裡有個主要的要求：

- You have a relatively big application where users can visit different parts of it
- You do care a lot about initial render time


- 你有一個相對比較大的應用，可以讓用戶可以訪問應用的不同部分。
- 你非常關注初始渲染時間


*webpack.production.config.js*
```javascript
var path = require('path');
var webpack = require('webpack');
var node_modules_dir = path.resolve(__dirname, 'node_modules');

var config = {
  entry: {
    app: path.resolve(__dirname, 'app/main.js'),
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
So we are pretty much back where we started with a split application and vendors bundle. You do not really define your lazy dependencies in a configuration, Webpack automatically understands them when analyzing your code. So let us see how we would lazy load a **profile page**:

所以我們把應用和第三方分離是一件非常漂亮的事，你不需要在配置中設置懶加載依賴，Webpack 會自動理解他們，然後分析你的代碼。所以讓我們看看我們是如何加載一個 **個人信息頁**：

*main.js (使用 ES6 語法)*
```javascript
import React from 'react';
import Feed from './Feed.js';

class App extends React.Component {
  constructor() {
    this.state = { currentComponent: Feed };
  }
  openProfile() {
    require.ensure([], () => {
      var Profile = require('./Profile.js');
      this.setState({
        currentComponent: Profile
      });
    });
  }
  render() {
   return (
      return <div>{this.state.currentComponent()}</div>
    );
  }
}
React.render(<App/>, document.body);
```
So this is just an example. You would probably hook this up to a router, but the important part is using `require.ensure`.

這只是一個例子，你需要把這些寫入到一個路由中，不過重要的事情是使用了 `require.ensure`。

**What is the array on the first argument?**: If you try to lazy load a chunk that depends on an other lazy loaded chunk you can set it as a dependency in the array. Just type in the path to the chunk. E.g. `['./FunnyButton.js']`

**第一個數組參數是什麼？**：如果你嘗試去懶加載一段由另一個懶加載的代碼加載的代碼的話，把它作為依賴寫在數組裡，就把路徑寫進去，比如 `['./FunnyButton.js']`
