# React 学习

## 搭建 react 环境

- 引入以下文件

```
<script src="https://cdn.staticfile.org/react/16.4.0/umd/react.development.js"></script>- React 的核心库
<script src="https://cdn.staticfile.org/react-dom/16.4.0/umd/react-dom.development.js"></script>- 提供与 DOM 相关的功能
<!-- 生产环境中不建议使用 -->
<script src="https://cdn.staticfile.org/babel-standalone/6.26.0/babel.min.js"></script>- Babel 可以将 ES6 代码转为 ES5 代码
```

- nodejs 中安装 react
>$ cnpm install -g create-react-app  
>$ create-react-app my-app  
>$ cd my-app/  
>$ npm start
>$ npm run dev
- 关闭 npm run dev
>$ ctrl+c
- 端口占用
>$ sudo lsof -i tcp:8080
>$ kill PID

## 使用 React.Component

```
class ShoppingList extends React.Component {
  render() {
    return (
      <div className="shopping-list">
        <h1>Shopping List for {this.props.name}</h1>
        <ul>
          <li>Instagram</li>
          <li>WhatsApp</li>
          <li>Oculus</li>
        </ul>
      </div>
    );
  }
}
```
ShoppingList 为 React 组件类，每个组件接受 props 参数，并通过 render 方法返回嵌套视图（被封装为 React 元素）

## React 生命周期

- initialization 组件初始化阶段  
  使用类的构造方法(constructor())做一些组件的初始化工作

```
// Test 类只有继承了 Component 这个基类才能有 render()，生命周期等方法  
// super(props)调用基类的构造方法，同时将父组件的 props 注入子组件
import React, { Component } from 'react';
class Test extends Component {
  constructor(props) {
    super(props);
  }
}
```

- 挂载阶段(componentWillMount, render, componentDidMount)
  componentWillMount(): 在组件挂载到 DOM 之前调用，只会调用一次，调用 this.setState 不会引起组件重新渲染。  
  render(): 在 render 函数中，只要 props 和 state 发生了重传递和重赋值，都会引起组件的重新渲染，render 函数是纯函数，不能更改组件的参数，不能执行 this.setState。  
  componentDidMount(): 组件挂载到 DOM 后调用，只会调用一次。
- 更新阶段(props 更新:componentWillReceiveProps, shouldComponentUpdate, componentWillUpdate, componentDidUpdate, state 更新: shouldComponentUpdate, componentWillUpdate, componentDidUpdate, 区别在于 props 有 componentWillReceiveProps 阶段)  
  componentWillReceiveProps(nextProps): 父组件发生 props 更新就会调用这个方法  
  shouldComponentUpdate(nextProps, nextState): 函数接受两个参数(nextProps, nextState)，分别表示下一个 props 和 state 的值，当 props 和 state 没有变化时，阻止接下来 render 重新渲染的工作。PureComponent 就是重写了 shouldComponentUpdate(),然后在里面作了 props 和 state 的浅层对比。

```
shouldComponentUpdate(nextProps,nextState){
  if(nextProps.number == this.props.number){
    return false
  }
  return true
}
```

componentWillUpdate(nextProps, nextState): 在调用 render 方法前执行一些组件更新前的工作  
render(): 重新渲染  
componentDidUpdate(prevProps, pervState): 组件更新后调用。

- 卸载阶段(componentWillUnmount)
  componentWillUnmount: 执行一些清理工作，比如清除组件中的定时器，手动创建的 DOM 元素等，避免引起内存泄漏
- v16.4 推出了 Fiber，开启了 async rendering，在 render 函数之前的所有函数都可能被多次执行。
  比如：挂载阶段的 componentWillMount(), 更新阶段的 componentWillReceiveProps(), shouldComponentUpdate(), componentWillUpdate(), 这些在 render 函数之前的所有函数都被 getDerivedStateFromProps(props, state)替代，唯独保留了 shouldComponentUpdate()函数，渲染之后的函数被 getSnapComponentUpdate(props, state)函数替代  
  v16.4 之后的 update 阶段: getDerivedStateFromProps(props, state) -> shouldComponentUpdate(nextProps, nextState) -> render() -> getSnapComponentUpdate(props, state) -> componentDidUpdate(preProps, preState, snapshot)  
  v16.4 之后的卸载阶段: componentWillUnmount()

## state 与 props 的区别

