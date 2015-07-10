You might notice after requiring React JS into your project that the time it takes from a save to a finished rebundle of your application takes more time. In development you ideally want from 200-800 ms rebundle speed, depending on what part of the application you are working on.

你可能注意到在引入 React JS 到你的项目之后，给你的应用重新合并会花费太多的时间。在开发环境中，最理想的是编译最多 200 到 800 毫秒的速度，取决于你在开发的应用。

## 在开发环境中使用压缩文件 

Instead of making Webpack go through React JS and all its dependencies, you can override the behavior in development.

为了不让 Webpack 去遍历 React JS 及其依赖，你可以在开发中重写它的行为。

*webpack.config.js*
```javascript
var path = require('path');
var node_modules = path.resolve(__dirname, 'node_modules');
var pathToReact = path.resolve(node_modules, 'react/dist/react.min.js');

config = {
    entry: ['webpack/hot/dev-server', path.resolve(__dirname, 'app/main.js')],
    resolve: {
	    alias: {
	      'react': pathToReact
	    }
	},
    output: {
        path: path.resolve(__dirname, 'build'),
        filename: 'bundle.js',
    },
    module: {
    	loaders: [{
    		test: /\.jsx?$/,
    		loader: 'babel'
    	}],
    	noParse: [pathToReact]
    }    
};

module.exports = config;
```

We do two things in this configuration:

我们在配置中做了两件事：

1. Whenever "react" is required in the code it will fetch the minified React JS file instead of going to *node_modules*

2. Whenever Webpack tries to parse the minified file, we stop it, as it is not necessary


1. 不管 “React” 是什么时候在代码中引入的，它会去匹配压缩后的 React JS 文件取代去 *node_modules* 中遍历。

2. 不管 Webpack 什么时候试图是解析压缩文件，我们阻止它，告诉它那不是必须的。

Take a look at [Optimizing development](Optimizing-development) for more information on this.

可以到 [优化开发](Optimizing-development) 看到更多这方面的信息。