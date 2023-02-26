# Redux

# 安装

​	` npm install redux --save`

# redux原理

![image-20210716093942389](https://images-1300732204.cos.ap-chengdu.myqcloud.com/image-20210716093942389.png)

# 项目构建

`项目文件`

```file
src
 + redux
  + store.js
  + xxx_reducer.js
  + xxx_action_creator.js
  + constant.js
```

tips: 在需要使用redux进行状态管理的组件中,将管理的状态从组件中删去保留不需要redux管理的状态并import`store.js`通过调用`store.getState()`获取到状态,`store.dispatch(action)`更新redux状态,再监听redux更新的状态调用`store.subscribe(()=>{this.render()})`触发react的render函数执行更新,reducer的本质是函数,有多个reduer命名可以是`name_reducer.js`

`src/redux/store.js`

```js
import { createStore } from 'redux'
import todolistReduce from './todolis_treducer'
/* eslint-disable no-underscore-dangle */
const store = createStore(
    todolistReduce,
    //使用浏览器的插件redux
    window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__()
);
/* eslint-enable */
export default store 
```

`src/redux/todolist_reducer.js`

```js
import { ADDNEWITEM, DELETEITEM } from './constant'
const defaultState = {
    list: [{
        content: "todo something...",
        done: false,
        time: '2021/7/15',
        id: "1"
    }]
}
const todolistReducer = (previousState = defaultState, action) => {
    console.log("reducer", previousState, action);
    const { type, data } = action
    switch (type) {
        case ADDNEWITEM:
            //此处更新状态
            break;
        case DELETEITEM:
            break;
        default:
            return previousState;
    }

}
export default todolistReducer
```
`src/redux/constant.js`

```js
//用于定义action对象中type类型常量值
export const ADDNEWITEM = "ADDNEWITEM"
export const DELETEITEM = "DELETEITEM"
```
`src/redux/todolist_action_creator.js`

```js
//为组件生成action对象
import { ADDNEWITEM, DELETEITEM } from './constant'
const createAddNewItem = data => ({ type: ADDNEWITEM, data })
const deleteItem = data => ({ type: DELETEITEM, data })

// eslint-disable-next-line import/no-anonymous-default-export
export default {
    createAddNewItem,
    deleteItem
}
```

```js
//是同步action还是异步action
//同步action返回是Object异步action返回是Function
//使用中间件 redux-thunk
//在store.js中引入 import thunk from 'redux-thunk' 和import {applymiddleware} from 'redux'且需要更改store.createStore(reducers,applymiddleware(thunk))
//至此action返回函数store执行
//列如异步action.js有如下代码
const action = {
    type: '',
    data: '',
}
export const asyncFunc = ()=>{
    return (dispatch)=>{
        setTimeOut({
             dispatch(action) //dispatch是store传来的等于store.dispatch(action)
        },500)//异步请求可以发送ajax
    }
}
```

`src/index.js`

```js
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App.jsx';
import reportWebVitals from './reportWebVitals';
import store from './redux/store'
//在此处订阅,只要redux状态改变则更新,不需要在每一个组件的componentDidMount中手动调用render
store.subscribe(() => {
  ReactDOM.render(
    <App />,
    document.getElementById('root')
  );
})
ReactDOM.render(
    <App />,
    document.getElementById('root')
  );
```

`src/components/Todolist.jsx`

```js
import React, { PureComponent } from "react";
import { Button, Input } from "antd";
import "./index.css";
import "antd/dist/antd.css";
import store from "../../redux/store.js";
import todolistActionCreator from '../../redux/todolist_action_creator.js'
export default class index extends PureComponent {
  constructor(props) {
    super(props);
    this.state = store.getState();
    this.changeInput = this.changeInput.bind(this)
  }

  changeInput = (e)=>{
      store.dispatch(todolistActionCreator.createAddNewItem(e.currentTarget.value))
  }
  render() {
    const { list } = this.state;
    console.log(list);
    return (
      <div className="todolist">
        <div className="title">Add New Thing</div>
        <div className="input">
          <div className="header">
            <Input
              placeholder="输入你想做的事"
              style={{ width: "70%", marginRight: "15%" }}
              onChange={this.changeInput}
              />
            <Button>添加</Button>
          </div>
 			<div className="content">
          	<List id={list[0].id} time={list[0].time} content={list[0].content} />
        	</div>
        </div>
      </div>
    );
  }
}
```



# 参考文献

[阮一峰的redux](https://www.ruanyifeng.com/blog/2016/09/redux_tutorial_part_one_basic_usages.html)

# react-redux

​	视频:[https://www.bilibili.com/video/BV1wy4y1D7JT?p=104&spm_id_from=pageDriver](https://www.bilibili.com/video/BV1wy4y1D7JT?p=104&spm_id_from=pageDriver)

# 原理图

![image-20210716160715710](https://images-1300732204.cos.ap-chengdu.myqcloud.com/image-20210716160715710.png)

# 安装库

​	`npm i react-redux  --save `

# 项目文件

```file
src
 + components ui组件
 + containers 用于包裹ui组件
```

tips: 页面渲染时使用的是`Container`组件而不是`UI`组件

`src/index.js`

```js
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App.jsx';
import reportWebVitals from './reportWebVitals';
import store from './redux/store'
import { Provider } from 'react-redux'
//Provider能够将store精准的注入到每一个需要使用store的容器组件,避免了app.jsx中多个容器组件需要store从而反复写store={store}
//使用react-redux后不需要在此处订阅,只要redux状态改变则更新,不需要在每一个组件的componentDidMount中手动调用render和正常一样
//在使用connect使用的时候react就会检测redux状态改变
ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
);
```

`src/app.jsx`

```js
import Todolist from './Container/Todolist'
import store from './redux/store';
import './App.css';

function App() {
  return (
    <div className="App">
      //使用Provider后不需要写store={store}只需要写组件名称即可
      <Todolist store={store}/>
    </div>
  );
}

export default App;
```

`src/Containers/Todolist/index.jsx`

```js
//引入todolist的ui组件
import TodolistUI from '../../Components/Todolist'
//连接ui和redux
import {connect} from 'react-redux'
import todolistActionCreator from '../../redux/todolist_action_creator'
//连接容器组件和ui ,要向子组件传递状态和操作状态的函数,connect的第一个参数必须是两个函数
//mapStateToProps函数的返回值作为状态传递给了UI组件,mapStateToProps函数返回一个对象,对象的属性就作为了子组件的props
//mapDispatchToProps函数的返回值作为操作状态的方法和函数传递给了ui组件,方法也作为了ui组件的属性props
//react在调用mapStateToProps函数时传递参数为state = store.getState()
//react的ui触发状态更新时传递参数dispatch = store.dispatch()
const mapStateToProps = (state)=>{
    return state
}
const mapDispatchToProps = dispatch=> (
  {
    addNewItem: (value) => {
      //自己传递参数action
      dispatch(todolistActionCreator.createAddNewItem(value));
    }, //func在此处是作为对象的属性,调用mapDispatchToProps后将会成为ui组件的属性
  }
)
const todolistContainer = connect(mapStateToProps,mapDispatchToProps)(TodolistUI)
export default todolistContainer
```

`src/Components/Todolist/index.js`

```js
import React, { PureComponent } from "react";
import { Button, Input } from "antd";
import List from "../List";
import "./index.css";
import "antd/dist/antd.css";
export default class index extends PureComponent {
  constructor(props) {
    super(props);
    this.changeInput = this.changeInput.bind(this)
  }

  changeInput = (e)=>{
      this.props.addNewItem(e.currentTarget.value)
  }
  render() {
    const { list } = this.props;
    console.log("todolist props",this.props,list);
    return (
      <div className="todolist">
        <div className="title">Add New Thing</div>
        <div className="input">
          <div className="header">
            <Input
              placeholder="输入你想做的事"
              style={{ width: "70%", marginRight: "15%" }}
              onChange={this.changeInput}
              />
            <Button>添加</Button>
          </div>
        </div>
        <div className="content">
          <List id={list[0].id} time={list[0].time} content={list[0].content} />
        </div>
      </div>
    );
  }
}

```

```js
//优化后的src/Components/Todolist/index.js
import TodolistUI from "../../Components/Todolist";
import { connect } from "react-redux";
import todolistActionCreator from "../../redux/todolist_action_creator";

export default connect(
  state => {
    return state;
  },
  /*一般写法
  dispatch => ({
    addNewItem: (value) => {
      dispatch(todolistActionCreator.createAddNewItem(value));
    },
  })
  */
 //精简版写法:dispatch和参数都可以省略只剩下action对象的调用
 {
    addNewItem:todolistActionCreator.createAddNewItem,
 }
)(TodolistUI);

```

`整合UI和容器组件:将Container文件移除并修改Components下的ui组件`

```js
import React, { PureComponent } from "react";
import { Button, Input } from "antd";
import List from "../List";
import "./index.css";
import "antd/dist/antd.css";
import { connect } from "react-redux";
import todolistActionCreator from "../../redux/todolist_action_creator";

class TodolistUI extends PureComponent {
  constructor(props) {
    super(props);
    this.changeInput = this.changeInput.bind(this);
  }

  changeInput = (e) => {
    this.props.addNewItem(e.currentTarget.value);
  };
  render() {
    const { list } = this.props;
    console.log("todolist props", this.props, list);
    return (
      <div className="todolist">
        <div className="title">Add New Thing</div>
        <div className="input">
          <div className="header">
            <Input
              placeholder="输入你想做的事"
              style={{ width: "70%", marginRight: "15%" }}
              onChange={this.changeInput}
            />
            <Button>添加</Button>
          </div>
        </div>
        <div className="content">
          <List id={list[0].id} time={list[0].time} content={list[0].content} />
        </div>
      </div>
    );
  }
}

export default connect(
  (state) => {
    return state;
  },
  {
    addNewItem: todolistActionCreator.createAddNewItem,
  }
)(TodolistUI);
```

**src文件夹下存在`redux`文件夹及其相关操作同上面的redux里的文件一样**

```js
//存在多个action文件时将action文件放于actions文件夹下
//存在多个reducer时将reducer放置于reducers文件夹下
//多个不同actions操作的type常量都放置constant.js中再由不同的reducer.js引入
//多个reducer.js需要修改store.js的第一个参数并将多个reducer.js映入,如下
import { createStore ,applyMiddleware,combineReducers,compose} from 'redux'
import * as reduces from './todolist_reducer'
//thunk用于支持异步的action
import thunk from 'redux-thunk'
//combineReducers传递的参数是对象,这个对象就是redux管理的状态

export default createStore(
    combineReducers({reduces}),
    compose(applyMiddleware(thunk),window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__())
);
```

**reducer是纯函数,意味着发送请求应该在action中**

![image-20210716194756852](https://images-1300732204.cos.ap-chengdu.myqcloud.com/image-20210716194756852.png)

`使用index.js`汇总所有的reducer.js,像下面这样

![image-20210716201030238](https://images-1300732204.cos.ap-chengdu.myqcloud.com/image-20210716201030238.png)

`store.js`引入

![image-20210716200943140](https://images-1300732204.cos.ap-chengdu.myqcloud.com/image-20210716200943140.png)

# 优化

**当组件ui组件只剩下render函数时可以使用函数式组件代替函数式组件的性能高于class继承的组件性能**

![image-20210717164204903](https://images-1300732204.cos.ap-chengdu.myqcloud.com/image-20210717164204903.png)

