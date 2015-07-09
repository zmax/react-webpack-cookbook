如果需要一直输入 `npm run build` 确实是一件非常无聊的事情，幸运的是，我们可以把让他安静的运行，让我们设置 `webpack-dev-server`

## 设置 `webpack-dev-server`

第一步，输入 `npm i webpack-dev-server --save`，此外，我们需要去调整 `package.json` *scripts* 部分去包含这个指令，下面是基本的设置：

*package.json*
```json
{
  "scripts": {
    "build": "webpack",
    "dev": "webpack-dev-server --devtool eval --progress --colors --hot --content-base build"
  }
}
```

当你在命令行里运行 `npm run dev` 的时候他会执行 **dev** 属性里的值。
这是这些指令的意思：

1. `webpack-dev-server` - 在 localhost:8080 建立一个 Web 服务器
2. `--devtool eval` - 为你的代码创建源地址。当有任何报错的时候可以让你更加精确地定位到文件和行号
3. `--progress` - 显示合并代码进度
4. `--colors` - Yay，命令行中显示颜色！
5. `--content-base build` - 指向设置的输出目录


总的来说，当你运行 `npm run dev` 的时候，会启动一个 Web 服务器，然后监听文件修改，然后自动重新合并你的代码。真的非常简洁！

访问 **http://localhost:8080** 你会看到效果。