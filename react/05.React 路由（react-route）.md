# 路由

**react-router** 是 React 中的一个重要的组件，在制作单页面应用时，使用它来管理组件的显示，类似于 html 中 a 标签的功能，使用方法也工 a 标签基本一样;

## 第一步：安装 react-router-dom 包

默认情况下, 使用 react-create-app 创建的程序是不包含 react-router-dom 包，所以在使用之前必须安装此包：

~~~text
npm install react-router-dom --save
~~~

### Link, Route, BrowserRouter, 三个基本对象

要实现路由，必须使用这三个对象来配合使用， BrowserRouter（此对象可以替换为 HashRouter）, Link 和 Route 必须放在 BrowserRouter 对象的内部，否则报错，Link 对象用来做链接，供用户操作，Route 对象用来匹配 Link 的路径，并指定要显示的组件。

~~~javascript
import React, { Component } from 'react';
import ReactDOM from 'react-dom';
import { Link, Route, BrowserRouter } from 'react-router-dom';

function Home(props) {
    return <h1>Home Page</h1>;
}
function Products(props) {
    return <h1>Products Page</h1>;
}
function AboutUs(props) {
    return <h1>About Us</h1>;
}

class App extends Component {
    
    constructor(props) {
        super(props);
    }

    render() {
        return (
            <BrowserRouter>
                <div>
                    <div className="nav">
                        <ul>
                            <li><Link to="/">Home</Link></li>
                            <li><Link to="/Products">Products</Link></li>
                            <li><Link to="/AboutUs">About Us</Link></li>
                        </ul>
                    </div>
                    <div class="container">
                        {/* Route 对象放在哪里，指定的组件就显示在哪里 */}
                        <Route path="/" component={Home} />
                        <Route path="/Products" component={Products} />
                        <Route path="/AboutUs" component={AboutUs} />
                    </div>
                </div>
            </BrowserRouter>
        );
    }
}

ReactDOM.render(<App />, document.body);
~~~

### Link

Link 对象相当于 html 中的 a 标签，用来做链接供用户操作。

**to**： 指定链接的地址， 有两种设置方式：
~~~javascript
// 直接使用字符串的形式：
<Link to="/Products">Procudts</Link>

// 使用对象的形式：
<Link to={{
 pathname: '/Products',
 search: '?id=100',
 hash: '#top',
 replace: true,
 state: { fromHome: true }
}}>Procudts</Link>
~~~

### Route
Route 组中的 path 属性用来匹配 URL 地址，匹配上了就会显示此 Route 指定的 component。

**path**: 设置一个路径，用来匹配当前浏览器的 URL 地址，

~~~javascript
path="/post" // 匹配 /post 请求
path="/hello/:name" // 匹配 /hello/michael 和 /hello/ryan
path="/hello(/:name)" // 匹配 /hello, /hello/michael 和 /hello/ryan
path="/files/*.*" // 匹配 /files/hello.jpg 和 /files/path/to/hello.jpg
~~~

* :paramName – 匹配一段位于 /、? 或 # 之后的 URL。 命中的部分将被作为一个参数
* () – 在它内部的内容被认为是可选的
* – 匹配任意字符（非贪婪的）直到命中下一个字符或者整个 URL 的末尾，并创建一个 splat 参数

**component**: 设置显示的组件，当匹配到当前路径，并显示此组件时，Route 框架会给此组件的 props 属性里添加一个 match 属性，用来表示匹配 url 地址相关的数据。

> ### match 对象的属性
> - **params**: Route的 path 可以包含参数，例如 <Route path="/foo/:id" 包含一个参数 id。params就是用于从匹配的 URL 中解析出 path 中的参数，例如，当 URL = 'http://example.ocm/foo/1' 时，params= {id: 1}。
> - **isExact**: 是一个布尔值，当 URL 完全匹时，值为 true; 当 URL 部分匹配时，值为 false.例如，当 path='/foo'、URL="http://example.com/foo" 时，是完全匹配; 当 URL="http://example.com/foo/1" 时，是部分匹配。
> - **path**: Route 的 path 属性，构建嵌套路由时会使用到。
> - **url**: URL 的匹配的方式

### render:
render 的值是一个函数，这个函数返回一个 React 元素。这种方式方便地为待渲染的组件传递额外的属性。例如：
~~~javascript
<Route path='/foo' render={(props) => {
  <Foo {...props} data={extraProps} />
}}>
</Route>

### children:
children 的值也是一个函数，函数返回要渲染的 React 元素。 与前两种方式不同之处是，无论是否匹配成功， children 返回的组件都会被渲染。但是，当匹配不成功时，match 属性为 null。例如:

~~~javascript
<Route path='/foo' children={(props) => {
  <div className={props.match ? 'active': ''}>
    <Foo {...props} data={extraProps} />
  </div>
}}>
</Route>
~~~

如果 Route 匹配当前 URL，待渲染元素的根节点 div 的 class 将设置成 active.

### exact:
exact默认为false，如果为true时，需要和路由相同时才能匹配，但是如果有斜杠也是可以匹配上的。
如果在父路由中加了exact，是不能匹配子路由的,建议在子路由中加exact。

### strict
strict默认为false，如果为true时，路由后面有斜杠而url中没有斜杠，是不匹配的

## 嵌套路由
嵌套路由是指在Route 渲染的组件内部定义新的 Route。例如，在上一个例子中，在 Posts 组件内再定义两个 Route:

~~~javascript
const Posts = ({match}) => {
  return (
    <div>
      {/* 这里 match.url 等于 /posts */}
      <Route path={`${match.url}/:id`} component={PostDetail} />
      <Route exact path={match.url} component={PostList} />
    </div>
  )
}
~~~

## 参考链接
[官方文档](http://react-guide.github.io/react-router-cn/docs/Introduction.html)

讲得比较详细：

<https://segmentfault.com/a/1190000016421036>

<https://segmentfault.com/a/1190000019130514>
