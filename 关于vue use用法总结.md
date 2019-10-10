### 1.Vue.use函数执行，要在Vue实例化即new Vue(options)之前
在new Vue(options)时首先会执行this._init进行初始化，将Vue上的属性和options进行合并，然后在进行事件、生命周期等的初始化。beforeCreate,created生命周期的hook函数也是在这里进行调用
如果Vue.use在new Vue()之后执行，this._init()时你使用的插件的内容还没有添加到Vue.options.components、Vue.options.directives、Vue.options.filters等属性中。所以新初始化的Vue实例中也就没有插件内容

### 2. vue.use和vue.prototype的区别
打开引入插件包的main.js，留意到引入包或者文件有两种方式：
```
import Vue from 'vue'
import echarts from 'echarts'
import global from './global.js'	   //自己创建的全局变量函数文件
Vue.prototype.$echarts=echarts;
Vue.use(global)
```
这两种方式引入有什么区别吗？

 (1)不是为了vue写的插件(插件内要处理)不支持Vue.use()加载方式
 (2)非vue官方库不支持new Vue()方式
 (3)每一个vue组件都是Vue的实例，所以组件内this可以拿到Vue.prototype上添加的属性和方法。

### 3.实现vue插件的方式
    一种是将这个插件的逻辑封装成一个对象最后将最后在install编写业务代码暴露给Vue对象。这样做的好处是可以添加任意参数在这个对象上方便将install函数封装得更加精简，可拓展性也比较高。
    还有一种则是将所有逻辑都编写成一个函数暴露给Vue。
   其实两种方法原理都一样，无非第二种就是将这个插件直接当成install函数来处理

例子代码如下:
```
export const Plugin = {
    install(Vue) {
        Vue.component...
        Vue.mixins...
        Vue...
        // 我们也可以在install里面执行其他函数，Vue会将this指向我们的插件
        console.log(this)  // {install: ...,utils: ...}
        this.utils(Vue)    // 执行utils函数
        console.log(this.COUNT) // 0
    },
    utils(Vue) {
        Vue...
        console.log(Vue)  // Vue
    },
    COUNT: 0    
}
// 我们可以在这个对象上添加参数，最终Vue只会执行install方法，而其他方法可以作为封装install方法的辅助函数

const test = 'test'
export function Plugin2(Vue) {
    Vue...
    console.log(test)  // 'test'
    // 注意如果插件编写成函数形式，那么Vue只会把this指向null，并不会指向这个函数
    console.log(this)  // null
}
// 这种方式我们只能在一个函数中编写插件逻辑，可封装性就不是那么强了
```
