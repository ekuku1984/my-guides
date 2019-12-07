#初步总结

##标签格式

在 react 中，所有的 html 标签名称、属性名称都必须小写，否则会报找不到对象的错误

*style* 是一个特殊属性，不能使用 style="css" 这样来写，需要使用 style={{fontSize:'20px', padding: '10px'}} 这样的格式来写

*class*：由于 class 在 javascript 中是一个关键字，所以也不能在 html 标签中使用 class 属性，需要改用 className 属性
