## createClass

```jsx
import React from 'react';
let Greeting = React.createClass({
  // prop验证
  propTypes: {
    name: React.PropTypes.string //属性校验
  },
  // 默认props
  getDefaultProps: function() {
    return {
      name: 'Mary' //默认属性值
    };
  },
  // 初始state
  getInitialState: function() {
    return {count: this.props.initialCount}; //初始化state
  },
  
  handleClick: function() {
    //用户点击事件的处理函数
    // React对属性中的所有函数都进行了this绑定
  },

  render: function() {
    return <h1>Hello, {this.props.name}</h1>;
  }
});

export default Greeting;
```


## ES6 Classes

```jsx
import React from 'react';
class Greeting extends React.Component {

  constructor(props) {
    super(props);
    this.state = {
        count: props.initialCount
    };
    this.handleClick = this.handleClick.bind(this);
  }
  
  //static defaultProps = {
  //  name: 'Mary'  //定义defaultprops的另一种方式
  //}
  
  //static propTypes = {
    //name: React.PropTypes.string
  //}
  
  handleClick() {
    //点击事件的处理函数
  }
  
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}

Greeting.propTypes = {
  name: React.PropTypes.string
};

Greeting.defaultProps = {
  name: 'Mary'
};

export default Greating;
```
**注意**：
- 用`ES6 Classes`创建组件时，React并没有对内部的函数，进行this绑定。
- 不支持`mixins`
- 组件的`props`是父组件通过调用子组件向子组件传递的，子组件内部不应该对`props`进行修改，它更像是所有子组件实例共享的状态，不会因为子组件内部操作而改变，因此将`props`定义为类Greeting的属性更为合理，而在面向对象的语法中类的属性通常被称作静态(static)属性，这也是为什么`props`还可以像上面注释掉的方式来定义。
- 对于`Greeting`类的一个实例对象的`state`，它是组件对象内部维持的状态，通过用户操作会修改这些状态，每个实例的`state`也可能不同，彼此间不互相影响，因此通过`this.state`来设置

## 无状态函数

```jsx
import React from 'react';
const Button = (props) => {
  return (
    <div>
      <button onClick={ props.increment }>Today is { props.day }</button>
    </div>
  )
}

Button.propTypes = {
  day: PropTypes.string.isRequired,
  increment: PropTypes.func.isRequired,
}
Button.defaultProps = {
  day: 'Monday'
}

export default Button;
```
**注意**：
- 这种组件，没有自身的状态，所有数据都是通过props传入

## PureComponet
当组件的`props`或者`state`发生变化的时候：React会对组件当前的`Props`和`State`分别与`nextProps`和`nextState`进行比较，当发现变化时，就会对当前组件以及子组件进行重新渲染，否则就不渲染。有时候为了避免组件进行不必要的重新渲染，我们通过定义`shouldComponentUpdate`来优化性能

```jsx
class CounterButton extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
        count: 1
    };
  }

  shouldComponentUpdate(nextProps, nextState) {
    if (this.props.color !== nextProps.color) {
      return true;
    }
    if (this.state.count !== nextState.count) {
      return true;
    }
    return false;
  }

  render() {
    return (
      <button
        color={this.props.color}
        onClick={() => this.setState(state => ({count: state.count + 1}))}>
        Count: {this.state.count}
      </button>
    );
  }
}
```
React提供了`PureComponent`自动为我们添加的`shouldComponentUpate`函数（只是对`props`和`state`进行浅比较,即不能为引用类型）

```jsx
class ListOfWords extends React.PureComponent {
  render() {
    return <div>{ this.props.words.join(',') }</div>;
  }
 }
 
class WordAdder extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      words: ['marklar']
    };
    this.handleClick = this.handleClick.bind(this);
  }
  
  handleClick() {
    // 这个地方导致了bug
    const words = this.state.words;
    words.push('marklar');
    this.setState({words: words});
  }

  render() {
    return (
      <div>
        <button onClick={ () => this.handleClick() } />
        <ListOfWords words={ this.state.words } />
      </div>
    );
  }
}
```
`PureComponent`只会对`this.props.words`进行一次浅比较，虽然数组里面新增了元素，但是`this.props.words`与`nextProps.words`指向的仍是同一个数组，因此`this.props.words !== nextProps.words` 返回的便是`flase`，从而导致`ListOfWords`组件没有重新渲染


**参考**：
- [谈一谈创建React Component的几种方式](https://segmentfault.com/a/1190000008402834?utm_source=tuicool&utm_medium=referral)
- [React 中文文档](http://reactjs.cn/react/docs/reusable-components-zh-CN.html)
- [React.createClass和extends Component的区别](https://segmentfault.com/a/1190000005863630)
- [React PureComponent 源码解析](https://segmentfault.com/a/1190000006741060?utm_source=tuicool&utm_medium=referral)