我记得当我第一次看到 React 的时候是十分怀疑的，特别是把 HTML 揉进代码里违背了传统的那些约定，看起来像是一个 “坏注意”。

但是这就是 React，他们挑战了一些约定，然后用这种方式代替了。作为一个开发者，有时候为了能够改变思考方式是你向前走的一个必经之路。这就是 React 对我的影响，它建立在函数式编程上带给了我很多有意思的想法。

## 基础功能

在你能理解 React 是如何改变 Web 开发之前，这里有一点东西你需要知道的。React 它本身是不够丰富的，它只能解决一些视图上的问题，你仍然需要一些东西去帮助它完成事情。

最伟大的和最差劲的框架总是企图给你建立一个笼子，当你在他们划定的地盘里能够完成任何需求，一切都是很好的，但是，当你需要跨出边界的时候，开始有问题了，在一个库驱动方法里你是不被限制的，最初你可能是高效地完成需求，但是伴随时间的推进，问题开始变得严峻起来，你需要更多可行的选择。

### JSX 的基础

React 为前端开发提供了一个组件为中心的方法，你可以为你的应用设计一个更小的组件，所有组件有各自的目的，甚至极端来说，一个组件可能包含它自身的逻辑，结构和基本的样式。这里有个 JSX 的例子：
```html
...
<TodoItem className='urgent' owner={owner} task='Make a dinner' />
...
```

你可以看到一个基本功能的 JSX，我们用 `className` 替代了使用 `class`。此外，我们定义了一些自定义属性 `owner` 和 `task`，`owner` 的值是从 JSX 同环境中一个变量叫 `owner` 注入，而我们为 `task` 提供了一个固定的值。

在实践中，你可以根据你的数据模型结构来稍微调整目录结构会更好，虽然是 React 进阶的知识点了。

我们可以用那些 `{}` 来混合普通 JavaScript 代码，我们可以用这个点像这样去去渲染一个 `TodoItem` 列表（ES 语法）：

```html
<ul>{todoItems.map((todoItem, i) =>
    <li key={'todoitem' + i}><TodoItem owner={todoItem.owner} task={todoItem.task} /></li>
)}</ul>
```

你可能注意到这里有些特殊的地方，什么是 `key` 属性？这是告诉 React 这个项的准确顺序。如果你不需要像这样需要提供列表项的唯一 Key 的话，否则 React 会警告你这样它没办法保证是正确的顺序。

需要了解的事实是 React 实际上在真实 DOM 上实现了虚拟 DOM（简称 VDOM），这是一个 DOM 的子集，能够让 React 能够优化它的渲染。这个优化的方法是它让 React 能够避开困恼我们多年的 DOM 性能损耗。这就是 React 高性能的原因。

### 整体组件

为了能够让你更加清楚组件长什么样子，让我们拓展 `TodoItem` 例子来感受一下（ES6 + JSX），我已经搞完了，然后我们来过一遍看看：

```javascript
var React = require('react');


module.exports = React.createClass({
    getInitialState() {
        return {
            // 让我们保持追踪看看我们给项点了多少次赞
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

你可以在上面看到一些基础的 React 组件的功能，一开始我们创建了一个组件的类，然后我们在初始化的时候定义一些状态，然后我们渲染，最后我们定制了一些回调。在这个例子中，我决定去实施一个额外的功能，点赞。这个工作只是保持了一个跟踪点赞组件的计数。

在实践中，你需要把点赞计数传输给后端，然后添加一些验证，不过目前这个阶段对于理解 State 在 React 中的是如何使用的已经是一个非常好的开端了。

`getInitialState` 和 `render` 是 [React 组件的生命周期官方文档](http://facebook.github.io/react/docs/component-specs.html) 的一部分。也有额外的能够让你去设置加载 `jQuery` 插件之类的适配器的钩子。

在例子中的 CSS 的命名是根据 [Suit CSS](http://suitcss.github.io/) 约定处理后的，这样对我来说看起来非常干净，这只是一种解决方案而已。

### 处理操作

让我们来开始修改我们的 TodoItems 的 owner，为了简化这个目的，我们假设 owner 只是一个字符串而且只有一个，基于这个设计，在用户界面里增加一个输入框给用户来修改用户名，改动是这样的：

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

我们可以把 `TodoItems` 和 `ChangeOwner` 分离出去，但是我暂时不这么做了。React 默认提供了单向数据绑定，我们可以通过设置来调整，React 提供了 [ReactLink](http://facebook.github.io/react/docs/two-way-binding-helpers.html) 来提供双向数据绑定。

尽管双向数据绑定的缺失听起来让人沮丧，但实际上并不是一件坏事，它能够让系统更加轻松的运行，你可能需要跟踪数据流。不过 FB 提出了 Flux 架构，最简单的方式去想出一个无限瀑布流，或者贪吃蛇，就是 React 的世界在做的这个数据流的事情，对比这两种绑定方式让人感觉更加迷茫。

### 使用一个Mixin

如果我们想使用 ReactLink的话，就想下面这样：

```javascript
// ReactLink 是一个插件，所以我们需要把它引入。
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

