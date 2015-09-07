So the great thing about React JS is that it runs on the server too. But that does not mean you can just create any app and run it on the server. You have to make some decisions on the architecture. The reason is that even though React JS and the components run on the server, you might be having dependencies in those components that does not run on the server.

React Js 最偉大的地方是它也可以運行在服務端，不過這不意味著你可以創建任何一個應用然後運行在服務端，你需要做一些決策和架構。原因是哪怕 React JS 和一些組件可以在服務端運行，但還是有一些組件中的依賴不能在服務端運行。

## 注入狀態
One of the most important decisions you make is to inject the state of your application through the top component. This basically means that your components does not have any external dependencies at all. All they need to know comes through this injected state.

一個重要的事情是應用需要通過頂層組件把狀態注入，這意味著你的組件沒有了任何的外部依賴，他們只能通過注入的狀態來獲取信息。

This cookbook is not about isomorphic apps, but let us take a look at an example. We will not use ES6 syntax here because Node JS does not support it yet.

這本小書不是主要講同構渲染的應用，不過讓我們來看一下例子，我們這次不使用 ES6 語法了，因為 Node JS 還不完全支持。

*main.js (client)*
```javascript
var React = require('react');
var AppState = require('./client/AppState.js');
var App = require('./App.js');

React.render(<App state={AppState}/>, document.body);
```

*router.js (server)*
```javascript
var React = require('react');
var App = require('./App.js');
var AppState = require('./server/AppState.js');
var index = '<!DOCTYPE html><html><head></head><body>{{component}}</body></html>';

app.get('/', function (req, res) {
  var componentHtml = React.renderToString(App({state: AppState}));
  var html = index.replace('{{component}}', componentHtml);
  res.type('html');
  res.send(html);
});
```

So this was a very naive and simple way of showing it, but what you should notice here is that we use the same **App.js** file on the client and server, but we have two different ways of producing the state.

所以這是一個非常初級且簡單的例子來展示它，不過你需要注意的是我們在客戶端和服務端使用了同一個 **App.js**，但是我們需要兩種方式來提供狀態。