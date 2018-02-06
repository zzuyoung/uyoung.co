---
title: "ReactWEB应用-前后端联调"
date: 2018-01-01 14:31:00
comments: true
categories: 学习册
---

### 前后端联调
> 使用asios发送异步请求
> 如何发送，端口不一致，使用proxy配置
> axios拦截器，统一loading处理
> redux里使用异步数据，渲染页面
<!-- more -->

#### axios
> 简洁好用的发送请求库，npm install axios --save 安装
> Axios.interceptors 设置拦截器，比如全局的loading
> axios.get，.post发送请求，返回promise对象
> Redux里获取数据，然后dispatch即可

``` Javascript

    axios.get('/data').then(res=>res.data).then(cc=>console.log(cc));
    axios.get('/data').then(res=>console.log(res))
    axios.get('/data').then(res=>{
      if (res.status === 200) {
        this.setState({data:res.data[0]})
      }
      console.log(res)
    })


```


``` Javascript

import axios from 'axios';
import { Toast } from 'antd-mobile';

// 拦截请求
axios.interceptors.request.use(function(config) {
  Toast.loading('加载中',0)
  return config
})

// 拦截响应
axios.interceptors.response.use(function(config) {
  setTimeout(()=>{
    Toast.hide()
  },2000)
  return config
})


```

























