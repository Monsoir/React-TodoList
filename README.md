# Todo List

使用了

- react
- react-redux

可运行 Demo 地址：[🔗🔗🔗](https://monsoir.github.io/React-TodoList/)

样式采用 CSS in JS 的方式编写，本来写的是 React Native, 习惯了 CSS in JS 的样式编写方法；同时，CSS in JS 也很好地避免了 CSS 文件混乱，样式应用时效果乱窜。

## 关于 Redux 的进一步认识

### 从 store 中获取 state

当我们使用了 `combineReducers` 来将多个 Reducer 合并时，需要注意，当我们想从 store 中拿到 state 时，我们首先需要拿到某个 reducer, 再通过这个 reducer 去拿到 state

### state 的初始化

当我需要需要一个初始化的 state 时，我陷入了疑惑，这个 state 的初始状态究竟需要从哪里定义？

- 定义 reducer 方法时赋予一个默认值？
- 创建 store 时同时赋值？

[👉 问题的答案](https://stackoverflow.com/questions/33749759/read-stores-initial-state-in-redux-reducer/33791942#33791942)

简单来说：

1. state 的初始状态是需要在 reducer 函数定义时赋予一个默认值的
2. 同时，可以在创建 store 时，通过赋予 state 值来覆盖之前的默认值

这样的作用是：

- 第一步是使得 app 能够在一开始的时候顺利运行，而不致于因为数据问题而产生异常
- 第二步的作用是，使得我们在 app 的初始化工作后，还有机会对初始数据进行修改，例如
    - 本 app 中，从 localStorage 中获取之前存储的 todo
    - 另外，也可以在这里进行网络请求，将获得的数据覆盖刚才的初始化数据

## 编写中间件

### 存储中间件

对 Todo 的保存使用了 `localStorage` 保存在浏览器中，这里编写了一个中间件 `src/dataControl/myMiddleware/storager.js` 来进行处理

中间件实质上就是一个函数，一个稍微复杂了一点的函数，这个函数函数有 3 个参数可供使用:

- store 用来获取整个 state 树，并从中获取到需要的数据 `store.getState()`
- next 用于调用下一个中间件
- action 使用 redux 时，调遣的 action, 可根据 `action.type` 对 action 类型进行识别，进行特殊操作

> 这 3 个参数并不是像平常的函数定义那样定义，而是通过返回了一个又一个的函数来实现的，类似 Python 中的修饰器

在最后，需要调用 `next(action)` 来将操作流转到下一个中间件

## 组件封装

### 文本节点的传承

有时，我们需要实现一个自定义的控件，或者对原生的控件进行一些进一步的控制，但是，希望这个控件的文本节点能够继续传下去，而这个文本节点，就是 `props.children`, 有点奇怪。

见 `src/utils/UI/TouchableOpacity.js`

## 事件监听

### 窗口大小变化监听

在 `src/MainContainer.js` 中，对窗口大小进行了监听，硬写事件字符串 `resize`

```js
componentDidMount() {
    window.addEventListener('resize', this.updateTodosHeight);
}

componentWillUnmount() {
    window.removeEventListener('resize', this.updateTodosHeight);
}
```

## CSS

### 对 div 设置滚动

有时，需要将一个 div 的内容局限在一块大小固定的区域，但这个 div 中的内容比较多，如，一个列表，这时，我们就需要一个内嵌的可滚动的 div

这种需求，可以通过设置 css 属性 `{overflowX: 'scroll';}`, `{overflowY: 'scroll';}`, 还有设置 `height` 来实现

## ESLint 报错处理

- 不能在非 `.jsx`  文件中使用 JSX

```js
"rules": {
    // `.jsx` extension cannot be used with React Native
    // https://github.com/airbnb/javascript/issues/982
    "react/jsx-filename-extension": ["error", { "extensions": [".js", ".jsx"] }]
},
```

- unexpected block statement surrounding arrow body jsx

```js
"rules": {
    "arrow-body-style": ["error", "always"]
},
```



