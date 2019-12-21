# v-model

v-model 属性，是个语法糖，最终在标签上生成的是这样子的：

~~~html
<!--
<input v-model="vmField"> 生成如下代码
-->
<input v-bind:value="vmField" v-on:input="vmField = $event.target.value" />
~~~

从上面生成的代码中可以看出，所属的 v-model 是一个双向绑定的功能，其实最终是通过 input 事件来修改数据。

如果在自定义组件上使用 v-model，可能是需要做一些特殊的处理才能正常使用此指令，首先，自定义组件有没有 value 属性或 input 事件，如果没有，则需要进行一些配制，Vue 组件的选项中有一个 model 的选项，可以参考一下:

<https://cn.vuejs.org/v2/guide/components.html#在组件上使用-v-model>
<https://cn.vuejs.org/v2/api/#model>

### 自定义一个组件

~~~html
<template>
    <input ref="start" v-bind:value="start">
    <input ref="end" v-bind:value="end">
</template>
<script>
export default {
    // 通过设置 model 属性，来定义 v-model 语法糖的生成
    model: {
        prop: 'rangevalue', // 定义绑定到哪个属性上
        event: 'change' // 定义通过哪个事件来收集数据
    },

    // 计算属性
    computed: {
        rangevalue: {
            get: function() {
                if(this.start && this.end) {
                    return this.start + ',' + this.end;
                }

                return null;
            },
            set: function(newValue) {
                if(newValue) {
                    var parts = newValue.split(',');
                    this.start = parts[0];
                    this.end = parts[1];
                } else {
                    this.start = '';
                    this.end = '';
                }
            }
        }
    }
}
</script>
~~~

从上面的组件中可以看出，我们定义了一个 rangevalue 的计算属性，当外面给组件设置 rangevalue 时， 会调用计算属性的 set 方法，在 set 方法里，我们把值折分成两个值，分别放到 start 和 end 两个属性里，在 template 里的两个输入框分别绑定 start 和 end 属性，并且这两个输入框在输入的时候会引发一个 change 事件，父级组件里，通过 change 事件收集到改变的数据。

为什么会通过 change 事件来收集数据，在 model 属性里指定了此事件名称。
