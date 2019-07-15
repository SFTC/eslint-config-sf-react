# SF-React/JSX 编码规范

结合 airbnb 的 react 书写规范, 在 eslint-plugin-react eslint-plugin-jsx-a11y 推荐用法上做了调整

## 主要规则
  1. [强制组件生命周期和事件处理函数书写顺序](#sort-comp-强制组件生命周期和事件处理函数书写顺序)
  2. [在同一个文件中只能声明一个组件](#no-multi-comp-在同一个文件中只能声明一个组件)
  3. [自定义组件强制使用驼峰](#jsx-pascal-case-自定义组件强制使用驼峰)
  4. [避免静态的类属性和组件生命周期拼写错误](#no-typos-避免静态的类属性和组件生命周期拼写错误)
  5. [styles 属性必须为对象](#style-prop-object-style-属性必须为对象)
  6. [强制使用 es6 的 class 创建组件](#prefer-es6-class-强制使用-es6-的-class-创建组件)
  7. [函数组件中禁止使用 this](#no-this-in-sfc-函数组件中禁止使用-this)
  8. [无子节点组件强制自闭合](#self-closing-comp-无子节点组件强制自闭合)
  9. [JSX 元素属性为布尔值时, true 可以省略](#jax-boolean-value-JSX-元素属性为布尔值时,-true-可以省略)
  10. [jsx props 风格缩进 2 空格](#jsx-indent-props-jsx-props-风格缩进-2-空格)
  11. [校验多行 jsx 元素的标签结束位置](#jsx-closing-bracket-location-校验多行-jsx-元素的标签结束位置)
  12. [校验有子节点 jsx 元素结束标签位置](#jsx-closing-tag-location-校验有子节点-jsx-元素结束标签位置)
  13. [强制校验保证 jsx 元素花括号内部的空格](#jsx-curly-spacing-强制校验保证-jsx-元素花括号内部的空格)
  14. [强制自闭合元素不能添加 children](#void-dom-elements-no-children-强制自闭合元素不能添加-children)
  15. [校验 jsx 元素属性名和 = 之前不得有空格](#jsx-equals-spacing-校验-jsx-元素属性名和-=-之前不得有空格)
  16. [禁止在事件处理函数中使用 bind 建议使用箭头函数](#jsx-no-bind-禁止在事件处理函数中使用-bind-建议使用箭头函数)
  17. [校验 jsx 元素单行上属性个数最大值](#jsx-max-props-per-line-校验-jsx-元素单行上属性个数最大值)
  18. [避免外部的 propTypes 使用](#forbid-foreign-prop-types-校验所有对象不使用-propTypes-属性)


## sort-comp 强制组件生命周期和事件处理函数书写顺序

### 按照以下顺序:
- 静态属性和方法
- 生命周期函数: 
    - displayName
    - propTypes
    - contextTypes
    - childContextTypes
    - mixins
    - statics
    - defaultProps
    - constructor
    - getDefaultProps
    - state
    - getInitialState
    - getChildContext
    - getDerivedStateFromProps
    - componentWillMount
    - UNSAFE_componentWillMount
    - componentDidMount
    - componentWillReceiveProps
    - UNSAFE_componentWillReceiveProps
    - shouldComponentUpdate
    - componentWillUpdate
    - UNSAFE_componentWillUpdate
    - getSnapshotBeforeUpdate
    - componentDidUpdate
    - componentDidCatch
    - componentWillUnmount
- 自定义方法
- render 方法

### bad

```jsx
    class Hello extends React.Component {
        static defaultProps = {}

        constructor(props) {
            super(props)
            this.state = {}
        }

        componentDidMount() {}

        static propTypes = {}

        renderSpan = () => (<span>I'm span</span>)

        handleClick = () => {}

        componentWillMount() {}

        render() {
            return (
                <div onClick={this.handleClick}>
                    {this.renderSpan()}
                </div>
            )
        }
    }
```

### good

```jsx
    class Hello extends React.Component {
        static propTypes = {}

        static defaultProps = {}

        constructor(props) {
            super(props)
            this.state = {}
        }

        componentWillMount() {}

        componentDidMount() {}

        handleClick = () => {}

        renderSpan = () => (<span>I'm span</span>)

        render() {
            return (
                <div onClick={this.handleClick}>
                    {this.renderSpan()}
                </div>
            )
        }
    }
```

## no-multi-comp 在同一个文件中只能声明一个组件

### bad

```js
    class Hello extends React.Component {
        render() {
            return <div>Hello {this.props.name}</div>;
        }
    }

    class HelloJohn extends React.Component {
        render() {
            return <Hello name="John" />;
        }
    }
```

### good

```js
    var Hello = require('./components/Hello');

    class HelloJohn extends React.Component {
        render() {
            return <Hello name="John" />;
        }
    }
```


## jsx-pascal-case 自定义组件强制使用驼峰

### bad

```js
    <Text_Component />
    <Mycomponent />
```

### good

```js
    <TextComponent />
    <MyComponent />
    <div />
```

## no-typos 避免静态的类属性和组件生命周期拼写错误

### bad

```js
    class MyComponent extends React.Component {
    static PropTypes = {}
    }

    class MyComponent extends React.Component {
    static proptypes = {}
    }

    class MyComponent extends React.Component {
    static ContextTypes = {}
    }

    class MyComponent extends React.Component {
    static contexttypes = {}
    }
```

### good

```js
    class MyComponent extends React.Component {
    static propTypes = {}
    }

    class MyComponent extends React.Component {
    static contextTypes = {}
    }

    class MyComponent extends React.Component {
    static childContextTypes = {}
    }

    class MyComponent extends React.Component {
    static defaultProps = {}
    }
```


## style-prop-object style 属性必须为对象

### bad
```js
    <div style="color: 'red'" />

    <div style={true} />

    <Hello style={true} />

    const styles = true;
    <div style={styles} />
```

### good

```js
    <div style={{ color: "red" }} />

    <Hello style={{ color: "red" }} />

    const styles = { color: "red" };
    <div style={styles} />
    React.createElement("div", { style: { color: 'red' }});

    React.createElement("Hello", { style: { color: 'red' }});

    const styles = { height: '100px' };
    React.createElement("div", { style: styles });
```

### prefer-es6-class 强制使用 es6 的 class 创建组件

### bad

```js
    var Hello = createReactClass({
    render: function() {
        return <div>Hello {this.props.name}</div>;
    }
    });
```

### good

```js
    class Hello extends React.Component {
    render() {
        return <div>Hello {this.props.name}</div>;
    }
    }
```

## no-this-in-sfc 函数组件中禁止使用 this

### bad

```js
    function Foo(props) {
    return (
        <div>{this.props.bar}</div>
    );
    }

    function Foo(props) {
    const { bar } = this.props;
    return (
        <div>{bar}</div>
    );
    }

    function Foo(props, context) {
    return (
        <div>
        {this.context.foo ? this.props.bar : ''}
        </div>
    );
    }

    function Foo(props, context) {
    const { foo } = this.context;
    const { bar } = this.props;
    return (
        <div>
        {foo ? bar : ''}
        </div>
    );
    }
```

### good

```js
    function Foo(props) {
    return (
        <div>{props.bar}</div>
    );
    }

    function Foo(props) {
    const { bar } = props;
    return (
        <div>{bar}</div>
    );
    }

    function Foo({ bar }) {
    return (
        <div>{bar}</div>
    );
    }

    function Foo(props, context) {
    return (
        <div>
        {context.foo ? props.bar : ''}
        </div>
    );
    }

    function Foo(props, context) {
    const { foo } = context;
    const { bar } = props;
    return (
        <div>
        {foo ? bar : ''}
        </div>
    );
    }
```

## self-closing-comp 无子节点组件强制自闭合

### bad

```js
    var HelloJohn = <Hello name="John"></Hello>;
```

### good

```js
    var contentContainer = <div className="content"></div>;

    var intentionalSpace = <div>{' '}</div>;

    var HelloJohn = <Hello name="John" />;

    var Profile = <Hello name="John"><img src="picture.png" /></Hello>;

    var HelloSpace = <Hello>{' '}</Hello>;
```

## jax-boolean-value JSX 元素属性为布尔值时, true 可以省略

### bad

```jsx

<Hello personal={true}/>

```

### good

```jsx

<Hello personal/>

```

### jsx-indent jsx 风格缩进 2 空格

### bad

```js

    <App>
    <Hello />
    </App>   

```

### good

```js
    <App>
        <Hello />
    </App>
```

### jsx-indent-props jsx props 风格缩进 2 空格

### bad

```js

    <Hello
      firstName="John"
       lastName="Doe"
    />   

```

### good

```js
    <Hello
      firstName="John"
      lastName="Doe"
    />
```

## jsx-closing-bracket-location 校验多行 jsx 元素的标签结束位置

### bad

```js
    <Hello
    lastName="Smith"
    firstName="John" />;

    <Hello
    lastName="Smith"
    firstName="John"
    />;
```

### good

```js
    <Hello firstName="John" lastName="Smith" />;

    <Hello
    firstName="John"
    lastName="Smith"
    />;
```

## jsx-closing-tag-location 校验有子节点 jsx 元素结束标签位置

### bad

```js
    <Hello>
        marklar
        </Hello>
    <Hello>
    marklar</Hello>
```

### good

```js
    <Hello>
        marklar
    </Hello>

    <Hello>marklar</Hello>
```

## jsx-curly-spacing 强制校验保证 jsx 元素花括号内部的空格

### bad

```js
    <Hello name={firstname} />;
    <Hello name={ firstname} />;
    <Hello name={firstname } />;
    <Hello>{firstname}</Hello>;
```

### good

```js
    <Hello name={ firstname } />;
    <Hello name={ {firstname: 'John', lastname: 'Doe'} } />;
    <Hello name={
    firstname
    } />;
    <Hello>{ firstname }</Hello>;
    <Hello>{
    firstname
    }</Hello>;
```

## void-dom-elements-no-children 强制自闭合元素不能添加 children

### bad

```js
    <br>Children</br>
    <br children='Children' />
    <br dangerouslySetInnerHTML={{ __html: 'HTML' }} />
    React.createElement('br', undefined, 'Children')
    React.createElement('br', { children: 'Children' })
    React.createElement('br', { dangerouslySetInnerHTML: { __html: 'HTML' }})
```

### good

```js
    <div>Children</div>
    <div children='Children' />
    <div dangerouslySetInnerHTML={{ __html: 'HTML' }} />
    React.createElement('div', undefined, 'Children')
    React.createElement('div', { children: 'Children' })
    React.createElement('div', { dangerouslySetInnerHTML: { __html: 'HTML' }})
```

## jsx-equals-spacing 校验 jsx 元素属性名和 = 之前不得有空格

### bad

```js
    <Hello name = { firstname } />;
    <Hello name ={ firstname } />;
    <Hello name= { firstname } />;
```

### good

```js
    <Hello name={ firstname } />;
    <Hello name />;
    <Hello { ...props } />;
```

## jsx-no-bind 禁止在事件处理函数中使用 bind 建议使用箭头函数

### bad

```js
    <Foo onClick={this._handleClick.bind(this)}></Foo>
```

### good

```js
    <Foo onClick={() => console.log('Hello!')}></Foo>
```

## jsx-max-props-per-line 校验 jsx 元素单行上属性个数最大值

### bad

```js
    <Hello firstName="John" secondName="Mike" lastName="Smith" />
```

### good

```js
    <Hello 
        firstName="John" 
        secondName="Mike" 
        lastName="Smith" 
    />
```

## forbid-foreign-prop-types 校验所有对象不使用 propTypes 属性

### bad

```js
    import SomeComponent from './SomeComponent';
    SomeComponent.propTypes;
    var { propTypes } = SomeComponent;
    SomeComponent['propTypes'];
```

### good

```js
    import SomeComponent, {propTypes as someComponentPropTypes} from './SomeComponent';
```


## 修改规则配置

*由于 eslint-plugin-jsx-a11y 推荐用法上大多为 error 所以结合业需求修改一些配置关闭或 warn*

- click-events-have-key-events 强制执行 onClick 必须伴随一下几个事件 onKeyUp, onKeyDown, onKeyPress 之一, 为了兼顾残障用户的阅读器阅读 <font color=red>**off**</font>
- nteractive-supports-focus 可交互元素必须是可聚焦的, 需要添加 tabIndex 才能通过规则 <font color=red>**off**</font>
- no-interactive-element-to-noninteractive-role 不能通过 role 属性将交互元素转换为非交互元素 <font color=yellow>**warn**</font>
- media-has-caption 媒体元素必须具有添加字幕, 来满足残障用户提高可访问性 <font color=red>**off**</font>
- mouse-events-have-key-events 强制 onmouseover/onmousemove 伴随 onBlur/onFocus 事件, 为了满足无法使用鼠标情况 <font color=red>**off**</font>
- no-noninteractive-element-interactions 非交互元素不应添加事件处理函数 <font color=red>**off**</font>
- no-noninteractive-element-to-interactive-role 不能通过 role 属性将非交互元素转换为交互元素 <font color=yellow>**warn**</font>
- no-onchange 在选择性菜单上强制使用 onBlur 为了仅限屏幕和键盘阅读的用户 <font color=red>**off**</font>
- no-static-element-interactions 在对静态无语义元素添加事件处理函数时应当通过 role 赋予对应的角色值 <font color=red>**off**</font>
- no-access-key 强制不使用键盘快捷键 <font color=red>**off**</font>
- anchor-has-content a 标签必须要有内容 <font color=yellow>**warn**</font>


