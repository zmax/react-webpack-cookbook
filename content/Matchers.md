## 我可以使用哪種匹配器?

* `{ test: /\.js$/, loader: 'babel-loader' }` - Matches just .js
* `{ test: /\.(js|jsx)$/, loader: 'babel-loader' }` - Matches both js and jsx
* Generally put it's just a JavaScript regex so standard tricks apply


* `{ test: /\.js$/, loader: 'babel-loader' }` - 只匹配 .js
* `{ test: /\.(js|jsx)$/, loader: 'babel-loader' }` - 匹配 js 和 jsx
* 一般來說它就是一段 JavaScript 的正則，所以按照標準來即可