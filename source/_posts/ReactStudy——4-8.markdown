---
title: "React学习——4-8"
date: 2017-12-16 13:28:00
categories: 学习册
---

### React-router4基础知识
> React-router4是什么
> React-router4核心概念
> React-router4实战

<!-- more -->

#### React-router4是什么
> React官方推荐路由库，4是最新版本
> 4是全新的版本，和之前版本不兼容，浏览器和RN均兼容
> React开发单页面应用必备，践行路由即组件的概念
> 核心概念：动态路由、Router、Link、Switch

##### 一个简单的栗子
> npm install react-router-dom --save
> Router4使用react-router-dom作为浏览器端的路由
> 忘记Router2的内容，拥抱最新Router4

##### 入门组件
> BrowserRouter，包裹整个应用
> Router 路由对应用渲染的组件，可嵌套
> Link 跳转专用

``` Javascript

class Test extends React.Component{
  render(){
    console.log(this.props)
    return <h2>测试组件{this.props.match.params.location}</h2>
  }
}
ReactDom.render(
  <Provider store={store} >
    <BrowserRouter>
      <div>
          <ul>
            <li><Link to='/'>一营</Link></li>
            <li><Link to='/erying'>二营</Link></li>
            <li><Link to='/qibinglian'>骑兵连</Link></li>
          </ul>
          <Route path='/' exact component={App}></Route>
          <Route path='/:location' component={Test}></Route>
      </div>
    </BrowserRouter>
  </Provider>,
    document.getElementById('root')
)

```

##### 其他组件
> url参数，Route组件参数可用冒号标识参数
> Redirect组件跳转
> Switch只渲染一个子Route组件

``` Javascript

ReactDom.render(
  <Provider store={store} >
    <BrowserRouter>
      <div>
          <ul>
            <li><Link to='/'>一营</Link></li>
            <li><Link to='/erying'>二营</Link></li>
            <li><Link to='/qibinglian'>骑兵连</Link></li>
          </ul>
          <Switch>
            {/* 只渲染命中的第一个Route */}
            <Route path='/' exact component={App}></Route>
            <Route path='/erying' component={Erying}></Route>
            <Route path='/qibinglian' component={Qibinglian}></Route>
            <Route path='/:location' component={Test}></Route>
          </Switch>
      </div>
    </BrowserRouter>
  </Provider>,
    document.getElementById('root')
)

```
























