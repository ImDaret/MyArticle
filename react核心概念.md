---
title: React核心概念
---
# 一、最简易的React示例
```javascript
ReactDom.render(
    <div>Hello, ling!</div>,
    document.getElementById('root')
);
```
上面这个例子将会在浏览器中展示 "Hello, ling!" 。

# 二、JSX？
## 为什么是JSX？
React 并不强制使用 JSX，但是大部分人觉得将 JSX 和 UI 放在一起时，能提升视觉效果，并且可以使 React 提示更多的错误和警告。

## JSX编译
Babel 会把 JSX 转译成一个名为 `React.createElement()` 函数调用，例如下面的代码是等效的：
```javascript
const  element = (
    <div className="myEle">
        Hello, ling!
    </div>
);
```

```javascript
const element = React.createElement(
    'div',
    {className: 'myEle'},
    'Hello, ling!'
);
```
`React.createElement`实际上会创造以下一个对象（经简化）：
```javascript
const element = {
    type: 'div',
    props: {
        className: 'myEle',
        children: 'Hello, ling!'
    }
}
```

# 三、元素如何渲染？
元素是构成 React 应用的最小砖块，与浏览器的 DOM 元素不同，React 元素是创建开销极小的普通对象。React DOM 会负责更新 DOM 来与 React 元素保持一致。

## 开始渲染
假设现在 HTML 上有一个根结点 `<div id="root"></div>`, 如果你想要在这个根结点中渲染元素，只需要把他们传入 `ReactDom.render()` 中就可。就像下面这样：
```javascript
const element = (<div>Hello, ling</div>);
ReactDom.render(element, document.getElementById('root'))
```

## 如何更新已经渲染的元素？
React 元素是不可变对象，一旦被创建就无法更改它的子元素或者属性，唯一的更新方法就是重新传入 `ReactDom.render()`，例如下面这个例子：
```javascript
function Timer() {
    const element = (
        <div>Now, is {new Date()}</div>
    );
    ReactDom.render(element, document.getElementById('root'))
}

setInterval(Timer, 1000);
```
然而，在实际实践中，大多数的 React 应用只会调用一次 `ReactDom.render()` 在**第五节**中会解决这个问题。

## 只更新需要的部分
利用上面这个例子，使用浏览器检查元素工具可以发现即使每隔一秒都会传入一个全新的元素，但是 React 只会更新它需要更新的部分，例如上面的代码只会更新 `{new Date()}` 中的内容。

# 四、组件 && Props
## 组件类型
React 有两种组件，一种是函数组件，另一种是 class 组件。顾名思义，函数组件使用 `function` 定义的组件，class 组件是用 `class` 定义的组件。这两者在效果上是等效的，但是在其他特性上有一定差异，详见**第五节**。

## 渲染组件
React 元素可以是自定义组件，JSX 会将接收的属性和子组件转换为 `props` 对象传递给组件。例如下面这段代码将会渲染 “Hello, ling!”：
```javascript
function Welcome(props) {
    return (
        <div>Hello, {props.name}!</div>
    );
}

const element = (<Welcome name="ling" />);
ReactDom.render(element, document.getElementById('root'))
```
以上这个例子发生了什么：
-  `ReactDom.render()` 函数传入 element 元素，也就是 Welcome 组件。
- React 调用 Welcome 组件并将 `{name: 'ling'}` 作为 props 对象传入。
- Welcome 组件将 `<div>Hello, ling!</div>` 作为返回值。
- React Dom 将 DOM 高效更新为 `<div>Hello, ling!</div>`。

**注意**：React 组件名称必须大写开头，因为 React 会将小写的标签当作原生 DOM 标签，而以大写开头则代表是一个组件，原因将在下一期 **React高级指引** 中详细展开。

## 组合组件
引用上面的 `<Welcome />` 组件，我们可以多次调用：
```javascript
function App() {
    return (
        <div>
            <Welcome name="ling" />
            <Welcome name="yi" />
        </div>
    )
}

ReactDom.render(<App />, document.getElementById('root'));
```
通常来说，每个 React 应用的最顶层都是 `<App />` 组件。你可能需要引入一些类似 `Button` 这样的小组件，并将他们自下而上地运用到每一处。为什么说是自下而上的，因为自下而上意味着从最基础的组件开始写，比较适合大部分大一点的项目，而自上而下适合简单的应用。

