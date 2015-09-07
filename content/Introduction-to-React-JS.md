I remember when I saw React the first time around the time it was announced I was skeptical. Particularly mixing some sort of HTML within your code seemed against good conventions. It just felt like a "bad idea"&reg;.

我記得當我第一次看到 React 的時候是十分懷疑的，特別是把 HTML 揉進代碼裡違背了傳統的那些約定，看起來像是一個 “壞注意”。

But that's what React and similar approaches are doing. They challenge some of the conventions and replace them with something more palatable. Sometimes a bigger change in thinking is needed for you to move forward as a developer. That's what React did for me. It takes some powerful ideas from the world of functional programming and then builds on top of those.

但是這就是 React，他們挑戰了一些約定，然後用這種方式代替了。作為一個開發者，有時候為了能夠改變思考方式是你向前走的一個必經之路。這就是 React 對我的影響，它建立在函數式編程上帶給了我很多有意思的想法。

## 基礎功能

Before you can understand React and how it changes web development, there are a few things you should know about it. React itself won't be enough. It solves only the problem of views. You still need to complement it with something else. But that's a good thing.

在你能理解 React 是如何改變 Web 開發之前，這裡有一點東西你需要知道的。React 它本身是不夠豐富的，它只能解決一些視圖上的問題，你仍然需要一些東西去幫助它完成事情。

The greatest and worst feature of frameworks is that they sort of cage you in. As long as you are doing what they expect you to do within their boundaries, everything is fine. It is only after you start to reach beyond those boundaries that problems begin to appear. In a library driven approach you aren't as bound. Initially you might not be as fast or efficient but over time as problems become harder, you will have more choices available.

最偉大的和最差勁的框架總是企圖給你建立一個籠子，當你在他們劃定的地盤裡能夠完成任何需求，一切都是很好的，但是，當你需要跨出邊界的時候，開始有問題了，在一個庫驅動方法裡你是不被限制的，最初你可能是高效地完成需求，但是伴隨時間的推進，問題開始變得嚴峻起來，你需要更多可行的選擇。

### JSX 的基礎

React provides a component centric approach to frontend development. You will design your application as smaller components, each of which has it own purpose. Taken to the extreme a component may contain its logic, layout and basic styling. To give you an example of JSX:

React 為前端開發提供了一個組件為中心的方法，你可以為你的應用設計一個更小的組件，所有組件有各自的目的，甚至極端來說，一個組件可能包含它自身的邏輯，結構和基本的樣式。這裡有個 JSX 的例子：
```html
...
<TodoItem className='urgent' owner={owner} task='Make a dinner' />
...
```

You can see a couple of basic features of JSX here. Instead of using `class`, we'll use the JavaScript equivalent. In addition we have defined a couple of custom properties in form of `owner` and `task`. `owner` is something that is injected from a variable named `owner` that's within the same scope as our JSX. For `task` we provide a fixed value.

你可以看到一個基本功能的 JSX，我們用 `className` 替代了使用 `class`。此外，我們定義了一些自定義屬性 `owner` 和 `task`，`owner` 的值是從 JSX 同環境中一個變數叫 `owner` 注入，而我們為 `task` 提供了一個固定的值。

In practice you would most likely structure this a little differently to fit your data model better. That goes a little beyond basic React, though.

在實踐中，你可以根據你的數據模型結構來稍微調整目錄結構會更好，雖然是 React 進階的知識點了。

We can mix normal JavaScript code within those \{\}'s. We can use this idea to render a list of `TodoItem`s like this (ES syntax):

我們可以用那些 `{}` 來混合普通 JavaScript 代碼，我們可以用這個點像這樣去去渲染一個 `TodoItem` 列表（ES 語法）：

```html
<ul>{todoItems.map((todoItem, i) =>
    <li key={'todoitem' + i}><TodoItem owner={todoItem.owner} task={todoItem.task} /></li>
)}</ul>
```
You probably noticed something special here. What is that `key` property about? It is something that tells React the exact ordering of your items. If you don't provide unique keys for list items like this, React will warn you as it won't be able to guarantee the correct ordering otherwise.

你可能注意到這裡有些特殊的地方，什麼是 `key` 屬性？這是告訴 React 這個項的準確順序。如果你不需要像這樣需要提供列表項的唯一 Key 的話，否則 React 會警告你這樣它沒辦法保證是正確的順序。

This has to do with the fact that React actually implements something known as Virtual DOM (VDOM for short) on top of actual DOM. It is a subset of DOM that allows React to optimize its rendering. The primary advantage of this approach is that it allows React to eschew a lot of legacy our good old DOM has gained through years. This is the secret to React's high performance.