- props 不可变，state 可以根据与用户的交互来改变
- 以上导致了容器组件一般是定义 state 来更新和修改数据，而通过 props 来传递数据
- state 不能直接修改，而应该使用 setState(),例如：this.setState({comment: 'Hello'});构造函数是唯一可以给 this.state 赋值的地方。由于 this.props 和 this.state 可能是异步更新的，所以不能依靠它们的值来计算下一个状态，而应该使用以下方法，将先前的状态作为第一个参数，此次更新被应用时的 props 作为第二个参数

```
this.setState((prevState, props) => ({
  counter: prevState.counter + props.increment
}));
```

## 使用 state 控制页面的显示隐藏

```
//父组件Function中
function FatherUnit(props){
  const {visible: visibleProp, ...otherProps} = props;
  const [visible, setVisible] = useState(visibleProp);   //使用props中的visible作为初始值设置函数内部的visible状态，可以在函数内部通过setVisible改变
  useEffect(() => {
    setVisible(visibleProp);
  },[visibleProp]);                 //每次visible有变化就执行useEffect()
  <ChildSubunit visible={visible}   //父组件传递给子组件的visible
    onOk={v => {                    //子组件中需要初始化的函数
      onOk(v);                      //调用上级组件的onOk()
      setVisible(false);
    }}
    onCancle={() => {
      setVisible(false);
    }}
  >
  </ChildSubunit>
}
//不能在组件内部通过this.state修改状态，因为该状态会在调用setState()后被替换。
//父组件class中
class Page extends React.PureComponent {
  constructor(props) {
    super(props);
    this.state = {
      visible: false,
      ...otherState: defaultValue,     //初始化state
    }
  }
  render() {
    const {
      visible,
      ...otherState,
    } = this.state;                    //将state析构
    return (
      <ChildSubunit visible={visible}  //控制子组件的显示隐藏
        onOk={() => {
          this.setState({
            visible: false,
          });
        }}
        onCancle={()=>{
          this.setState({
            visible: false,
          });
        }}
      ></ChildSubunit>
    )
  }
}
//子组件中
function ChildSubunit(props){
  const {visible: visibleProp = true, onOk = () => {}, onCancel = () => {},} = props;
  const [visible, setVisible] = useState(visibleProp);
  useEffect(() => {setVisible(visibleProp);[visibleProp]})
  return(
    <div visible={visible}>
      <button onClick={() => {
        onCancel();
      }}></button>
      <button onClick={() => {
        onOk();
      }}></button>
    </div>
  )
}
```

## React Hooks

React Hooks 使得 function 组件可以像 class 组件一样维护状态。

```
function App(){
  const [inputValue, setInputValue] = useState('');
  const [todos, setTodos] = useState(undefined);
  const [error, setError] = useState('');
}
```

useState()的参数是状态的初始值，它返回一个读取这个状态的只读引用，一个设置状态的函数。  
每次这个组件被重新渲染时，App() 这个函数都会被调用。每个 useState 只有第一次被调用时返回的状态是初始值，之后每次都会返回已经记住的当前值。这里有三个状态，React 是用调用 useState 的顺序来区分他们。可以理解为 App() 的所有状态存储在一个数组里，第一个 useState() 返回的是第一个状态，第二个 useState() 返回的是第二个状态，以此类推。所以使用 hook 必须保证这个组件函数每次运行中： 
- 对 useState() 的调用次数必须是一样的。  
- 与各状态对应的 useState()的调用顺序是一样的。  
- 这就意味着 useState() 的调用不能放在条件分支或循环中。为了避免出错，最好把所有 useState() 调用放在函数开头。
- useState中可以放置回调函数，当它接受一个函数时，就变成了Lazy initialization (延迟初始化)，该函数返回值即为initialState

```
useEffect(() => {
  console.log(count);
}, [count]);
```
useEffect有两个参数callback和dependencies数组，如果dependencies不存在，那么callback每次render都会执行，如果dependencies存在，只有当dependencies发生变化，才会执行callback。当dependencies为空时，相当于componentDidMount，因为依赖一直不发生改变，callback不会再次执行。  
useEffect中可以接受return回调函数，会在组件unmount时触发。
## React 与 Vue 的异同点

- React 和 Vue 都是使用组件来实现拼接页面的，但是 React 的组件是函数式的思想，

