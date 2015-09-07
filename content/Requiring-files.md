## 模組

Webpack allows you to use different module patterns, but "under the hood" they all work the same way. All of them also works straight out of the box.

Webpack 允許你使用不同的模組類型，但是 “底層”必須使用同一種實現。所有的模組都能夠開箱即用。

#### ES6 模組

```javascript
import MyModule from './MyModule.js';
```

#### CommonJS

```javascript
var MyModule = require('./MyModule.js');
```

#### AMD

```javascript
define(['./MyModule.js'], function (MyModule) {

});
```

## 理解檔案路徑

A module is loaded by filepath. Imagine the following tree structure:

一個模組會按它的檔案路徑來加載，看一下下面的這個結構：

- /app
  - /modules
    - MyModule.js
  - main.js (entry point)
  - utils.js

Lets open up the *main.js* file and require *app/modules/MyModule.js* in the two most common module patterns:

打開 *main.js* 然後可以通過下面兩種方式引入 *app/modules/MyModule.js* 

*app/main.js*
```javascript
// ES6
import MyModule from './modules/MyModule.js';

// CommonJS
var MyModule = require('./modules/MyModule.js');
```

The `./` at the beginning states "relative to the file I am in now".

最開始的 `./` 是 “相對當前檔案路徑” 

Now let us open the *MyModule.js* file and require **app/utils**.

讓我們打開 *MyModule.js* 然後引入 **app/utils**：

*app/modules/MyModule.js*
```javascript
// ES6 相對路徑
import utils from './../utils.js';

// ES6 絕對路徑
import utils from '/utils.js';

// CommonJS 相對路徑
var utils = require('./../utils.js');

// CommonJS 絕對路徑
var utils = require('/utils.js');
```

The **relative path** is relative to the current file. The **absolute path** is relative to the entry file, which in this case is *main.js*.

**相對路徑**是相對當前目錄。**絕對路徑**是相對入口檔案，這個案例中是 *main.js*。

### 我需要使用檔案尾碼麼？

No, you do not have to use *.js*, but it highlights better what you are requiring. You might have some .js files, and some .jsx files and even images and css can be required by Webpack. It also clearly differs from required node_modules and specific files.
   
不，你不需要去特意去使用 *.js*，但是他能夠更讓你更清楚你正引入的檔案。因為你可能有一些 .js 檔案和一些 .jsx 檔案，甚至一些圖片和 css 可以用 Webpack 來引入。加入檔案尾碼，可以讓你清楚地區分你引入的是 node_modules 或特定檔案還是一般檔案檔案。

Remember that Webpack is a module bundler! This means you can set it up to load any format you want given there is a loader for it. We'll delve into this topic later on.

記住，Webpack 只是一個模組合併器！也就是說你可以設置他去加載任何你寫的匹配，只要有一個加載器。我們稍後會繼續深入這個話題。