需要瞭解的事實是 React 實際上在真實 DOM 上實現了虛擬 DOM（簡稱 VDOM），這是一個 DOM 的子集，能夠讓 React 能夠優化它的渲染。這個優化的方法是它讓 React 能夠避開困惱我們多年的 DOM 性能損耗。這就是 React 高性能的原因。

### 整體組件

To give you a better idea of what components look like, let's expand our `TodoItem` example into code (ES6 + JSX). I've done this below and will walk you through it:

為了能夠讓你更加清楚組件長什麼樣子，讓我們拓展 `TodoItem` 例子來感受一下（ES6 + JSX），我已經搞完了，然後我們來過一遍看看：

```javascript
var React = require('react');


module.exports = React.createClass({
    getInitialState() {
        return {
            // 讓我們保持追蹤看看我們給項點了多少次贊
            likes: 0,
        };
    },
    render() {
        var owner = this.props.owner;
        var task = this.props.task;
        var likes = this.state.likes;

        return <div className='TodoItem'>
            <span className='TodoItem-owner'>{owner}</span>
            <span className='TodoItem-task'>{task}</span>
            <span className='TodoItem-likes'>{likes}</span>
            <span className='TodoItem-like' onClick={this.like}>Like</span>
        </div>;
    },
    like() {
        this.setState({
            likes: this.state.likes + 1
        });
    },
});
```

You can see some basic features of a React component above. First we create a class for our component. After that we define some initial state for it, then we render and finally we define some custom callbacks for our handlers if they exist. In this case I decided to implement an extra feature, liking. The current implementation just keeps track of the amount of likes per component.

你可以在上面看到一些基礎的 React 組件的功能，一開始我們創建了一個組件的類，然後我們在初始化的時候定義一些狀態，然後我們渲染，最後我們定製了一些回調。在這個例子中，我決定去實施一個額外的功能，點贊。這個工作只是保持了一個跟蹤點贊組件的計數。

In practice you would transmit like amounts to a backend and add some validation there but this is a good starting point for understanding how state works in React.

在實踐中，你需要把點贊計數傳輸給後端，然後添加一些驗證，不過目前這個階段對於理解 State 在 React 中的是如何使用的已經是一個非常好的開端了。

