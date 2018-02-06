---
title: "React学习——3-1"
date: 2017-12-06 23:55:00
categories: 学习册
---

### React基础知识
1.Reacts 是什么
2.实用React实现组件化
3.React如何进阶实用
4.事件
5.生命周期

<!-- more -->
> 更新React
npm install --save react@next react-dom@next

---
##### 组件之间传递数据
> 组件之间用props传递数据
> 使用<组件 数据=“值” >的形式传递
> 组件里使用this.props获取值
> 如果组件只有 render 函数，还可以用函数的形式写组件

``` Javascript
import React from 'react';

class App extends React.Component{
  render() {
    const boss = '李云龙'
    return (
      <div>
        <h2>独立团，{boss}</h2>
        <YiYing buff='张大喵' />
        <Qbl buff='孙德胜' />
      </div>
    )
  }
}

function Qbl(props){
  return <h2>骑兵连连长{props.buff}，冲啊！</h2>
}

class YiYing extends React.Component{
  render() {
    return <h2>一营营长，{this.props.buff}</h2>
  }
}

export default App
```
##### 组件内部state
> 组件内部通过state管理状态
> jsx本质就是js，所以可以直接 数组.map渲染列表
> Constructor（构造函数） 设置初始状态，记得执行super（props）
> 如State就是一个不可变的对象，使用this.state获取

```Javascript
class YiYing extends React.Component{
  constructor(props) {
    super(props);
    this.state = {
      solders:['虎子', '柱子', '王根生']
    }
  }
  render() {
    return (
      <div>
        <h2>一营营长，{this.props.buff}</h2>
        <ul>
          {this.state.solders.map((value, key)=>{
            return <li key={key}>{key}.{value}</li>
          })}
        </ul>
      </div>
    )
  }
}
```


##### 事件
> JSX里，onClick={this.函数名}来绑定事件
> this引用的问题，需要在构造函数里用bind绑定this
> this.setState修改state，记得返回新的state，而不是修改

```javascript

<button onClick={()=>this.addSolder()}>新兵入伍</button>
<button onClick={this.addSolder.bind(this)}>新兵入伍</button>

this.addSolder = this.addSolder.bind(this)
<button onClick={this.addSolder}>新兵入伍</button>

```


##### React生命周期
> React组件有若干钩子函数，在组件不同的状态执行
> 初始化周期
> 组件重新渲染生命周期
> 组件卸载生命周期


![React生命周期](http://cdn-hexo.uyoung.co/Reactlifecycle.jpg)

![React生命周期](http://cdn-hexo.uyoung.co/Reactlifecycle-1.jpg)
```javascript

class YiYing extends React.Component{
  constructor(props) {
    super(props);
    this.state = {
      solders:['虎子', '柱子', '王根生']
    }
  }
  
  componentWillMount() {
    console.log('组件马上渲染')
  }
  componentDidMount(){
    console.log('组件加载完毕')
  }
  
  render() {
	console.log('组件正在加载')
	return (
		<div>
			<ul>
				{this.state.solders.map((value, key)=>{
					return <li key={key}>{key}.{value}</li>
				})}
			</ul>
		</div>
	)
  }
  
}

```









