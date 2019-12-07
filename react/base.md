# 初步总结

## JSX

JSX 使用 html 标签和 javascript 可以混写， 在没有 jsx 之前，我们在 javascript 中生成 html 时，都是定义好 html 内容对应的字符串，然后再通过把 html 字符串设置到某个元素，或者通过 html 字符串创建实际的 html 元素，这种方式在编写代码时是非常不方便。

在 JSX 中可以这样：

~~~javascript
const tag = <h1>Hello, React!!!</h1>;
~~~

在 HTML 标签中可以引用 JS 的值：

~~~javascript
const tag = <h1 title={mytitle}>{myname}</h1>;
const tag2 = <div>{1 + 1}</div>
~~~

在大括号中表示 javascript 变量或表达式， 但是在大括号之间只能是一些简单的表达式，如 **if**, **else**, **for** 等语句是不能使用的，但是三元运行符可以使用
~~~javascript
const tag = <div>{i == 1 ? 'true' : 'false'}</div>
~~~

不能使用循环， 可以使用 map 函数：

~~~javascript
var names = ['asp.net', 'php', 'java', 'nodejs'];
const tags = (<div>
  {
    names.map((item) => {
      return <div>{item}</div>
    });
  }
</div>);
~~~

也可以先把生成的元素放到一个数组字，然后把数组放到标签中：

~~~javascript
var arr = [];
for(var i = 0; i < names.length; i++) {
  arr.push(<div>{names[i]}</div>);
}

const tags = <div>{arr}</div>;
~~~

***从上面的例子中可以看出， 在大括号中的表达式， 必须能返回一个值，所以还可以这样***

~~~javascript
var names = ['asp.net', 'php', 'java', 'nodejs'];
const tags = (
  <div>
    {
      (function() {
        var results = [];
        for(var i = 0; i < names.length; i++) {
          results.push(<div>{names[i]}</div>);
        }
        
        return results;
      })();
    }
  </div>
);
~~~

在里面定义一个函数并执行。

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

**style**： 不能使用 style="css" 这样来书写，需要使用 style={{fontSize:'20px', padding: '10px'}} 这样的格式来写。

**class**：由于 class 在 javascript 中是一个关键字，所以也不能在 html 标签中使用 class 属性，需要改用 className 属性。
**for**：for 也是 javascript 中的一关键字， 所以也不能在 html 标签中使用， 需要改用 htmlfor 来替代。