## 拆分组件
拆分组件可以使代码更加易读和维护，在大型应用中，更应该构建可复用组件库。如果代码中有一部分被重复调用或者具有一定复杂性，那么这部分代码是值得被拆分的。例如下面是 React 官方拆分组件的示例：
```javascript
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <img className="Avatar"
          src={props.author.avatarUrl}
          alt={props.author.name}
        />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```
首先提取 `Avatar` 组件, 这里建议将 author 改为 user, 因为 `Avatar` 并不知道 `Comment` 是用来干嘛的，官方建议从组件自身的角度命名属性。
```javascript
function Avatar(props) {
    return (
        <img className="Avatar"
          src={props.user.avatarUrl}
          alt={props.user.name}
        />
    )
}
```
再提取 `UserInfo` 组件, 并引入 `Avatar` 组件
``` javascript
function UserInfo(props) {
    <div className="userInfo">
        <Avatar user={props.user}/>
        <div className="UserInfo-name">
            {props.user.name}
        </div>
    </div>
}
```
最终形成的 `Comment` 组件如下：
```javascript
function Comment(props) {
  return (
    <div className="Comment">
      <UserInfo user={props.author} />
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

## props 是只读的
React 有一个严格的规则：**所有 React 组件都必须像纯函数一样保护它们的 props 不被更改**，什么是纯函数？对比下面两个例子就知道了：
```javascript
function(a, b) {
    return a + b
}
```
这是一个纯函数，因为入参 a 和 b 没有发生改变。
```javascript
function (total, a) {
    total = total+a
}
```
这不是一个纯函数，因为入参 total 发生了改变。

# 五、 State && 生命周期
引用上面**第三节**中一个定时器 `Timer` 组件，在该组件中需要不断触发计时器来达到更新UI的效果，理想情况下，我们应该只编码一次来达到这个效果，state 就是干这个的，state 和 props 类型，只不过 state 是私有的，并且完全受控于当前组件。

## 利用生命周期和state改造定时器 `Timer`
```javascript
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }

  componentWillUnmount() {
    clearInterval(this.timerID);
  }

  tick() {
    this.setState({
      date: new Date()
    });
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```
该代码发生了什么？

1. 当执行到 `ReactDom.render()` 时，`Clock` 组件被传入，构造函数 construct 被触发，state 初始化。
2. `Clock` 组件调用 `render()` 方法，React 更新 Dom。
3. 当 `Clock` 输出的内容被插入 Dom 之后，会触发 `ComponentDidMount()` 生命周期，计时器被建立，每秒都会调用 `tick()` 方法。
4. `tick()` 方法被调用，state 发生改变，`render()` 重新触发，React 重新更新 Dom。
5. 当 `Clock` 组件从 Dom 中移除时会触发 `componentWillUnmount()` 生命周期，计时器被移除。
## 正确使用State
### 不要直接修改State
直接修改的 state 不会触发 `render()`，视图不会更新。例如下面：
```javascript
// wrong
this.state.name = 'lisi';
```
应该调用 `setState()` 方法。
```javascript
// correct
this.setState({
    name: 'lisi'
});
```
构造函数是唯一可以给 state 赋值的地方。
### State的更新是异步的
出于性能考虑，React 可能会把多个 `setState()` 合并成一个调用

所以 `this.state` 和 `this.props` 是异步更新的，所以不要依赖他们的值来更新下一个状态。如果要解决这个问题，请使用函数。例如下面是一个错误示例和正确示例：
```javascript
// wrong
this.setState({
    user: this.state.id + this.props.name,
});
```
```javascript
// correct
this.setState((state, props)=> ({
    user: state.id + props.name,
}));
```
### State的更新会被合并
例如下面有两个 state :
```javascript
construct(props) {
    super(props)
    this.state = {
        name: 'lisi',
        age: 20
    }
}
```
可以分别更新这两个状态
```javascript
this.setState({name});
this.setState({age});
```
这是一种浅合并，更新 name 时会完整保留 age, 完全替换 name。更新 age 时同理。

## 数据向下流动
不管是父组件还是子组件，都无法知道某个组件是有状态组件还是无状态组件，因为 state 是私有的，当父组件向子组件传值时，该子组件并不知道这个 props 对象是来自父组件的 state 还是 props，这种现象称为数据的向下流动，也叫单向数据流。任何从某个组件中衍生出来的 state 只能影响“低于”它们的组件。

# 六、事件处理
## 与传统 DOM 语法不同
1. React 事件命名采用驼峰式，而不是纯小写
2. React 事件绑定采用传入函数，而不是一个字符串，例如：
```javascript
// 传统 HTML
<button onClick="clickMe()">
    click me
</button>
```
```javascript
// React
<button onClick={clickMe}>
    click me
</button>
```
3. 传统 HTML 可以通过返回 false 来阻止默认行为，在 React 必须显式使用`preventDefault` 例如：
```javascript
// 传统 HTML
<a href="#" onClick="return false">click me</a>
```
```javascript
// React
const clickMe = (e) => {
    e.preventDefault();
    console.log('i am clicked');
}
<a href="#" onClick={clickMe}>click me</a>
```

## class 中的 this
class 中的方法默认不会绑定this,假如你执行下面一段代码：
```javascript
class MyThis extends React.component {
    construct(props) {
        super(props)
        this.state = {
            name: 'zhangsan',
            age: 20,
        }
    }
    handleClick() {
        console.log(this.name) //undefined
    }
    render() {
        return (
            <button onClick={this.handleClick}>click test</button>
        )
    }
}

ReactDom.render(<MyThis />, document.getElementById('root'))
```
上面代码当点击按钮的时候会在控制台打印 undefined，要想解决这类问题，有三种方法。
1. 在构造函数中绑定 this, 例如：
```javascript
construct(props) {
    super(props)
    this.state = {
        name: 'zhangsan',
        age: 20,
    }
    this.handleClick = this.handleClick.bind(this)
}
```
2. 在定义函数时使用箭头函数, 例如：
```javascript
handleClick = () => {
    console.log(this.name) // zhangsan
}
```
3. 在绑定事件时采用箭头函数, 例如：
```javascript
<button onClick={() => this.handleClick()}>click test</button>
```
以上三种方法都是可以改变函数中的this指向，第一种基本没有局限性，第二种需要你使用实验性的 public class fields 语法，第三种在大多数情况下没什么问题，但如果该回调函数作为 prop 传入子组件时，这些组件可能会进行额外的重新渲染， **为什么？** React 更新元素时只更新需要的部分，当 diff 算法触发时会触发浅比较，如果在 render 中创建函数，会在每次渲染的时候都创建一个新的函数，这时浅比较会触发 false，即使你没有发生任何变化，子组件也会重新渲染。

## 传递参数
传递参数没什么好说的，来说说事件对象 e 吧。当你在事件绑定时采用箭头函数的形式，那么该显式地传递e, 当你用其他两种方法时，e 更多的会被当作后一个参数隐式传入，例如下面两种方法式等价的：
```javascript
<button onClick={(id, e) => this.delete(id, e)}>click</button>
<button onClick={this.delete.bind(this, id)}>click</button>
```
# 七、条件渲染
## 运算符 if
```javascript
class Condition extends React.component {
    state = {
        isAdmin: false
    }
    
    render() {
        const { isAdmin } = this.state
        let button;
        if (isAdmin) {
            button = (<button>true</button>)
        } else {
            button = (<button>false</button>)
        }
        return (
            <div>
                {button}
            </div>
        )
    }
}
ReactDom.render(<Condition />, document.getElementById('root'))
```

## 运算符 &&
利用上面的例子，我们的 `render()` 可以这么写
```javascript
render() {
    const { isAdmin } = this.state
    return (
        {
            isAdmin && 
            button = (<button>true</button>)
        }
        {
            !isAdmin &&
            button = (<button>false</button>)
        }
    )
}
```
之所以能这么写是因为假如 && 左侧的元素是 true，那么会返回右侧的元素，假如左侧元素是 false，那么会跳过右侧的元素，但是会返回 false 表达式，所以这里有一个坑。
```javascript
render() {
    const count = 0
    return (
        <div>count && <h1>count: {count}</h1></div>
    )
}
```
上面的表达式会返回 `<div>0</div>`

## 三目运算符
```javascript
render() {
    const { isAdmin } = this.state
    return (
        {
            isAdmin? 
            <button>true</button>
            :
            <button>false</button>
        }
    )
}
```

## 阻止组件渲染
例如现在有一个组件 `children`, 它被两个父组件调用，其中一个父组件中需要展示，另一个父组件中需要隐藏，我们可以通过返回 null的形式来隐藏该组件，而且这并不影响父组件的生命周期
```javascript
function Children(props) {
    if(!props.isAdmin) {
        return null;
    }
    return (
        <button>true</button>
    );
}
```

# 八、列表 && key
## map()
我们可以通过 `map()` 方法来遍历数组并将数组中每个元素都渲染成标签，例如下面:
```javascript
const arr = [1, 2, 3, 4, 5];
const listItem = arr.map(item => (
    <li>{item}</li>
));

ReactDom.render(<ul>{listItem}</ul>, document.getElementById('root'));
```
这会产生一个 1 到 5 的列表，但是在控制台会发现一个警告 `a key should be provided for list items` ，这是因为你没有指定 key 的值。

## key
1. key 可以帮助 React 高效识别哪些元素发生了改变，哪些元素没有发生改变。所以，key最好是一个独一无二的值，比如 id 。万不得已时，可以使用 index ，但是并不推荐，因为当你的数组中某些元素的顺序发生变化时， index 并没有发生变化，这不会导致重新渲染。
2. key 设定的位置也是比较讲究的，只有放在就近数组的上下文才会生效，一般都是在 `map()` 方法中指定。
3. 当 key 绑定在一个组件上时，子组件 props 对象中并没有 key 属性，所以最好不要用 key 来传递一些状态。
4. key 只是在兄弟节点之间唯一，并不是全局唯一，所以不同数组可以使用相同的 key。

# 表单
## 非受控组件
与传统的 HTML 标签不同，`form` 表单会自己维护一个状态。
```javascript
<form>
  <label>
    名字:
    <input type="text" name="name" />
  </label>
  <input type="submit" value="提交" />
</form>
```
例如这段代码会触发表单默认事件，即提交后跳转到新的页面，如果你需要做一些特殊操作，请使用**受控组件**。

## 受控组件
顾名思义，受控组件是“受控”的，就是让 React 内部的 state 来接管 form 表单，例如：
```javascript
class MyForm extends React.component {
    state = {
        myName: 'initValue',
        myContent: 'initContent',
        myCheck: false,
    }

    inputChange = (e) => {
        const target = e.target
        const value = target.type === 'checkbox'? target.checked : target.value
        const name = target.name
        this.setState({
            [name]: value,
        })
    }

    render() {
        const { myName, myContent, myCheck } = this.state
        return (
            <form>
              <label>
                名字:
                <input type="text" name="myName" value={myName} onChange={this.inputChange}/>
              </label>
              <input type="checkbox" name="myCheck" checked={myCheck} onChange={this.inputChange}/>
              <input type="text"  name="myContent" value={myContent} onChange={this.inputChange}/>
            </form>
        )
    }
}
```
上面代码中有三个关键点：
1. 用 `value` 或者 `checked` 接管表单标签的状态， 用`onChange`事件触发state 改变从而重新渲染页面。
2. 所有状态都来自组件内部的状态，state 成为唯一数据源，所以是受控组件。
3. 给不同的标签绑定name，使用一个函数就可以管理多种标签。这里用到了 ES6 的计算属性名称语法。
## 受控组件输入空值
在**受控组件**上使用 value 会阻止用户输入，如果此时用户还能输入，可能该 value 值为 null 或 undefined,例如下面：
```javascript
ReactDOM.render(<input value="hi" />, mountNode);

setTimeout(function() {
  ReactDOM.render(<input value={null} />, mountNode);
}, 1000);
```
一开始用户是无法输入的，隔一段时间后可以进行输入。

# 九、状态提升
状态提升的具体操作方法这里不做赘述，简单的说就是将子组件的 state 和操作 state 的方法提取到父组件中，然后利用自上而下的数据流，子组件可以从 props 对象中接收状态。
## 何时需要状态提升
当兄弟组件之间需要利用对方的状态，这时最好的办法就是将这个 state 提取到他们的公共父组件中，然后利用“自上而下数据流”注入子组件中，也就是单向数据流。

## 为什么不用双向绑定，
React 的单向数据流会造成代码量增多，但是也会带来一些好处，因为数据流是单向的，在遇到问题解决 bug 的时候，bug 的排查范围被大大减少了。

## 弊端
除了代码量增多的缺点外，当你的应用不断变大时，你会发现如果始终采用自上而下的数据流，状态管理会被变得非常复杂并且不易维护，但还好，mobx，redux的出现缓解了这种尴尬，他们会把需要共享的状态存放在 store 中，然后分别注入到需要的组件。

# 十、组合 VS 继承
React 拥有非常强大的组合模式，推荐使用组合而非继承。
## 包含关系
有些组件并不能提前知道子组件的具体内容。我们可以利用 `children` prop来将子组件的内容渲染到父组件中
```javascript
function MyBox(props) {
    return (
        <div className={'myBox' + props.color}>
            {props.children}
        </div>
    )
}

function WelcomeDialog() {
    return(
        <MyBox color="red">
            <div>
                <h1>Welcome !</h1>
                <p>i am welcome dialog</p>
            </div>
        </MyBox>
    )
}
```
`<MyBox></MyBox>` 标签中内容将会作为 `props.children` 传递给自身组件。但是有时候 `children` 并不管用，例如下面：
```javascript
function MyBox(props) {
    return (
        <div className="myBox">
            <div className="header">
                {props.top}
            </div>
            {props.children}
            <div className="footer">
                {props.bottom}
            </div>
        </div>
    )
}

function WelcomeDialog() {
    return(
        <MyBox top={<Top />} bottom={<Bottom />}>
            <div>
                <h1>Welcome !</h1>
                <p>i am welcome dialog</p>
            </div>
        </MyBox>
    )
}
```
因为像 `<Top />` 这样的组件本身就是 React 元素，而 React 元素本质就是一个对象，所以可以把他们当作 props 用作传递。

## 特例关系
比如现在有一个 `Dialog` 组件, 而 `WelcomeDialog` 只是它一种特殊的表现形式。这也可以用组合实现，例如下面：
```javascript
function Dialog(props) {
    return (
        <div>
            <h1>{props.title}</h1>
            <p>{props.message}</p>
        </div>
    )
}

function WelcomeDialog() {
    return (
        <Dialog 
            title="Welcome !"
            message="i am welcome dialog" />
    )
}
```

## 继承去哪了
目前来看，组合能满足所有场景需求，并不需要继承。当你要复用非 UI 的功能时，也可以创建一个单独 js 模块并用 export 暴露，并在需要的组件中用 import 引入。

# 十一、React 哲学
这一块我认为更多需要实践吧，可以形成自己的构建思维，React 官方建议的思路如下：
1. 将设计好的 UI 划分为组件层级
2. 用 React 创建一个静态版本
3. 确定 UI state 的最小（且完整）表示
4. 确定 state 放置的位置
5. 添加反向数据流

# 总结
到此，React 核心概念已经结束了。回顾一些重要的概念：
1. 最好使用JSX
2. 元素渲染只更新需要更新的部分
3. 组件根据复杂性和复用性进行拆分组合，props是只读的
4. state 是组件私有的，组件具有生命周期函数
5. 事件处理需要绑定this,或者使用箭头函数
6. 三种条件渲染的方式：if运算符，&&运算符，三目运算符
7. 使用 `map()` 需要绑定 key
8. 受控组件利用组件自身的状态，非受控组件利用标签自带的状态
9. 状态提升的方法和时机
10. 拥抱组合抛弃继承 
11. 五步走构建 React 应用