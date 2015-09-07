Until HTTP/2 is here you want to avoid setting up too many HTTP requests when your application is loading. Depending on the browser you have a set number of requests that can run in parallel. If you load a lot of images in your CSS it is possible to automatically inline these images as BASE64 strings to lower the number of requests required. This can be based on the size of the image. There is a balance of size of download and number of downloads that you have to figure out for your project, and Webpack makes that balance easy to adjust.

直到 HTTP/2 你才能在應用加載的時候避免設置太多 HTTP 請求。根據瀏覽器不同你必須設置你的並行請求數，如果你在你的 CSS 中加載了太多圖片的話，可以自動把這些圖片轉成 BASE64 字元串然後內聯到 CSS 裡來降低必要的請求數，這個方法取決與你的圖片大小。你需要為你的應用平衡下載的大小和下載的數量，不過 Webpack 可以讓這個平衡十分輕鬆適應。

## 安裝 url-loader
`npm install url-loader --save-dev` will install the loader that can convert resolved paths as BASE64 strings. As mentioned in other sections of this cookbook Webpack will resolve "url()" statements in your CSS as any other require or import statements. This means that if we test on image file extensions for this loader we can run them through it.

`npm install url-loader --save-dev` 來安裝加載器，它會把需要轉換的路徑變成 BASE64 字元串，在其他的 Webpack 書中提到的這方面會把你 CSS 中的 “url()” 像其他 require 或者 import 來處理。意味著如果我們可以通過它來處理我們的圖片檔案。

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
      loader: 'jsx'
    }, {
      test: /\.(png|jpg)$/,
      loader: 'url?limit=25000'
    }]
  }
};
```
The limit is an argument passed to the url-loader. It tells it that images that er 25KB or smaller in size will be converted to a BASE64 string and included in the CSS file where it is defined.

url-loader 傳入的 limit 參數是告訴它圖片如果不大於 25KB 的話要自動在它從屬的 css 檔案中轉成 BASE64 字元串。