现在我们可以跳过绑定 `onChange` 事件了，`React.addons.LinkedStateMixin` 封装了逻辑。[Mixins](http://facebook.github.io/react/docs/reusable-components.html#mixins) 给我们提供了一种可以重复使用的封装方法。

> 现在拓展例子已经非常容易了，你现在应该就可以拓展 Todo 列表，或者把各种组件分离，这取决于你。如果你现在还是觉得不够掌握，那么继续阅读，这节只是话题的一个简介。

### 测试

如果你觉得这个 Todo 应用需要测试，那么我推荐 [Jest](https://facebook.github.io/jest/)，刚开始写测试用例可能是一点挑战但是在你学习一些基础的 API 之后，它会变得更加简单，最基本的是你实例化一个带有一些属性的组件的时候，然后用 Jest 查询 DOM，最后断言界面中的值是你期望的值。

当你站在组件层级之上时，就有了比如类似 Selenium 的组件，你可以在更高层级使用标准的端对端测试工具。

## Flux 架构及其变种

就想你看到的，把一些组件放到一起就可以组成一个应用，你可以用 `props` 和 `state` 做的更多，或者在 `getInitialState` 中使用 AJAX 加载数据然后传给其他组件。这些使用一段时间之后可能会觉得有些笨拙，为什么？实践中，组件们需要知道如何和后端通讯。

所以出现了 Flux 架构和它的一些变种，我会介绍 [Reflux](https://github.com/spoike/refluxjs) 一种非常简单的 Flux 变种，你可以阅读这个 [understanding Flux](http://facebook.github.io/flux/docs/overview.html) 来全面了解 Flux。

并且，Reflux 介绍了一种 Actions 和 Stores 的概念去组成刚才我们讨论的试图组件。整个数据流是这样的：组件 -> Actions -> Stores -> 组件。这样你可以在一个组件中控制触发一些 Action 然后执行一些操作（比如 PUT）然后更新 Store 的状态，状态的更新传播给监听 Store 的那些组件们。

在我们的这个 Todo 例子中，我们定义了一个基础的 `TodoActions` 比如创建、更新、删除之类的，我们也有个 `TodoStore`，它是整个 `TodoList` 的数据结构中心，组件们会读取那里的数据，然后适当得展现出来。

Reflux 的开发和 Flux 差不多，我就不在这里详细讲述了，我只是想说明如何处理从纯 React 扩大的一种可能的方式。你需要去浏览一些相关看法，然后深入理解那些架构。那些想法都差不多，就是细节不太一样，都有各自的一些缺点有待考略。

### 两端渲染

React 设计了一种叫做两端渲染的重要功能。过去我们在后端渲染整个 HTML，然后交给客户端去渲染，然后我们加上一点点 JavaScript 做的交互。再后来，这个活交给浏览器做去，后端返回最小的 HTML 给客户端，然后在客户端构建了一整套用 JavaScript 全权控制的系统，包括路由。

前端驱动渲染的主要问题是性能问题、 JavaScript 的高度依赖（设想那些不能运行 Javascript 的情况）和 SEO 问题。使用两端渲染，你可以轻松解决这些问题。React 允许你在服务器端预渲染 HTML，你也可以在服务器端预先存储一些数据来跳过初始化的一些数据查询。甚至一些网络爬虫能够轻松地获取到一些 HTML 内容。

这一部分仍然是一个未知的领域（译者注：原文发布于2015年4月），很多 Flux 的实现在这个概念上还没同意，但是毋庸置疑，我们在未来能够看到更加完善的解决方案，无论如何，大家会认识到两端渲染的好处。虽然两端渲染听起来是一个非常好的解决方案，但是它不是必要的。现在已经有很多能够解决这些问题的方案，比如 SEO 的问题，甚至没有用这种解决方案，它只是依赖于你想要如何付出努力。
