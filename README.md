# React 和 Webpack 小书 - 一本介绍在 React JS 中使用 Webpack 的小书。

> [中文版](https://fakefish.github.io/react-webpack-cookbook/)

## 贡献

若书中有遗漏或者错误的地方，可以按照下面的方式：

1. Fork 这个仓库
2. 切换到 zh-cn 分支
3. 在 `/content` 里做修改
4. 提交一个 PR

或者可以到原仓库中 [提 issue](https://github.com/christianalfoni/react-webpack-cookbook/issues/new)。

注意 `gh-pages` 内容是自动生成的。

## Gitbook 生成器

生成器会自动生成 wiki 和网站，为了能够让它提交 `gh-pages` 分支，这样使用它：

1. `npm install`
2. `npm run generate-gitbook`

它会自动生成 `/gh-pages`，你可以直接通过文件系统来访问。

如果要发布内容，那么输入 `npm run deploy-gitbook`，它会自动更新 `gh-pages` 分支。
