Webpack can be handy for packaging your library for general consumption. You can use it to output UMD, a format that's compatible with various module loaders (CommonJS, AMD) and globals.

Webpack 可以非常方便地打包和生成你的庫，你可以用它輸出 UMD，一種可以兼容很多模組加載器（CommonJS、AMD）和全局變數的格式。

## 如何把庫輸出成 UMD?

Especially if you are creating a library, it can be useful to output an UMD version of your library. This can be achieved using the following snippet:

尤其是如果你創建了一個庫，那麼輸出一個 UMD 版本是非常有用的，就像下面的片段一樣實現：

```javascript
output: {
    path: './dist',
    filename: 'mylibrary.js',
    libraryTarget: 'umd',
    library: 'MyLibrary',
},
```

In order to avoid bundling big dependencies like React, you'll want to use a configuration like this in addition:

為了避免去合併類似 React 的大型依賴，你可以使用下面這樣的設置：

```javascript
externals: {
    react: 'react',
    'react/addons': 'react'
},
```

## 如何輸出壓縮版?

Here's the basic idea:

這裡是一個簡單的方案：

```javascript
output: {
    path: './dist',
    filename: 'awesomemular.min.js',
    libraryTarget: 'umd',
    library: 'Awesomemular',
},
plugins: [
    new webpack.optimize.UglifyJsPlugin({
        compress: {
            warnings: false
        },
    }),
]
```