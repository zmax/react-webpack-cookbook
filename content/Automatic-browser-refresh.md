当运行 **webpack-dev-server** 的时候，它会监听你的文件修改。当项目重新合并之后，会通知浏览器刷新。为了能够处罚这样的行为，你需要把你的 *index.html* 放到 `build/` 文件夹下，然后做这样的修改：

*build/index.html*
```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8"/>
  </head>
  <body>
    <script src="http://localhost:8080/webpack-dev-server.js"></script>
    <script src="bundle.js"></script>
  </body>
</html>
```

我们需要增加一个脚本当发生改动的时候去自动刷新应用，你需要在配置中增加一个入口点。

```javascript
var path = require('path');


module.exports = {
    entry: ['webpack/hot/dev-server', path.resolve(__dirname, 'app/main.js')],
    output: {
        path: path.resolve(__dirname, 'build'),
        filename: 'bundle.js',
    },
};
```

就是这样！现在你的应用就可以在文件修改之后自动刷新了。

## 默认环境

在上面的例子中我们创建了 *index.html* 文件来获取更多的自由和控制。同样也可以从 **http://localhost:8080/webpack-dev-server/bundle** 运行应用。这会触发一个默认的你不能控制的 *index.html* ，它同样会触发一个允许iFrame中显示重合并的过程。