`getInitialState` and `render` are a part of a [React component's lifecycle as documented officially](http://facebook.github.io/react/docs/component-specs.html). There are additional hooks that allow you to do things like set up adapters for `jQuery` plugins and such.

`getInitialState` 和 `render` 是 [React 組件的生命周期官方文檔](http://facebook.github.io/react/docs/component-specs.html) 的一部分。也有額外的能夠讓你去設置加載 `jQuery` 插件之類的適配器的鉤子。

In this example CSS naming has been modeled after [Suit CSS](http://suitcss.github.io/) conventions as those look clean to me. That's just one way to deal with it.

在例子中的 CSS 的命名是根據 [Suit CSS](http://suitcss.github.io/) 約定處理後的，這樣對我來說看起來非常幹淨，這只是一種解決方案而已。

### 處理操作

Let's say we want to modify the owner of our TodoItems. For the sake of simplicity let's expect it's just a string and owner is the same for all TodoItems. Based on this design it would make sense to have an input for owner at our user interface. A naive implementation would look something like this:

讓我們來開始修改我們的 TodoItems 的 owner，為了簡化這個目的，我們假設 owner 只是一個字元串而且只有一個，基于這個設計，在用戶界面裡增加一個輸入框給用戶來修改用戶名，改動是這樣的：

```javascript
var React = require('react');
var TodoItem = require('./TodoItem.jsx');


module.exports = React.createClass({
    getInitialState() {
        return {
            todoItems: [
                {
                    task: 'Learn React',
                },
                {
                    task: 'Learn Webpack',
                },
                {
                    task: 'Conquer World',
                }
            ],
            owner: 'John Doe',
        };
    },

    render() {
        var todoItems = this.state.todoItems;
        var owner = this.state.owner;

        return <div>
            <div className='ChangeOwner'>
                <input type='text' defaultValue={owner} onChange={this.updateOwner} />
            </div>

            <div className='TodoItems'>
                <ul>{todoItems.map((todoItem, i) =>
                    <li key={'todoitem' + i}>
                        <TodoItem owner={owner} task={todoItem.task} />
                    </li>
                )}</ul>
            </div>
        </div>;
    },

    updateOwner() {
        this.setState({
            owner: e.target.value,
        });
    },
});
```

We could push `TodoItems` and `ChangeOwner` to separate components but I've kept it all in the same for now. Given React has one way binding by default, we get some extra noise compared to some other setups. React provides [ReactLink](http://facebook.github.io/react/docs/two-way-binding-helpers.html) helper to help deal with this particular case.

我們可以把 `TodoItems` 和 `ChangeOwner` 分離出去，但是我暫時不這麼做了。React 預設提供了單向數據綁定，我們可以通過設置來調整，React 提供了 [ReactLink](http://facebook.github.io/react/docs/two-way-binding-helpers.html) 來提供雙向數據綁定。

Even though lack of two way binding might sound like a downer, it actually isn't that bad a thing. It makes it easier to reason about the system. You simply have to follow the flow. This idea is highlighted in the Flux architecture. The easiest way to visualize it is to think up an infinite waterfall or a snake eating its tail. That's how the flow in the world of React generally works. Compared to this two way binding feels more chaotic.

儘管雙向數據綁定的缺失聽起來讓人沮喪，但實際上並不是一件壞事，它能夠讓系統更加輕鬆的運行，你可能需要跟蹤數據流。不過 FB 提出了 Flux 架構，最簡單的方式去想出一個無限瀑布流，或者貪吃蛇，就是 React 的世界在做的這個數據流的事情，對比這兩種綁定方式讓人感覺更加迷茫。

### 使用一個Mixin

If we wanted to model the code above using a ReactLink, we would end up with something like this:

如果我們想使用 ReactLink的話，就想下面這樣：

```javascript
// ReactLink 是一個插件，所以我們需要把它引入。
var React = require('react/addons');

...

module.exports = React.createClass({
    mixins: [React.addons.LinkedStateMixin],

    ...

    render() {
        var todoItems = this.state.todoItems;

        return <div>
            <div className='ChangeOwner'>
                <input type='text' valueLink={this.linkState('owner')} />
            </div>

            <div className='TodoItems'>
                <ul>{todoItems.map((todoItem, i) =>
                    <li key={'todoitem' + i}>
                        <TodoItem owner={owner} task={todoItem.task} />
                    </li>
                )}</ul>
            </div>
        </div>;
    },
});
```

Now we can skip that `onChange` handler. That `React.addons.LinkedStateMixin` encapsulates the logic. [Mixins](http://facebook.github.io/react/docs/reusable-components.html#mixins) provide us one way to encapsulate shared concerns such as this into something which can be reused easily.

現在我們可以跳過綁定 `onChange` 事件了，`React.addons.LinkedStateMixin` 封裝了邏輯。[Mixins](http://facebook.github.io/react/docs/reusable-components.html#mixins) 給我們提供了一種可以重複使用的封裝方法。

> It would be easy to start expanding the example now. You could for instance provide means to manipulate the contents of the Todo list or start extracting various parts into components of their own. It is up to you to make the app yours. If you are still feeling a bit lost, please read on. This is supposed to be a brief introduction to the topic!

> 現在拓展例子已經非常容易了，你現在應該就可以拓展 Todo 列表，或者把各種組件分離，這取決於你。如果你現在還是覺得不夠掌握，那麼繼續閱讀，這節只是話題的一個介紹。

### 測試

If you get serious about the Todo app, I recommend trying [Jest](https://facebook.github.io/jest/) out. Getting the initial test run might be a bit challenging but after you learn the basics of the API, it gets a lot simpler. The basic idea is that you instantiate a component with some properties and then query DOM using Jest and finally assert that the values in the UI are what you expect.

如果你覺得這個 Todo 應用需要測試，那麼我推薦 [Jest](https://facebook.github.io/jest/)，剛開始寫測試用例可能是一點挑戰但是在你學習一些基礎的 API 之後，它會變得更加簡單，最基本的是你實例化一個帶有一些屬性的組件的時候，然後用 Jest 查詢 DOM，最後斷言界面中的值是你期望的值。

When you go beyond component level, that is where tools such as Selenium come in. You can use standard end to end testing tools on a higher level.

當你站在組件層級之上時，就有了比如類似 Selenium 的組件，你可以在更高層級使用標準的端對端測試工具。

## Flux 架構及其變種

As you saw above, it is quite simple to throw together a couple of components and start building an app. You can get quite far with `props` and `state`. Just load up some data over AJAX at `getInitialState` and pass it around. After a while this all might start feeling a bit unwieldy. Why, for instance, my components should have to know something about how to communicate with the backend?

就想你看到的，把一些組件放到一起就可以組成一個應用，你可以用 `props` 和 `state` 做的更多，或者在 `getInitialState` 中使用 AJAX 加載數據然後傳給其他組件。這些使用一段時間之後可能會覺得有些笨拙，為什麼？實踐中，組件們需要知道如何和後端通訊。

This is where Flux architecture and its variants come in. I will start by describing [Reflux](https://github.com/spoike/refluxjs), a simplified variant of it. You can then work up to [understanding Flux](http://facebook.github.io/flux/docs/overview.html) in fuller detail once you understand this simplified setup.

所以出現了 Flux 架構和它的一些變種，我會介紹 [Reflux](https://github.com/spoike/refluxjs) 一種非常簡單的 Flux 變種，你可以閱讀這個 [understanding Flux](http://facebook.github.io/flux/docs/overview.html) 來全面瞭解 Flux。

In addition to View Components which we just discussed, Reflux introduces the concepts of Actions and Stores. The flow goes like this: Components -> Actions -> Stores -> Components. Ie. you could have some control in a Component which then triggers some Action which then performs some operation (say PUT) and updates Store state. This state change is then propagated to Components listening to the Store.

並且，Reflux 介紹了一種 Actions 和 Stores 的概念去組成剛纔我們討論的試圖組件。整個數據流是這樣的：組件 -> Actions -> Stores -> 組件。這樣你可以在一個組件中控制觸發一些 Action 然後執行一些操作（比如 PUT）然後更新 Store 的狀態，狀態的更新傳播給監聽 Store 的那些組件們。

In case of our Todo example we would define basic `TodoActions` like create, update, delete and such. We would also have a `TodoStore`. It would maintain a data structure of a `TodoList`. Our components would then consume that data and display it as appropriate.

在我們的這個 Todo 例子中，我們定義了一個基礎的 `TodoActions` 比如創建、更新、刪除之類的，我們也有個 `TodoStore`，它是整個 `TodoList` 的資料結構中心，組件們會讀取那裡的數據，然後適當得展現出來。

As development of Reflux is quite in flux I won't give you a full example in this case. I just wanted to illustrate one possible way to deal with scaling up from bare React. You should explore various options and deepen your understanding of possible architectures. The ideas are quite similar but the devil is in the details as always. There are always drawbacks to consider.

Reflux 的開發和 Flux 差不多，我就不在這裡詳細講述了，我只是想說明如何處理從純 React 擴大的一種可能的方式。你需要去瀏覽一些相關看法，然後深入理解那些架構。那些想法都差不多，就是細節不太一樣，都有各自的一些缺點有待考略。

### 同構渲染

One of the big features which React provides thanks to its design is so called isomorphic rendering. Back in the day we used to render whole HTML in the backend and provide just that for the client to render. Then we would sprinkle a little JavaScript magic to make things more interactive and so on. After a while the pendulum swung to frontend side. We served minimal amount of HTML to the client and constructed the rest, including routing, using JavaScript entirely on frontend.

React 設計了一種叫做兩端渲染的重要功能。過去我們在後端渲染整個 HTML，然後交給客戶端去渲染，然後我們加上一點點 JavaScript 做的交互。再後來，這個活交給瀏覽器做去，後端返回最小的 HTML 給客戶端，然後在客戶端構建了一整套用 JavaScript 全權控制的系統，包括路由。

The main problems with frontend driven rendering have to do with performance, high dependency on JavaScript (think of the noscript folk!) and poor SEO. With isomorphic rendering you can mitigate these problems effectively. React allows you to prerender HTML at backend side. You can also hydrate some stores with pre-existing data making it possible to skip certain data queries altogether initially! Even web crawlers will be happy as they get some HTML to scrape.

前端驅動渲染的主要問題是性能問題、 JavaScript 的高度依賴（設想那些不能運行 JavaScript 的情況）和 SEO 問題。使用兩端渲染，你可以輕鬆解決這些問題。React 允許你在伺服器端預渲染 HTML，你也可以在伺服器端預先存儲一些數據來跳過初始化的一些數據查詢。甚至一些網絡爬蟲能夠輕鬆地獲取到一些 HTML 內容。

This is still partly uncharted territory. Various implementations of Flux still struggle with the concept. I have no doubt we will see stronger solutions in the future, however, as people learn to deal with isomorphism better. That said isomorphic rendering can be considered a nice extra capability to have but it definitely isn't something that's just must have. There are some ways to work around certain issues, such as poor SEO, even without it. It just depends on where you want to put the effort.


這一部分仍然是一個未知的領域（譯者註：原文發佈于2015年4月），很多 Flux 的實現在這個概念上還沒同意，但是毋庸置疑，我們在未來能夠看到更加完善的解決方案，無論如何，大家會認識到兩端渲染的好處。雖然兩端渲染聽起來是一個非常好的解決方案，但是它不是必要的。現在已經有很多能夠解決這些問題的方案，比如 SEO 的問題，甚至沒有用這種解決方案，它只是依賴於你想要如何付出努力。
