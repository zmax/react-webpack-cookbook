When users hit the URL of your application they will need to download different assets. CSS, JavaScript, HTML, images and fonts. The great thing about Webpack is that you can stop thinking how you should download all these assets. You can do it through JavaScript.

當用戶輸入你應用的地址的時候，他們需要去下載不同的資源，比如 CSS、JavaScript、HTML、圖片和字型。不過 Webpack 做了一件事情，讓你不用去考慮如何不用下載全部資源。

> OccurenceOrderPlugin


## 如何讓生產輸出附上哈希值？

* Use `[hash]`. Example: `'assets/bundle.[hash].js'`
* 使用 `[hash]`。比如：`'assets/bundle.[hash].js'`

The benefit of this is that this will force the client to reload the file. There is more information about `[hash]` at [the long term caching](http://webpack.github.io/docs/long-term-caching.html) section of the official documentation.

這個的好處是能夠讓客戶端強制重新加載這個檔案，可以在 [the long term caching](http://webpack.github.io/docs/long-term-caching.html) 瞭解更多關於 `[hash]`，

> Is it possible to change the hash only if bundle changed?
> 有可能只有合併檔案變化了才會修改哈希值麼？