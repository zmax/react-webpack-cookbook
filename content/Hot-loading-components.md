So this part is just freakin' awesome. With React JS and the react-hot-loader you can change the class code of your component and see the instances update live in the DOM, without loosing their state! This is pretty much exactly how CSS updates behave, only that it is your components.

所以這個章節就是他媽的屌。使用 React JS 和 react-hot-loader 可以讓你去改變組件中的 class 代碼，然後可以在 DOM 上看到實時更新了實例，沒有修改他們的狀態！看起來就像 CSS 更新一樣，不過是換成了組件。

## 設置
This setup requires that you use the **webpack-dev-server** as introduced in earlier chapters. Now we just have to install the loader with `npm install react-hot-loader --save-dev`, do a small config change:

這個設置需要你使用前面章節中介紹的 **webpack-dev-server**，現在我們需要去安裝加載器 `npm install react-hot-loader --save-dev`，然後做一點配置：

```javascript
var webpack = require('webpack');
var path = require('path');

var config = {
  entry: ['webpack/hot/dev-server', './app/main.js'],
  output: {
    path: path.resolve(__dirname, './build'),
    filename: 'bundle.js'
  },
  module: {
    loaders: [{
      test: /\.js$/,

      // Use the property "loaders" instead of "loader" and 
      // add "react-hot" in front of your existing "jsx" loader
      // 使用 "loaders" 屬性代替 "loader"
      // 然後在 "jsx" 加載器之前添加 "react-hot" 
      loaders: ['react-hot', 'babel']
    }]
  }
};

module.exports = config;
```

And you will also need a small snippet of code in your main entry file. In the example above that would be the *main.js* file located in the `app/` folder.

同時你也需要在你的主入口檔案做一些修改，例子中，在 `app/` 檔案夾中的 *main.js* 像下面那樣修改：

*app/main.js*
```javascript
// You probably already bring in your main root component, 
// maybe it is your component using react-router
// 你可能已經把你的根組件引入了
// 組件可能用了 react-router
var RootComponent = require('./RootComponent.jsx');

// When you render it, assign it to a variable
// 當你渲染它的時候，讓它賦值給一個變數
var rootInstance = React.render(RootComponent(), document.body);

// Then just copy and paste this part at the bottom of
// the file
// 然後在檔案的最底部複製粘帖
if (module.hot) {
  require('react-hot-loader/Injection').RootInstanceProvider.injectProvider({
    getRootInstances: function () {
      // Help React Hot Loader figure out the root component instances on the page:
      // 幫助 React Hot Loader 識別出頁面中的根組件
      return [rootInstance];
    }
  });
}

```

It is that simple. Render a component to the DOM and make a code change on the class of that component. It will render itself again, keeping the existing state. Cool?

就是這麼簡單，在 DOM 中渲染一個組件，然後修改一些組件中的代碼，它會自動渲染，卻保存了已經存在了的狀態，屌不屌？

Read more about the [react-hot-loader](http://gaearon.github.io/react-hot-loader/getstarted/).

可以到 [react-hot-loader](http://gaearon.github.io/react-hot-loader/getstarted/) 瞭解更多。