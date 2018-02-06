---
title: "React学习——4-4"
date: 2017-12-14 15:12:00
categories: 学习册
---

### 更近一步
> 处理异步、调试工具、更优雅的和react结合

<!-- more -->

> Redux处理异步，需要redux-thunk插件
Npm install redux-devtools-extension 并且开启
使用react-redux优雅的链接react和redux



#### 处理异步
> Redux默认只处理同步，异步任务需要react-thunk插件
Npm install redux-thunk --save
使用applyMiddleware开启thunk中间件
Action可以返回函数，使用dispatch提交action

``` Javascript

import React from 'react';
import ReactDom from 'react-dom';
import { createStore, applyMiddleware } from 'redux';
import thunk from 'redux-thunk';

import App from './App';
import { counter, addGun,removeGun, addGunAsync } from './index.redux';


const store = createStore(counter, applyMiddleware(thunk))

function render() {
  ReactDom.render(
    <App store={store} addGun={addGun} addGunAsync={addGunAsync} removeGun={removeGun} />,
    document.getElementById('root')
  )
}
render()
store.subscribe(render)


// 延迟添加，拖两天再给
export function addGunAsync() {
  // thunk插件的作用，这里可以返回函数
  return dispatch=>{
    setTimeout(()=>{
      // 异步结束后，手动执行dispatch
      dispatch(addGun())
    }, 2000)
  }
}


```
#### 调试工具
> Chrome 搜索redux 安装
新建store的时候判断 window.devToolsExtension
使用compose结合thunk和 window.devToolsExtension
调试窗口的redux选项卡，实时看到state

``` Javascript

import React from 'react';
import ReactDom from 'react-dom';
import { createStore, applyMiddleware, compose } from 'redux';
import thunk from 'redux-thunk';

import App from './App';
import { counter, addGun,removeGun, addGunAsync } from './index.redux';

const store = createStore(counter, compose(
  applyMiddleware(thunk),
  window.devToolsExtension ? window.devToolsExtension() : f=>f
))

```

#### 使用react-redux
> npm install react-redux --save
> 忘记subscribe，记住reducer，action和dispatch即可
> React-redux提供Provider和connect两个接口来链接

Provider组件在应用最外层，传入store即可，只用一次
Connect负责从外部获取组件需要的参数
Connect可以用装饰器的方式来写

``` Javascript

//App.js
import React from 'react';
import { connect } from 'react-redux';
import {addGun, removeGun, addGunAsync} from './index.redux';
class App extends React.Component{
  // constructor(props) {
  //   super(props);
  // }
  render(){
    return (
      <div>
        <h1>现在有机枪{this.props.num}把</h1>
        <button onClick={this.props.addGun}>申请武器</button>
        <button onClick={this.props.removeGun}>上交武器</button>
        <button onClick={this.props.addGunAsync}>拖两天给</button>
      </div>
    )
  }
}
const mapStatetoProps = (state) => {
  return {num:state}
}
const actionCreators = {addGun, removeGun, addGunAsync}
App = connect(mapStatetoProps, actionCreators)(App)
export default App;

```


``` Javascript

// index.js
import React from 'react';
import ReactDom from 'react-dom';
import { createStore, applyMiddleware, compose } from 'redux';
import thunk from 'redux-thunk';
import { Provider } from 'react-redux';

import App from './App';
import { counter } from './index.redux';

const store = createStore(counter, compose(
  applyMiddleware(thunk),
  window.devToolsExtension ? window.devToolsExtension() : f=>f
))


ReactDom.render(
  (<Provider store={store} >
    <App />
  </Provider>),
    document.getElementById('root')
)


```


#### 使用装饰器优化connect代码
> npm run ejct
> npm install babel-plugin-transform-decorators-legacy插件
> Package.json 里 babel加上plugins配置

``` Javascript

// package.json
  "babel": {
    "presets": [
      "react-app"
    ],
    "plugins": [
      "transform-decorators-legacy",
	]
  }
```

``` Javascript

// App.js
// const mapStatetoProps = (state) => {
//   return {num:state}
// }
// const actionCreators = {addGun, removeGun, addGunAsync}
// App = connect(mapStatetoProps, actionCreators)(App)

@connect(
  //你要state什么属性放到props里
  state=>({num:state}),
  //你要什么方法，放到props里，知道dispatch
  {addGun, removeGun, addGunAsync}
)

``` 



















