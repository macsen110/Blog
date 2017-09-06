1. props 可以认为是组件中不可变的属性,不要写入修改之类
2. getDefaultProps: function() { return { value: 'default value' }; }
The result of getDefaultProps() will be cached and used to ensure that this.props.valuewill have a value if it was not specified by the parent component. This allows you to safely just use your props without having to write repetitive and fragile code to handle that yourself.

有点类似于直接在每个组件中写入的属性,propTypes 大多验证

Component Specs and Lifecycle

render
1. 创建的类里面以render()渲染组件的方法
2. getInitialState
Invoked once before the component is mounted. The return value will be used as the initial value of this.state.

3. getDefaultProps
Invoked once and cached when the class is created. Values in the mapping will be set onthis.props if that prop is not specified by the parent component (i.e. using an in check).
This method is invoked before any instances are created and thus cannot rely onthis.props. In addition, be aware that any complex objects returned by getDefaultProps()will be shared across instances, not copied.

4. propTypes
The propTypes object allows you to validate props being passed to your components.

5. mixins
The mixins array allows you to use mixins to share behavior among multiple components.

6. statics
The statics object allows you to define static methods that can be called on the component class.

Lifecycle Methods

1.Mounting: componentWillMount
Invoked once, both on the client and server, immediately before the initial rendering occurs. If you call setState within this method, render() will see the updated state and will be executed only once despite the state change.

2. Mounting:componentDidMount
Invoked once, only on the client (not on the server), immediately after the initial rendering occurs. At this point in the lifecycle, the component has a DOM representation which you can access via React.findDOMNode(this).

If you want to integrate with other JavaScript frameworks, set timers using setTimeout orsetInterval, or send AJAX requests, perform those operations in this method.

3. Updating:componentWillReceiveProps

4. Updating:shouldComponentUpdate
shouldComponentUpdate(object nextProps, object nextState)
Invoked before rendering when new props or state are being received. This method is not called for the initial render or when forceUpdate is used.
Use this as an opportunity to return false when you're certain that the transition to the new props and state will not require a component update.

5. Updating:componentWillUpdate
componentWillUpdate(object nextProps, object nextState)
Invoked immediately before rendering when new props or state are being received. This method is not called for the initial render.

6. Updating:componentDidUpdate
componentDidUpdate(object prevProps, object prevState)

Invoked immediately after the component's updates are flushed to the DOM. This method is not called for the initial render.
Use this as an opportunity to operate on the DOM when the component has been updated.

7. Unmounting: componentWillUnmount
Invoked immediately before a component is unmounted from the DOM.
Perform any necessary cleanup in this method, such as invalidating timers or cleaning up any DOM elements that were created in componentDidMount.

生命周期执行顺序
getDefaultProps ->getInitialState ->componentWillMount ->componentDidMount

组件命名首字母最好大写

react key
When React reconciles the keyed children, it will ensure that any child with key will be reordered (instead of clobbered) or destroyed (instead of reused).
重新排序(而非重创)或摧毁了(而不是重用)。

minix
Uncaught Error: Invariant Violation: ReactClass: You're attempting to use a component class as a mixin. Instead, just use a regular object.

Component API

setState(function|object nextState[, function callback])

Merges nextState with the current state. This is the primary method you use to trigger UI updates from event handlers and server request callbacks.
The first argument can be an object (containing zero or more keys to update) or a function (of state and props) that returns an object containing keys to update.

setState(function(previousState, currentProps) { return {myInteger: previousState.myInteger + 1};});

组件中,对比不同的算法是昂贵的,使用key可以很明确的指出相同或不同的组件来修改,可用来映射state

props多数用来渲染不变的节点,数据

The stateful component encapsulates all of the interaction logic, while the stateless components take care of rendering data in a declarative way.
有状态组件封装了所有的交互逻辑,而无状态组件声明的方式呈现数据。只有stateChange时才会触发render ,

State should contain data that a component's event handlers may change to trigger a UI update. I
状态应该包含数据组件的事件处理程序可能会改变触发一个UI更新。
When building a stateful component, think about the minimal possible representation of its state, and only store those properties in this.state.
在构建一个有状态的组件,思考其状态的最小可能表示,只有在this.state存储这些属性。

The situation gets more complicated when the children are shuffled around (as in search results) or if new components are added onto the front of the list (as in streams).

形势变得更加复杂,当子节点散列排布(如搜索结果)或者新组件被添加到列表的前面(如流)。

use immutable
React makes use of a virtual DOM, which is a descriptor of a DOM subtree rendered in the browser. This parallel representation allows React to avoid creating DOM nodes and accessing existing ones, which is slower than operations on JavaScript objects.

ReactDOM.unmountComponentAtNode 会unmout掉React 在文档中注册的dom 节点,在其中的每个组件都会执行到componentWillUnmount，

Redux

API Reference

createStore(reducer, [initialState])

     return (Store):
An object that holds the complete state of your app. The only way to change its state is bydispatching actions. You may also subscribe to the changes to its state to update the UI.

Store

A store holds the whole state tree of your application.
The only way to change the state inside it is to dispatch an action on it.

A store is not a class. It’s just an object with a few methods on it.
To create it, pass your root reducing function to createStore.

- getState()
- dispatch(action)
- subscribe(listener)
- getReducer()
- replaceReducer(nextReducer)

applyMiddleware(...middlewares)

Middleware is the suggested way to extend Redux with custom functionality. Middleware lets you wrap the store’s dispatch method for fun and profit. The key feature of middleware is that it is composable.

For example, redux-thunk lets the action creators invert control by dispatching functions. They would receive dispatch as an argument and may call it asynchronously. Such functions are called thunks. Another example of middleware is redux-promise. It lets you dispatch a Promise async action, and dispatches a normal action when the Promise resolves.

     Returns

(Function) A store enhancer that applies the given middleware. The store enhancer is a function that needs to be applied to createStore. It will return a different createStore which has the middleware enabled.

bindActionCreators(actionCreators, dispatch)

     Returns

(Function or Object): An object mimicking the original object, but with each function immediately dispatching the action returned by the corresponding action creator. If you passed a function asactionCreators, the return value will also be a single function.

整个的流程

创建actions

创建reducer (对应flux里的注册事件) ---> 聚合reducers(combineReducers得到allReducers)

配置store createStore(也通常利用applyMiddleware处理相关异步之类的actions,),参数为allReducers和initState

在container目录下每个模板文件绑定actions

function mapStateToProps(state) {
  return {
    counter: state.counter
  };
}

function mapDispatchToProps(dispatch) {
  return bindActionCreators(CounterActions, dispatch);
}

export default connect(mapStateToProps, mapDispatchToProps)(Counter);

最后, container下有一个根目录文件 root 将Store从顶部注入

const store = configureStore();

export default class Root extends Component {
  render() {
    return (
      <Provider store={store}>
        <CounterApp />
      </Provider>
    );
  }
}

入口文件 index.js 渲染 Root组件

所以 整个的目录类似于

>
![react目录](https://github.com/macsen110/Blog/image/redux.png)
>