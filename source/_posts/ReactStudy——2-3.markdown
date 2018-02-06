---
title: "React学习——2-3"
date: 2017-12-06 23:52:53
categories: 学习册
---

### Express + mongodb 开发web后台接口
1.Express 开发web接口
2.非关系型数据库mongodb
3.使用nodejs的mongoose模块链接和操作mongodb

<!-- more -->
##### Express
> 基于nodejs，快速、开发极简的web开发框架
> npm install express -save  安装express
> hello world 应用
> 监听路由和响应内容，使用nodemon自动重启 npm install -g nodemon
> 其他的特性
app.get、app.post分别开发get和post接口
app.use使用模块
代res.send（返回文本）、res.json（返回json）、res.sendfile（返回文件）响应不同的内容

``` Javascript

const express = require('express')

const app = express()
app.get('/',function(req,res){
  res.send('<h1>Hello word</h1>')
})

app.get('/data',function(req,res){
  res.json({name: 'imooc', type: 'IT'})
})

app.listen(9093, function(){
  console.log('Node app start at port 9093')
})

```
node server/server.js  //启动node server
nodemon erver/server.js  //启动node server

##### mongodb + mongoose
> 非关系型数据库
> 官网。。。。。。
> 下载、安装
> mongoos安装 npm install mongoose -save
> 通过mongoose操作mongodb，存储的就是json，相对mysql来说要易用很多
Connect链接数据库
定义文档模型，Schema和mode新建模型


``` Javascript

const mongoose = require('mongoose')

//链接mongo 并且使用imooc这个集合
const DB_URL = 'mongodb://127.0.0.1:27017/imooc'
mongoose.connect(DB_URL)
mongoose.connection.on('connected', function(){
  console.log('mongo connect success')
})

//类似于mysql的表 mongo里有文档、字段的概念
const User = mongoose.model('user', new mongoose.Schema({
  user:{type:String, require:true},
  age:{type:Number, require:true}
}))

```

> String、Number 等数据结构
定create、remove、update分别用来增、删、改的操作
Find和findOne用来查询数据
``` Javascript

 新增一个用户信息
User.create({
  user:'xiaoming',
  age:15
},function(err, doc){
  if(!err) {
    console.log(doc)
  }else{
    console.log(err)
  }
})

  删除agr=18的对象
User.remove({age:18},function(err, doc){
  if(!err){
    console.log('delete success')
    User.find({},function(e, d){
      console.log(d)
    })
  }else{
    console.log(err)
  }
})

更新
User.update({'user':'xiaoming'},{'$set':{age:26}}, function(err, doc){
  console.log(doc)
})

app.get('/data', function(req,res){
  User.find({}, function(err,doc){
    res.json(doc)
  })
})

```
mongod --dbpath F:\MongoDBData  //启动mongodb