- React 是单向数据流，数据是不可变的，当数据改变需要重新渲染组件，是通过在 shouldComponentUpdate()中进行手动比较，比较的深度可以手动控制，因此 React 的性能优化需要手动去做。而 Vue 是数据驱动的，通过对每一个属性建立 Watcher 监听数据变化，响应式的更新对应的虚拟 dom。由于 Vue 使用监听函数监听数据变化，因此当状态特别多的时候（大型应用）会导致卡顿，此时应使用 React 更加可控。
- React 的生命周期分为组件初始化、更新阶段、销毁阶段。Vue 生命周期分为创建阶段、挂载阶段、更新阶段、销毁阶段。
- React 使用 js、css、html 分离的方式，更多的是使用 js 来生成 html，操作 css，因此 React 设计了 JSX。Vue 将 js、css、html 组合到一起，通过指令实现的，Vue 提供了模版引擎来处理。
- React 采用 class，而 Vue 是声明式写法。
- 在不使用 ReactX 和 VueX 时，React 和 Vue 组件之间的通信。React 和 Vue 在父组件向子组件传值时，都是在父组件引用子组件时将属性写在子组件标签中，React 在父组件还要声明属性变量后就可以直接使用，而 Vue 需要在子组件中将属性注入。React 和 Vue 在子组件向父组件传值时，由于 React 是单向数据流，因此不会有这种情况，Vue 在传值时，父组件中需要声明属性变量，子组件中使用 emit()将属性散布到父组件中。

## JSX

- 为避免遇到自动插入分号的陷阱，当 JSX 被拆分为多行时，要将内容包裹在括号中
- 引号中使用字符串值，大括号中使用表达式，React DOM 使用小驼峰命名属性名称，而不使用-分隔符，class 和 for 应该使用 className 和 htmlFor 来做对应的属性。在设置内联样式时，React 会在指定元素后自动添加 px。自定义的 React 类名以大写字母开头。
- React DOM 在渲染之前会对所有输入内容进行转义，有效防止了 XSS 攻击
- Babel 将 JSX 转译为 React.createElement()函数调用

```
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);
```

```
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);
```

```
const element = {
  type: 'h1',
  props: {
    className: 'greeting',
    children: 'Hello, world!'
  }
};
```

以上是 JSX 代码转译过程。

- 大多数 React 应用只会调用一次 ReactDOM.render()。
- 组件名称使用大写字母开头，小写字母为原生 DOM 标签。
- 如果在定义函数的时候没有在方法后面添加()，例如 onClick={this.handleClick}，此时应该为这个方法绑定 this（class 的方法默认不会绑定 this），this.handleClick = this.handleClick.bind(this);
- 与&&或||运算符实现条件表达式，{unreadMessages.length > 0 && <div>this is {unreadMessages.length}.</div>}，true && expression == expression，将渲染 expression，而 false && expression == false，将不会渲染 expression。
- 三目表达式 condition ? true : false。
- render 方法返回 null，将不会进行渲染。

* sticky / can i use/ reset(重定样式)/cssnext playground/dva

## Sass
- $变量名:变量值
- 父选择器的标识符&，相当于将&直接替换为父选择器
- 直接子元素组合选择器>、同层相邻组合选择器+、同层全体组合选择器～
- sass在@import时不需要指明被导入文件的全名，可以省略后缀名。当通过@import把sass样式分散到多个文件时，你通常只想生成少数几个css文件。那些专门为@import命令而编写的sass文件，并不需要生成对应的独立css文件，这样的sass文件称为局部文件（以下划线开头，导入时可省略）
- 静默注释//
- 混合器@minxin，可以通过@include引入
```
@mixin no-bullets {
  list-style: none;
  li {
    list-style-image: none;
    list-style-type: none;
    margin-left: 0px;
  }
}
ul.plain {
  color: #444;
  @include no-bullets;
}
```
- 混合器传参
```
@mixin link-colors($normal, $hover, $visited) {
  color: $normal;
  &:hover { color: $hover; }
  &:visited { color: $visited; }
}
a {
  @include link-colors(blue, red, green);
}
//sass允许通过语法$name: value的形式指定每个参数的值。这种形式的传参，参数顺序就不必再在乎了，只需要保证没有漏掉参数即可
a {
    @include link-colors(
      $normal: blue,
      $visited: green,
      $hover: red
  );
}
```
- 选择器继承@extend
```
.error {
  border: 1px solid red;
  background-color: #fdd;
}
.seriousError {
  @extend .error;
  border-width: 3px;
}
```