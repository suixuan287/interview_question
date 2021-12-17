# Vue

### 深层嵌套组件数据传递
provide/inject
```
Todolist
    -todolist-item
        -todolist-item-state

Todolist.vue
{
    provide() {
        return {
            user: Vue.computed(() => this.userInfo.name)
        }
    }
}

todolist-item-state.vue
{
    inject: ['user'],
    create(){
        this.user
    }
}
```
vuex

### 如何获取dom
ref="domName"  
用法：this.$refs.domName  

  
### 为什么使用key?
需要使用key来给每个节点做一个唯一标识，Diff算法就可以正确的识别此节点。  
作用主要是为了高效的更新虚拟DOM。  

### computed和watch的使用场景
computed: 多个属性通过计算得到一个值时   
watch: 一条数据影响多条数据时使用

### $nextTick的使用
当你修改了data的值然后马上获取这个dom元素的值，是不能获取到更新后的值，  
你需要使用$nextTick这个回调，让修改后的data值渲染更新到dom元素之后在获取，才能成功。  
原理：vue修改data的方法是异步的，需要在setData的回调中才能得到当前值  

### vue更新数组时触发视图更新的方法
push()；pop()；shift()；unshift()；splice()； sort()；reverse()

### vue为什么要求组件模板只能有一个根元素？
vue2之前是可以多个存在的，但是vue2中引进了虚拟dom  
当前构建和diff 虚拟dom的算法还不支持多标签结构，也很难在保证性能的情况下支持  
当然函数式组件就没有这种问题了

### vuex主要解决什么问题
多个视图共享同一个状态  
来自不同视图的行为需要变更同一个状态

### 说说对Vue的理解
1. vue是一个渐进式框架，可以根据自己的需求添加功能  
2. 数据驱动采用mv vm模式，m数据层，v视图层，vm调度者  
3. 组件化，复用强

### vue优缺点
优点：  
数据驱动视图，组件化  
缺点：  
csr的首屏白屏问题
ie8以下浏览器不支持

### 如何解决vue白屏问题
主要原因是首屏需要加载很大的js文件，网速没那么快的时候会产生白屏  
可以使用懒加载和cdn资源优化，文件通过gzip压缩  
剩下的就是一些前端通用的手段：loading,骨架屏等等

### vue的diff算法
