---
title: "React学习——4-1"
date: 2017-12-07 18:41:00
categories: 学习册
---

### Redux 基础知识
1.Redux是什么
2.Redux核心概念
3.Redux实战


<!-- more -->
---
##### Redux是什么
> 专注于状态管理
> 单一状态，单向数据流
> 核心概念：store, state, action, reducer

##### Redux主要功能
> store所有人的状态，在哪里都有记录(state)
> 需要改变的时候，需要告诉专员（dispatch）要干什么(action)
> 处理变化的人(reducer)拿到state和action，生成新的state

> 首先通过reducer新建store，随后通过store.getState获取状态
> 需要状态变更，store.dispatch(action)来修改状态
> Reducer函数接收state和action，返回新的state，可以用store.subscribe监听每次修改




