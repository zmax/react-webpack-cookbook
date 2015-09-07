Fonts can be really difficult to get right. First of all we have typically 4 different formats, but only one of them will be used by the respective browser. You do not want to inline all 4 formats, as that will just bloat your CSS file and in no way be an optimization.

字型實在是非常難引入正確，首先，通常我們有 4 種不一樣的格式，但是只有其中一種會被對應的瀏覽器使用到。你肯定不會想引入全部四種格式，這樣只會讓 CSS 檔案更加膨脹，然後又沒辦法優化。

## 選擇一種格式

Depending on your project you might be able to get away with one font format. If you exclude Opera Mini, all browsers support the .woff and .svg format. The thing is that fonts can look a little bit different in the different formats, on the different browsers. So try out .woff and .svg and choose the one that looks the best in all browsers.

取決與你的項目，你可能可以選擇出一種字型格式，如果你不考略 Opera Mini，所有的瀏覽器都支持 .woff 和 .svg 格式。問題是不同格式下在各種瀏覽器下字型看起來會有一點點不同。所以測試 .woff 和 .svg，然後找出能夠在所有瀏覽器中看起來最好的那個。

There are probably other strategies here too, so please share by creating an issue or pull request.

如果有其他更好的策略，那請通過創建 issue 或者提 PullRequest 來分享。

## 實踐

You do this exactly like you do when inlining images.

就像內聯圖片一樣來內聯字型。

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
    }, {
      test: /\.woff$/,
      loader: 'url?limit=100000'
    }]
  }
};
```

Just make sure you have a limit above the size of the fonts, or they will of course not be inlined.
