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


### Proxy
### defineProperty的缺陷
+ 无法监听数组
+ 无法监听对象

### vue组件间通信
+ props emit
+ vuex
+ inject provide
+ eventBus

### vue解析模板
1. parse: 解析template中的内容转化为AST(抽象语法树)  
用正则等方式解析template模板中的指令、class、style等数据
2. optimize: 优化AST：检测不需要进行DOM改变的静态子树
    + 将静态子树变成常数，
    + 在patch的过程中直接跳过
主要作用是标记static静态节点，后面当update更新界面时，会有一个patch的过程，diff算法会直接跳过静态节点，减少比较的过程，优化patch的性能
3. generate: 根据AST生成所需的code(包含render和staticRenderFns)  
generate是将AST转化成render function字符串的过程，得到的是render的字符串以及staticRenderFns字符串

### VNODE
将DOM抽象成一个以js对象为节点的虚拟dom树，以VNode节点模拟真实DOM，可以对这颗抽象树进行创建、删除、修改等操作对节点进行操作。修改以后警告diff算法得出一些需要修改的最小单位，再将这些小单位的视图进行更新。  
VNode包含以下数据：  
+ tag 当前节点的标签名
+ text 当前节点文本
+ elm 当前虚拟节点对应的真实dom节点
+ children 当前节点的子节点，是一个数组
+ key 节点的key属性，被当做节点的标志，应以优化
+ parent 当前节点的父节点
+ isStatic 是否为静态节点
+ isComment 是否为注释节点
+ isRootInsert 是否作为根节点插入  
...

### patch
patch的核心：diff算法。diff算法是通过同层的树节点进行比较而非对树进行逐层搜索遍历的方式，所以时间复杂度只有O(n)，是一种相当高效的算法。  
在patch的过程中，如果两个VNode被认为是同一个VNode(sameVNode),则会进行深度的比较，得出最小差异，否则直接删除旧有的DOM节点，创建新的DOM节点。  
+ 如果新旧VNode都是静态的，同时他们的key相同(代表同一节点)，并且新的VNode是clone或者标记了once，那么只需要替换elm一级componentInstance即可
+ 新老节点均有子节点，则对子节点进行diff操作，调用updateChildren，这个updateChildren也是diff的核心
+ 如果老节点没有子节点而新节点存在子节点，先清空老节点DOM的文本内容，然后为当前DOM节点接入子节点
+ 当新节点没有子节点而老节点有子节点时，移除该DOM节点的所有子节点
+ 当新老节点都没有子节点时，只是文本的替换

### sameVNode
+ key相同
+ tag
+ isComment
+ 是否data
+ 标签是input的时候，type必须相同