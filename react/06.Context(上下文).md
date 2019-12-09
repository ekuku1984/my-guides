
# Context, 上下文, React.createontext(defaultValue)

通过 createContext 方法， 返回一个 ReactContext 对象，此对象包含两个属性， Provider 和 Consumer,  通过 Provider 提供数据，
在其他组件中， 通过 Consumer 来获取到相应的数据， 只要 Consumer 在 Provider 的子级中，不管中间隔了多少级元素，都是可以直接获取
Provider 提供的数据，而不需要通过每个组件的 props 属性来一级一级的传递。

~~~javascript
/* index.js */
import React from 'react';

// 通过 React.createContext 方法创建一个 ReactContext 对象
var TestContext = React.createContext({
    name: 'zhangshan'
});

// 导出 TestContext， 让其他地方可以使用
export default TestContext;

/* Page.jsx */
import React, { Component } from 'react';
import TestContext from './index';
import Title from './Title';
import Content from './Content';

class Page extends Component {
    constructor(props) {
        super(props);
        this.state = {
            name: 'zhangshan'
        }
    }

    render() {
        console.log(TestContext);
        return (
            <div>
                {/* 通过 TestContext.Provider 标签的 value 属性，把值传传递到子级的 TestContext.Consumer */}
                <TestContext.Provider value={{name: this.state.name}}>
                    <Title />
                </TestContext.Provider>

                <button type="button" onClick={()=> this.setState({name: 'star'})}>click me</button>
            </div>
        )
    }
}

export default Page;

/* Title.jsx */
import React, { Component } from 'react';
import TestContext from './index'
import Avatar from './Avatar'

class Title extends Component {

    constructor(props) {
        super(props);
    }

    render() {
        return (
            <div>Title
                {/* Avatar 组件中使用了 TestContext.Consumer */}
                <Avatar />
            </div>
        );
    }

}

export default Title;

/* Avatar */
import React, { Component } from 'react';
import TestContext from './index';

class Avatar extends Component {

    constructor(props) {
        super(props);
    }

    render() {
        return (
            // TestContext.Consumer 的子级只能是一个函数，TestContext.Consumer 会把上级
            // 的 TestContext.Provider 对象设置的 value 传递给函数
            <TestContext.Consumer>
                {
                    (ctx) => {
                        return <h1>{ctx.name}</h1>
                    }
                }
            </TestContext.Consumer>
        );
    }

}

export default Avatar;
~~~

# Context 实现原理

React.Component 组件有个 context 属性，此属性会获取上级的 Context.Provider 对象（如果存在的话），Context.Consumer 对象是放到 Context.Provider 对象的下面，所以 Context.Consumer 对象可以通过 context 属性获取到 Context.Provider 对象。
Context.Consumer 的子级只能是一个函数， 在 Context.Consumer 对象的 render 方法，Consumer 直接通过 this.props.children(this.context) 的方式来调用。

## 这里有几个参考的地方：

<https://blog.csdn.net/weixin_33768153/article/details/82668110> 超级简单的实现。

[React.createContext()? 大神们又在作什么妖？](https://zhuanlan.zhihu.com/p/34038469) 这篇文章从使用和原理都讲得挺详细的。

[React 官方插件实现](https://github.com/jamiebuilds/create-react-context) 这个是官方的实现，用来兼容老版本的 React。
