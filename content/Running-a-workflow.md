Hitting `npm run build` all the time will get boring eventually. Fortunately we can work around that quite easily. Let's set up `webpack-dev-server`.

如果需要一直輸入 `npm run build` 確實是一件非常無聊的事情，幸運的是，我們可以把讓他安靜的運行，讓我們設置 `webpack-dev-server`。

## 設置 `webpack-dev-server`

As a first step, hit `npm i webpack-dev-server --save`. In addition we'll need to tweak `package.json` *scripts* section to include it. Here's the basic idea:

第一步，輸入 `npm i webpack-dev-server --save`，此外，我們需要去調整 `package.json` *scripts* 部分去包含這個指令，下面是基本的設置：

*package.json*
```json
{
  "scripts": {
    "build": "webpack",
    "dev": "webpack-dev-server --devtool eval --progress --colors --hot --content-base build"
  }
}
```

When you run `npm run dev` from your terminal it will execute the command stated as a value on the **dev** property. This is what it does:

當你在命令行裡運行 `npm run dev` 的時候他會執行 **dev** 屬性裡的值。這是這些指令的意思：

1. `webpack-dev-server` - Starts a web service on localhost:8080
2. `--devtool eval` - Creates source urls for your code. Making you able to pinpoint by filename and line number where any errors are thrown
3. `--progress` - Will show progress of bundling your application
4. `--colors` - Yay, colors in the terminal!
5. `--content-base build` - Points to the output directory configured


1. `webpack-dev-server` - 在 localhost:8080 建立一個 Web 伺服器
2. `--devtool eval` - 為你的代碼創建源地址。當有任何報錯的時候可以讓你更加精確地定位到檔案和行號
3. `--progress` - 顯示合併代碼進度
4. `--colors` - Yay，命令行中顯示顏色！
5. `--content-base build` - 指向設置的輸出目錄

To recap, when you run `npm run dev` this will fire up the webservice, watch for file changes and automatically rebundle your application when any file changes occur. How neat is that!

總的來說，當你運行 `npm run dev` 的時候，會啟動一個 Web 伺服器，然後監聽檔案修改，然後自動重新合併你的代碼。真的非常簡潔！

Go to **http://localhost:8080** and you should see something.

訪問 **http://localhost:8080** 你會看到效果。