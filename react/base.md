# 初步总结

## 组件名称

在 react 中，定义的组件名称必须使用 **大写** 开头，否则在 jsx 中使用的时候，会当作一个普通的 html 标签直接输出到页面，不会进行任何转换。
~~~javascript
// 组件定义
import React, { Component } from 'react'

// 此于的类名 Hello 第一个字母必须大写
class Hello extends Components {
  render() {
    return (
      <div>Hello, React!!!</div>
    }
  }
}

export default Hello;

// 使用 Hello 组件
import React, { Component } from 'react';
import Hello from './Hello.js'

class App extends Component {
  render() {
    return (
      <Hello />
    );
  }
}

export default App;
~~~


## 标签格式

在 react 中，所有的 html 标签名称、属性名称都必须 **小写**，否则会报对象未定义的错误
~~~html
<!-- 将提示 H1 未定义 -->
<H1>标题</H1>
~~~

**style**： 不能使用 style="css" 这样来书写，需要使用 style={{fontSize:'20px', padding: '10px'}} 这样的格式来写

**class**：由于 class 在 javascript 中是一个关键字，所以也不能在 html 标签中使用 class 属性，需要改用 className 属性
