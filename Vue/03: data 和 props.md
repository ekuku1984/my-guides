# data 和 props

和 React 一样， 在 VUE 中， 也有状态和 props, 在 VUE 中， 状态就是 data, 访问 data 不需要通过 data 属性，而是直接使用 VUE 实例进行访问，props 里的数据也是直接通过 VUE 实例来访问，VUE 会把 data、props 和 methods 里面的属性全部放到 VUE 实例中去，所以 data、props 和 methods 三个对象里的 KEY 是不能有同名的。不然会出错

## data
data 是组件内部维护的一组数据，是可修改的，data 又分普通 属性和计算属性。

### 计算属性（computed） 
[官方参考文档](https://cn.vuejs.org/v2/guide/computed.html)

计算属性，就是根据其他的值计算而来的值。声明在 computed 属性中：

~~~javascript
// 可为计算属性定义为一个方法
var vue = new Vue({
    computed: {
        propName() {
            return this.firstName + ' ' this.lastName
        }
    }
});

// 分别定义计算属性的 get 和 set 方法
var vue = new Vue({
    computed: {
        propName: {
            get() {
                return this.firstName + ' ' + this.lastName;
            },
            set(value) {
                if(value) {
                    var parts = value.split(' ');
                    this.firstName = parts[0];
                    this.lastName = parts[0]
                } else {
                    this.firstName = '';
                    this.lastName = '';
                }
            }
        }
    }
});
~~~

#### 计算属性的缓存
可以有两种方式引用计算属性：
~~~javascript
var vue = new Vue({
    computed: {
        fullName() {
            return this.firstName + ' ' + this.lastName;
        }
    }
});
~~~
第一种引用方式：
~~~html
<div>{{fullName}}</div>
~~~
这种方式，只要 firstName 和 lastName 不变化，则在重新展示此元素时，不会在调用 fullName 方法来获取要显示的值，而是使用一个缓存值。

第二种引用方式：
~~~html
<div>{{fullName()}}</div>
~~~
直接调用计算属性的方法，这样，值不会缓存，每次重新显示此元素时，都会调用此方法来生新计算要显示的值。

## props
[官方文档](https://cn.vuejs.org/v2/guide/components-props.html)

props 是父组件传递到组件的数据（单向传递），不可修改。props 的使用方式和 React 差不多，不过也有所不同：

~~~javascript
// 简单指定属性名
var vue = new Vue({
    props: ['属性名称1', '属性名称2'...]
});

// 属性的详细信息设置
Vue.component('my-component', {
  props: {
    // 基础的类型检查 (`null` 和 `undefined` 会通过任何类型验证)
    propA: Number,
    // 多个可能的类型
    propB: [String, Number],
    // 必填的字符串
    propC: {
      type: String,
      required: true
    },
    // 带有默认值的数字
    propD: {
      type: Number,
      default: 100
    },
    // 带有默认值的对象
    propE: {
      type: Object,
      // 对象或数组默认值必须从一个工厂函数获取
      default: function () {
        return { message: 'hello' }
      }
    },
    // 自定义验证函数
    propF: {
      validator: function (value) {
        // 这个值必须匹配下列字符串中的一个
        return ['success', 'warning', 'danger'].indexOf(value) !== -1
      }
    }
  }
});
~~~

~~~javascript
var post = {
  id: 1,
  title: 'My Journey with Vue'
};
// 把对象的属性全部传递过去：
<blog-post v-bind="post"></blog-post>

// 上面是等价于这样：
<blog-post
  v-bind:id="post.id"
  v-bind:title="post.title"
></blog-post>

// 如果要传一个对象过去，这样：
<blog-post
  v-bind:data="post"
></blog-post>
~~~

## 监控 data 的变化

和 React 一个很大不同点就是，在 VUE 里 data 里的值是可以双向绑定的（控件的值变化自动修改 data 里对应的值，虽然也是通过语法糖实现的），所以大部分时候，我们不需要去监控控件的修改值事件，而是直接监控 data 属性值的变化，可以直接在 watch 属性中配制。

~~~js
var vue = new Vue({
    watch: {
        // 在 watch 里直接写上要 watch 的属性名，只要一变化，就会调用此方法
        propName(newValue, oldValue) {
            // 进行处理
        }
    }
})
~~~

也可以调用 vue.$watch 方法来监控：
~~~javascript
var unwatch = vue.$watch('path', function(newValue, oldValue){

}, {
    deep: true, // 是否监控子级属性
    immediate: true // 调用 $watch 方法时， 会马上执行一次改变事件，可以用来初始化
})
~~~

$watch 方法会返回一个函数，调用此函数，可以取消 watch。
