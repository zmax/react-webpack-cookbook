In web development we deal with a lot of small technical artifacts. You use HTML to describe page structure, CSS how to style it and JavaScript for logic. Or you can replace HTML with something like Jade, CSS with Sass or LESS, JavaScript with CoffeeScript, TypeScript and the ilk. In addition you have to deal with project dependencies (ie. external libraries and such).

在 Web 開發歷程上，我們構建了很多小型的技術解決方案，比如用 HTML 去描述頁面結構，CSS 去描述頁面樣式，JavaScript 去描述頁面邏輯，或者你也可以用一些比如 Jade 去取代 HTML，用 Sass 或 Less 去取代CSS，用 CoffeeScript 或者 TypeScript 之類的去取代 JavaScript，不過項目中的依賴可能是一件比較煩惱的事情。（需要安裝額外很多的庫）

There are good reasons why we use these various technologies. Regardless of what we use, however, we still want to end up with something that can be run on the browsers of the clients. This is where build systems come in. Historically speaking there have been many. [Make](https://en.wikipedia.org/wiki/Make_%28software%29) is perhaps the most known one and still a viable option in many cases. In the world of frontend development particularly [Grunt](http://gruntjs.com/) and [Gulp](http://gulpjs.com/) have gained popularity. Both are made powerful by plugins. [NPM](https://www.npmjs.com/), the Node.js package manager, is full of those.

這裡有很多為什麼我們需要嘗試那些新技術的理由。不管我們用什麼，總之，我們還是希望使用那些能夠處理在瀏覽器端的方案，所以出來了編譯方案。歷史上已經有很多分享了，比如 [Make](https://en.wikipedia.org/wiki/Make_%28software%29) 可能是很多解決方案中最知名且是可行的方案。[Grunt](http://gruntjs.com/) 和 [Gulp](http://gulpjs.com/) 是在是前端的世界中最流行的解決方案，他們兩個都有很多非常有用的插件。[NPM](https://www.npmjs.com/)（Node.js 的包管理器）則包含了他們兩個。

## Grunt

Grunt is the older project. It relies on plugin specific configuration. This is fine up to a point but believe me, you don't want to end up having to maintain a 300 line `Gruntfile`. The approach simply turns against itself at some point. Just in case you are curious what the configuration looks like, here's an example from [Grunt documentation](http://gruntjs.com/sample-gruntfile):

Grunt 是相比後面幾個更早的項目，他依賴于各種插件的配置。這是一個很好的解決方案，但是請相信我，你不會想看到一個 300 行的 `Gruntfile`。如果你好奇 Grunt 的配置會如何，那麼這裡是有個從 [Grunt 文檔](http://gruntjs.com/sample-gruntfile) 的例子：

```javascript
module.exports = function(grunt) {

  grunt.initConfig({
    jshint: {
      files: ['Gruntfile.js', 'src/**/*.js', 'test/**/*.js'],
      options: {
        globals: {
          jQuery: true
        }
      }
    },
    watch: {
      files: ['<%= jshint.files %>'],
      tasks: ['jshint']
    }
  });

  grunt.loadNpmTasks('grunt-contrib-jshint');
  grunt.loadNpmTasks('grunt-contrib-watch');

  grunt.registerTask('default', ['jshint']);

};
```

## Gulp

Gulp takes a different approach. Instead of relying on configuration per plugin you deal with actual code. Gulp builds on top of the tried and true concept of piping. If you are familiar with Unix, it's the same here. You simply have sources, filters and sinks. In this case sources happen to match to some files, filters perform some operations on those (ie. convert to JavaScript) and then output to sinks (your build directory etc.). Here's a sample `Gulpfile` to give you a better idea of the approach taken from the project README and abbreviated a bit:

Gulp 提供了一個不一樣的解決方案，而不是依賴于各種插件的配置。Gulp 使用了一個檔案流的概念。如果你熟悉 Unix，那麼 Gulp 對你來說會差不多，Gulp 會提供你一些簡單化的操作。在這個解決方案中，是去匹配一些檔案然後操作（就是說和 JavaScript 相反）然後輸出結果（比如輸出在你設置的編譯路徑等）。這裡有一個簡單的 `Gulpfile` 的例子：


```javascript
var gulp = require('gulp');
var coffee = require('gulp-coffee');
var concat = require('gulp-concat');
var uglify = require('gulp-uglify');
var sourcemaps = require('gulp-sourcemaps');
var del = require('del');

var paths = {
  scripts: ['client/js/**/*.coffee', '!client/external/**/*.coffee'],
};

// 不是所有的任務需要使用 streams
// 一個 gulpfile 只是另一個node的程序，所以你可以使用所有 npm 的包
gulp.task('clean', function(cb) {
  // 你可以用 `gulp.src` 來使用多重通配符模式
  del(['build'], cb);
});

gulp.task('scripts', ['clean'], function() {
  // 壓縮和複製所有 JavaScript （除了第三方庫）
  // 加上 sourcemaps
  return gulp.src(paths.scripts)
    .pipe(sourcemaps.init())
      .pipe(coffee())
      .pipe(uglify())
      .pipe(concat('all.min.js'))
    .pipe(sourcemaps.write())
    .pipe(gulp.dest('build/js'));
});

// 監聽檔案修改
gulp.task('watch', function() {
  gulp.watch(paths.scripts, ['scripts']);
});

// 預設任務（就是你在命令行輸入 `gulp` 時運行）
gulp.task('default', ['watch', 'scripts']);
```

Given the configuration is code you can always just hack it if you run into troubles. You can wrap existing Node.js modules as Gulp plugins and so on. You still end up writing a lot of boilerplate for casual tasks, though.

這些配置都是代碼，所以當你遇到問題也可以修改，你也可以使用已經存在的 Gulp 插件，但是你還是需要寫一堆模板任務。

## Browserify

Dealing with JavaScript modules has always been a bit of a problem given the language actually doesn't have a concept of module till ES6. Ergo we are stuck with the 90s when it comes to browser environment. Various solutions, including [AMD](http://browserify.org/), have been proposed. In practice it can be useful just to use CommonJS, the Node.js format, and let tooling deal with the rest. The advantage is that you can often hook into NPM and avoid reinventing the wheel.

處理 JavaScript 模組一直是一個大問題，因為這個語言在 ES6 之前沒有這方面的概念。因此我們還是停留在90年代，各種解決方案，比如提出了 [AMD](http://browserify.org/)。在實踐中只使用 CommonJS （ Node.js 所採用的格式）會比較有幫助，而讓工具去處理剩下的事情。它的優勢是你可以發佈到 NPM 上來避免重新發明輪子。

[Browserify](http://browserify.org/) solves this problem. It provides a way to bundle CommonJS modules together. You can hook it up with Gulp. In addition there are tons of smaller transformation tools that allow you to move beyond the basic usage (ie. [watchify](https://www.npmjs.com/package/watchify) provides a file watcher that creates bundles for you during development automatically). This will save some effort and no doubt is a good solution up to a point.

[Browserify](http://browserify.org/) 解決了這個問題，它提供了一種可以把模組集合到一起的方式。你可以用 Gulp 調用它，此外有很多轉換小工具可以讓你更兼容的使用（比如 [watchify](https://www.npmjs.com/package/watchify) 提供了一個檔案監視器幫助你在開發過程中更加自動化地把檔案合併起來），這樣會省下很多精力。毋庸置疑，一定程度來講，這是一個很好的解決方案。


## Webpack

Webpack expands on the idea of hooking into CommonJS `require`. What if you could just `require` whatever you needed in your code, be it CoffeeScript, Sass, Markdown or something? Well, Webpack does just this. It takes your dependencies, puts them through loaders and outputs browser compatible static assets. All of this is based on configuration. Here is a sample configuration from [the official Webpack tutorial](http://webpack.github.io/docs/tutorials/getting-started/):

Webpack 擴展了 CommonJs 的 `require` 的想法，比如你想在 CoffeeScript、Sass、Markdown 或者其他什麼代碼中 `require` 你想要的任何代碼的話？那麼 Webpack 正是做這方面的工作。它會通過配置來取出代碼中的依賴，然後把他們通過加載器把代碼兼容地輸出到靜態資源中。這裡是一個 [Webpack 官網](http://webpack.github.io/docs/tutorials/getting-started/) 上的例子： 

```javascript
module.exports = {
    entry: "./entry.js",
    output: {
        path: __dirname,
        filename: "bundle.js"
    },
    module: {
        loaders: [
            { test: /\.css$/, loader: "style!css" }
        ]
    }
};
```

In the following sections we'll build on top of this idea and show how powerful it is. You can, and probably should, use Webpack with some other tools. It won't solve everything. It does solve the difficult problem of bundling, however, and that's one worry less during development.

在接下來的章節中我們會使用 Webpack 來構建項目來展示它的能力。你可以用其他工具和 Webpack 一起使用。它不會解決所有事情，只是解決一個打包的難題，無論如何，這是在開發過程中需要解決的問題。