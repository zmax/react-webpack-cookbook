# React Webpack Cookbook

> [繁體中文版](https://zmax.github.io/react-webpack-cookbook/)
> [簡體中文版](https://fakefish.github.io/react-webpack-cookbook/)

## 貢獻

若書中有遺漏或者錯誤的地方，可以按照下面的方式：

1. Fork 這個倉庫
2. 切換到 zh-tw 分支
3. 在 `/content` 裡做修改
4. 提交一個 PR

或者可以到原 Repo 中 [提 issue](https://github.com/christianalfoni/react-webpack-cookbook/issues/new)。

注意 `gh-pages` 內容是自動生成的。

## Gitbook 生成器

生成器會自動生成 wiki 和網站，為了能夠讓它提交 `gh-pages` 分支，這樣使用它：

1. `npm install`
2. `npm run generate-gitbook`

它會自動生成 `/gh-pages`，你可以直接通過文件系統來訪問。

如果要發佈內容，那麼輸入 `npm run deploy-gitbook`，它會自動更新 `gh-pages` 分支。
