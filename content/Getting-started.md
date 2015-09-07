> Before getting started you should make sure you have a recent version of Node.js and NPM installed. See [nodejs.org](http://nodejs.org/) for installation details. We'll use NPM to set up various tools.

> 在開始之前，你需要把你的 Node.js 和 NPM 都更新到最新的版本。訪問 [nodejs.org](http://nodejs.org/) 查看安裝詳情。我們將會使用 NPM 安裝一些工具。

Getting started with Webpack is straightforward. I'll show you how to set up a simple project based on it. As a first step, set a directory for your project and hit `npm init` and fill in some answers. That will create a `package.json` for you. Don't worry if some fields don't look ok, you can modify those later.

開始使用 Webpack 非常簡單，我會展示給你看使用它的一個簡單的項目。第一步，為你的項目新建一個檔案夾，然後輸入 `npm init`，然後填寫相關問題。這樣會為你創建了 `package.json`，不用擔心填錯，你可以之後修改它。

## 安裝 Webpack

Next you should get Webpack installed. We'll do a local install and save it as a project dependency. This way you can invoke the build anywhere (build server, whatnot). Run `npm i webpack --save-dev`. If you want to run the tool, hit `node_modules/.bin/webpack`.

接下來我們安裝 Webpack，我們要把它安裝在本地，然後把它作為項目依賴保存下來。這樣你可以在任何地方編譯（服務端編譯之類的）。輸入 `npm i webpack --save-dev`。如果你想運行它，就輸入 `node_modules/.bin/webpack`。

## 目錄結構

Structure your project like this:

項目的目錄結構長這樣：

- /app
  - main.js
  - component.js
- /build
  - bundle.js (自動創建)
  - index.html
- package.json
- webpack.config.js

In this case we'll create `bundle.js` using Webpack based on our `/app`. To make this possible, let's set up `webpack.config.js`.

我們會使用 Webpack 在我們的 `/app` 裡來自動創建 `bundle.js` 。接下來，我們來設置 `webpack.config.js`。

## 設置 Webpack

Webpack 的配置檔案長這樣：

*webpack.config.js*

```javascript
var path = require('path');


module.exports = {
    entry: path.resolve(__dirname, 'app/main.js'),
    output: {
        path: path.resolve(__dirname, 'build'),
        filename: 'bundle.js',
    },
};
```

## 運行你的第一個編譯

Now that we have basic configuration in place, we'll need something to build. Let's start with a classic `Hello World` type of app. Set up `/app` like this:

現在我們有了一個最簡單的配置，我們需要有什麼東西去編譯，讓我們開始一個經典的 `Hello World`，設置 `/app` 像這樣：

*app/component.js*

```javascript
'use strict';


module.exports = function () {
    var element = document.createElement('h1');

    element.innerHTML = 'Hello world';

    return element;
};
```

*app/main.js*

```javascript
'use strict';
var component = require('./component.js');


document.body.appendChild(component());

```

Now run `webpack` in your terminal and your application will be built. A *bundle.js* file will appear in your `/build` folder. Your *index.html* file in the `build/` folder will need to load up the application.

現在在你的命令行運行 `webpack`，然後你的應用會開始編譯，一個 `bundle.js` 檔案就這樣出現在你的 `/build` 檔案夾下，需要在 `build/` 下的 *index.html* 去啟動項目。

*build/index.html*

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8"/>
  </head>
  <body>
    <script src="bundle.js"></script>
  </body>
</html>
```

> It would be possible to generate this file with Webpack using [html-webpack-plugin](https://www.npmjs.com/package/html-webpack-plugin). You can give it a go if you are feeling adventurous. It is mostly a matter of configuration. Generally this is the way you work with Webpack.

> 這個檔案可以用 [html-webpack-plugin](https://www.npmjs.com/package/html-webpack-plugin) 來生成。如果你覺得冒險，那就把剩下的工具交給它來做。使用它就只有一個配置的問題。一般來說使用 Webpack 來工作就是這麼個套路。

## 運行應用

Just double-click the *index.html* file or set up a web server pointing to the `build/` folder.

只要雙擊 *index.html* 或者設置一個 Web 服務指向 `build/` 檔案夾。

## 設置 `package.json` *scripts*

It can be useful to run build, serve and such commands through `npm`. That way you don't have to worry about the technology used in the project. You just invoke the commands. This can be achieved easily by setting up a `scripts` section to `package.json`.

`npm` 是一個非常好用的用來編譯的指令，通過 `npm` 你可以不用去擔心項目中使用了什麼技術，你只要調用這個指令就可以了，只要你在 `package.json` 中設置 `scripts` 的值就可以了。

In this case we can move the build step behind `npm run build` like this:

在這個案例中我們把編譯步驟放到 `npm run build` 中是這樣：

1. `npm i webpack --save` - If you want to install Webpack just a development dependency, you can use `--save-dev`. This is handy if you are developing a library and don't want it to depend on the tool (bad idea!).
2. Add the following to `package.json`:


1. `npm i webpack --save` - 如果你想要把 Webpack 作為一個項目的開發依賴，就可以使用 `--save-dev`，這樣就非常方便地讓你在開發一個庫的時候，不會依賴工具（但不是個好方法！）。
2. 把下面的內容添加到 `package.json`中。

```json
  "scripts": {
    "build": "webpack"
  }
```

To invoke a build, you can hit `npm run build` now.

現在你可以輸入 `npm run build` 就可以編譯了。

Later on this approach will become more powerful as project complexity grows. You can hide the complexity within `scripts` while keeping the interface simple.

當項目越發複雜的時候，這樣的方法會變得越來越有效。你可以把所有複雜的操作隱藏在 `scripts` 裡面來保證界面的簡潔。

The potential problem with this approach is that it can tie you to a Unix environment in case you use environment specific commands. If so, you may want to consider using something environment agnostic, such as [gulp-webpack](https://www.npmjs.com/package/gulp-webpack).

不過潛在的問題是這種方法會導致如果你使用一些特殊的指令的時候只能在 Unix 環境中使用。所以如果你需要考慮一些未知的環境中的話，那麼 [gulp-webpack](https://www.npmjs.com/package/gulp-webpack) 會是一個好的解決方案。

> Note that NPM will find Webpack. `npm run` adds it to the `PATH` temporarily so our simple incantation will work.

> 注意 NPM 會找到 Webpack，`npm run` 會把他臨時加到 `PATH`來讓我們這個神奇的命令工作。
