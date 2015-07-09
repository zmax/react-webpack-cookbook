在 Web 开发历程上，我们构建了很多小技术解决方案。比如用 HTML 去描述页面结构，CSS 去描述页面样式，Javascript 去描述页面逻辑。或者你也可以用一些比如 Jade 去取代 HTML，用 Sass 或 Less 去取代CSS，用 CoffeeScript 或者 TypeScript 之类的去取代 Javascript，但是你需要解决项目中的依赖。（需要安装额外很多的库）

这里个很多为什么我们需要使用那些新技术的理由。不管我们用什么，总之，我们还是希望使用那些能够允许在客户端上浏览器的方案。所以出来了 build 系统。历史上已经有很多分享了，比如 [Make](https://en.wikipedia.org/wiki/Make_%28software%29) 可能是很多解决方案中最知名且是可行的方案。[Grunt](http://gruntjs.com/) 和 [Gulp](http://gulpjs.com/) 是在是前端的世界中最流行的解决方案。他们两个都有很多非常有用的插件。[NPM](https://www.npmjs.com/)（Node.js 的包管理器）则包含了他们两个。

## Grunt

Grunt 是相比后面几个最早的项目，他依赖于各种插件的配置。这是一个很好的解决方案，但是请相信我，你不会想看到一个 300 行的 `Gruntfile`。如果你好奇 Grunt 的配置会如何，那么这里是有个从 [Grunt 文档](http://gruntjs.com/sample-gruntfile) 的例子：

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

Gulp 提供了一个不一样的解决方案，而不是依赖于各种配置的插件。Gulp 使用了一个流的概念。如果你熟悉 Unix，那么 Gulp 对你来说会差不多，Gulp会提供你一些简单化的操作。在这个解决方案中，是去匹配一些文件然后操作（就是说和 JavaScript 相反） 然后输出结果（你的设置的 build 路径等）。这里有一个简单的 `Gulpfile` 来展示一个比项目 README 更好的方式。


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

// 不是所有的任务需要使用 streams
// 一个 gulpfile 只是另一个node的程序，所以你可以使用所有 npm 的包
gulp.task('clean', function(cb) {
  // 你可以用 `gulp.src` 来使用多重通配符模式
  del(['build'], cb);
});

gulp.task('scripts', ['clean'], function() {
  // 压缩和复制所有 JavaScript （除了第三方库）
  // 加上 sourcemaps
  return gulp.src(paths.scripts)
    .pipe(sourcemaps.init())
      .pipe(coffee())
      .pipe(uglify())
      .pipe(concat('all.min.js'))
    .pipe(sourcemaps.write())
    .pipe(gulp.dest('build/js'));
});

// 监听文件修改
gulp.task('watch', function() {
  gulp.watch(paths.scripts, ['scripts']);
});

// 默认任务（就是你在命令行输入 `gulp` 时运行）
gulp.task('default', ['watch', 'scripts']);
```

哪怕你陷入一些问题中，那你也可以修改这些配置代码，你也可以使用已经存在的 Gulp 插件，但是你还是需要写一堆模板任务。

## Browserify

处理 Javascript 模块一直是一个大问题，因为这个语言在 ES6 之前没有这方面的概念。因此我们还是停留在90年代，各种解决方案，比如提出了 [AMD](http://browserify.org/)。在实践中，它在使用 CommonJs 中会很有用，会节约很多时间。它的优势是你可以发布到 NPM 上来避免重新发明轮子。

[Browserify](http://browserify.org/) 解决了这个问题，它提供了一种可以把模块集合到一起的方式。你可以用 Gulp 调用它，此外有很多小转换工具可以让你更兼容的使用（比如 [watchify](https://www.npmjs.com/package/watchify ）提供了一个文件监视器帮助你在开发过程中更加自动化地把文件合并起来），这样会省下很多精力。毋庸置疑，一定程度来讲，这是一个很好的解决方案。


## Webpack

Webpack 扩展了 CommonJs 的 `require` 的想法，比如你想在 CoffeeScript、Sass、Markdownh或者其他什么代码中 `require` 你想要的任何代码？那么 Webpack 正是做这方面的工作。它会通过配置来取出代码中的依赖，然后把他们通过 loaders 把代码兼容地输出到静态资源中。这里是一个 [Webpack 官网](http://webpack.github.io/docs/tutorials/getting-started/) 上的例子： 

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

在接下来的章节中我们会使用 Webpack 来构建项目来展示它的一个能力。你可以用其他工具和 Webpack 一起使用。它不会解决所有事情，只是解决一个打包的难题，无论如何，这是在开发过程中需要注意的问题。