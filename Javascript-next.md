## 類

As of React JS 0.13 you will be able to define components as classes.

在 React JS 0.13 中，你可以把組件定義為類。

```javascript
class MyComponent extends React.Component {
  constructor() {
    this.state = {message: 'Hello world'};
  }
  render() {
    return (
      <h1>{this.state.message}</h1>
    );
  }
}
```

This gives you a very short and nice syntax for defining components. A drawback with using classes though is the lack of mixins. That said, you are not totally lost. Lets us see how we could still use the important **PureRenderMixin**.

這樣能夠寫出非常短且優雅的語法來定義組件。使用類的缺點是缺乏了很多 mixin，不過，不是所有的不能用，讓我們來看看使用重要的 **PureRenderMixin**：

```javascript
import React from 'react/addons';

class Component extends React.Component {
  shouldComponentUpdate() {
    return React.addons.PureRenderMixin.shouldComponentUpdate.apply(this, arguments);
  }
}

class MyComponent extends Component {
  constructor() {
    this.state = {message: 'Hello world'};
  }
  render() {
    return (
      <h1>{this.state.message}</h1>
    );
  }
}
```