When **webpack-dev-server** is running it will watch your files for changes. When that happens it rebundles your project and notifies browsers listening to refresh. To trigger this behavior you need to change your *index.html* file in the `build/` folder.

當運行 **webpack-dev-server** 的時候，它會監聽你的檔案修改。當項目重新合併之後，會通知瀏覽器刷新。為了能夠觸發這樣的行為，你需要把你的 *index.html* 放到 `build/` 檔案夾下，然後做這樣的修改：

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

We added a script that refreshes the application when a change occurs. You will also need to add an entry point to your configuration:

我們需要增加一個腳本當發生改動的時候去自動刷新應用，你需要在配置中增加一個入口點。

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

Thats it! Now your application will automatically refresh on file changes.

就是這樣！現在你的應用就可以在檔案修改之後自動刷新了。

## 預設環境

In the example above we created our own *index.html* file to give more freedom and control. It is also possible to run the application from **http://localhost:8080/webpack-dev-server/bundle**. This will fire up a default *index.html* file that you do not control. It also fires this file up in an iFrame allowing for a status bar to indicate the status of the rebundling process.

在上面的例子中我們創建了 *index.html* 檔案來獲取更多的自由和控制。同樣也可以從 **http://localhost:8080/webpack-dev-server/bundle** 運行應用。這會觸發一個預設的你不能控制的 *index.html* ，它同樣會觸發一個允許iFrame中顯示重合併的過程。