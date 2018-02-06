---
title: "React学习——3-7"
date: 2017-12-07 16:40:00
categories: 学习册
---

### antd-mobile使用


<!-- more -->
---
##### antd-mobile
> 使用 npm install antd-mobile@next --save 安装最新版


``` Javascript

组件使用实例：
import { Button } from 'antd-mobile';
import 'antd-mobile/dist/antd-mobile.css';

ReactDOM.render(<Button>Start</Button>, mountNode);

```

#### 按需加载
> 使用 babel-plugin-import（推荐）。
>


``` Javascript

// .babelrc or babel-loader option
{
  "plugins": [
    ["import", { libraryName: "antd-mobile", style: "css" }] // `style: true` 会加载 less 文件
  ]
}

```

然后只需从 antd-mobile 引入模块即可，无需单独引入样式。

``` Javascript

// babel-plugin-import 会帮助你加载 JS 和 CSS
import { DatePicker } from 'antd-mobile';

```



