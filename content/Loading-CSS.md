Webpack allows you to load CSS like you load any other code. What strategy you choose is up to you, but you can do everything from loading all your css in the main entry point file to one css file for each component.

Webpack允許像加載任何代碼一樣加載 CSS。你可以選擇你所需要的方式，但是你可以為每個組件把所有你的 CSS 加載到入口主檔案中來做任何事情。

Loading CSS requires the **css-loader** and the **style-loader**. They have two different jobs. The **css-loader** will go through the CSS file and find `url()` expressions and resolve them. The **style-loader** will insert the raw css into a style tag on your page.

加載 CSS 需要 **css-loader** 和 **style-loader**，他們做兩件不同的事情，**css-loader**會遍歷 CSS 檔案，然後找到 `url()` 表達式然後處理他們，**style-loader** 會把原來的 CSS 代碼插入頁面中的一個 style 標籤中。

## 準備加載 CSS

Install the two loaders: `npm install css-loader style-loader --save-dev`.

安裝這兩個加載器：`npm install css-loader style-loader --save-dev`

In the *webpack.config.js* file you can add the following loader configuration:

你可以把下面的加載器配置加到 *Webpack.config.js* 檔案中。

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
    }, {
      test: /\.css$/, // Only .css files
      loader: 'style!css' // Run both loaders
    }]
  }
};

module.exports = config;
```

## 加載 CSS 檔案
Loading a CSS file is a simple as loading any file:

加載一個 CSS 檔案就和加載其他檔案一樣簡單：

**main.js**

```javascript
import './main.css';
// Other code
```

**Component.jsx**

```javascript
import './Component.css';
import React from 'react';

export default React.createClass({
  render: function () {
    return <h1>Hello world!</h1>
  }
});
```

**Note!** You can of course do this with both CommonJS and AMD.

**注意！** 你也可以在 CommonJS 和 AMD 中做同樣的事情。

## CSS 加載策略

Depending on your application you might consider three main strategies. In addition to this you should consider including some of your basic CSS inlined with the initial payload (index.html). This will set the structure and maybe a loader while the rest of your application is downloading and executing.

根據你的應用，你可能會考略三種策略。另外，你需要考慮把一些基礎的 CSS 內聯到初始容器中（index.html），這樣設置的結構能夠在應用下載和執行的時候加載剩下的應用。

### 所有合併成一個

In your main entry point, e.g. `app/main.js` you can load up your entire CSS for the whole project:

在你的主入口檔案中個，比如 `app/main.js` 你可以為整個項目加載所有的 CSS：

**app/main.js**

```javascript
import './project-styles.css';
// 其他 JS 代碼
```

The CSS is included in the application bundle and does not need to download.

CSS 就完全包含在合併的應用中，再也不需要重新下載。

### 預先加載 Lazy loading

If you take advantage of lazy loading by having multiple entry points to your application, you can include specific CSS for each of those entry points:

如果你想發揮應用中多重入口檔案的優勢，你可以在每個入口點包含各自的 CSS：

**app/main.js**

```javascript
import './style.css';
// 其他 JS 代碼
```

**app/entryA/main.js**

```javascript
import './style.css';
// 其他 JS 代碼
```

**app/entryB/main.js**

```javascript
import './style.css';
// 其他 JS 代碼
```

You divide your modules by folders and include both CSS and JavaScript files in those folders. Again, the imported CSS is included in each entry bundle when running in production.

你把你的模組用檔案夾分離，每個檔案夾有各自的 CSS 和 JavaScript 檔案。再次，當應用發佈的時候，導入的 CSS 已經加載到每個入口檔案中。

### 具體的組件

With this strategy you create a CSS file for each component. It is common to namespace the CSS classes with the component name, thus avoiding some class of one component interfering with the class of an other.

你可以根據這個策略為每個組件創建 CSS 檔案，可以讓組件名和 CSS 中的 class 使用一個命名空間，來避免一個組件中的一些 class 干擾到另外一些組件的 class。

**app/components/MyComponent.css**

```css
.MyComponent-wrapper {
  background-color: #EEE;
}
```

**app/components/MyComponent.jsx**

```
import './MyComponent.css';
import React from 'react';

export default React.createClass({
  render: function () {
    return (
      <div className="MyComponent-wrapper">
        <h1>Hello world</h1>
      </div>
    )
  }
});
```

## 使用內聯樣式取代 CSS 檔案

With "React Native" you do not use stylesheets at all, you only use the *style-attribute*. By defining your CSS as objects. Depending on your project, you might consider this as your CSS strategy.

在 “React Native” 中你不再需要使用任何 CSS 檔案，你只需要使用 *style 屬性*，可以把你的 CSS 定義成一個對象，那樣就可以根據你的項目重新來考略你的 CSS 策略。

**app/components/MyComponent.jsx**

```javascript
import React from 'react';

var style = {
  backgroundColor: '#EEE'
};

export default React.createClass({
  render: function () {
    return (
      <div style={style}>
        <h1>Hello world</h1>
      </div>
    )
  }
});
```
