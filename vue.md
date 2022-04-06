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

### mixin的特点
1. 组件的参数和方法不共享
2. 和组件的方法冲突时会被组件中的方法覆盖
3. 生命周期(created, mounted)中的代码会和组件中的生命周期中的代码合并执行

### $props、$attrs和$listeners
#### $props
当前组件接收到的props对象。vue实例代理了其对props对象属性的访问

#### $attrs
包含了父作用域中不作为被prop识别(且获取)的特性绑定(不包含class和style)

#### $listeners
包含了父作用域中(不含.native修饰器)v-on事件监听器。可以通过"v-on='$listeners'"传入内部组件---在创建更高的层次的组件时使用


### vue3和vue2
+ 生命周期：  
    vue3：大部分生命周期函数加上“on”前缀；生命周期函数需要提前引入  
    vue2：直接引入  
    tips：setup是围绕beforeCreate和created生命周期钩子运行的，不需要显式地去定义

+ 多根节点
    vue3：支持多根节点(fragment)  
    vue2：多节点会报错

+ 组合式API
    vue3：组合式api，可将同一逻辑的内容写到一起，增强可读性、内聚。
    vue2：选项式api

+ 异步组件
    vue3：suspense组件，允许程序在等待异步组件加载完成之前渲染兜底的内容。使用之前需要在模板中声明，包括两个命名插槽：default和fallback。  
    fallback：加载状态
    ```html
        <suspense>
            <template #default>
            <List />
            </template>
            <template #fallback>
            <div>
                Loading...
            </div>
            </template>
        </suspense>
    ```

+ Teleport
    vue3：可将部分DOM移动到Vue app之外的位置。

    ```html
        <button @click="dialogVisible = true">显示弹窗</button>
            <teleport to="body">
            <div class="dialog" v-if="dialogVisible">
                我是弹窗，我直接移动到了body标签下
            </div>
        </teleport>
    ```

+ 响应式原理
    vue3：proxy 对象代理  
    vue2：Object.definepProperty对象劫持    
    Object.defineProperty无法监听对象或数组新增、删除的元素

+ 虚拟dom
    vue3：增加patchFlag字段：  
    1：代表动态文本节点。在diff过程中，只需要对比文本内容，无需关注class、style等
    -1：代表静态节点，都保存为一个变量进行静态提升，在重新渲染时直接引用，无需重新创建

+ 事件缓存
    vue3的cacheHandler可在第一次渲染后缓存我们的事件。相对于vue无需每次渲染都传递一个新的函数，加一个click事件

+ Diff算法优化
    patchFlag帮助diff时区分静态节点，以及不同类型的动态节点。一定程序减少节点本身及其属性的对比时间

+ 打包优化
    tree-shaking  
    vue3针对全局和内部API进行了重构，对tree-shaking进行支持。全局API只能作为ES模块构建的命名导出进行访问。
    ```JS
        import { nextTick } from 'vue';   // 显式导入
        nextTick(() => {
        // 一些和DOM有关的东西
            });
    ```
    受影响的全局API
        - Vue.nextTick
        - Vue.observable （用 Vue.reactive 替换）
        - Vue.version
        - Vue.compile （仅全构建）
        - Vue.set （仅兼容构建）
        - Vue.delete （仅兼容构建）

+ Typescript支持
    vue3由ts重写，相对于vue2可以更好地支持ts，ts是一种类型系统，面向对象的语法  
    vue2：Option API中option是个简单对象，不是特别匹配  
    vue3：
         

### vue为什么不能使用index作为key
key是给每一个vnode的唯一id，可以依靠key更准确更快地拿到old vnode中对应的vnode节点，高效地更新vdom  
如果删除数组中的某一个元素，那么这个元素后的所有元素的index都会发生改变，与之对应key也会发生改变。

### scoped css可以影响子组件
使用>>> 
```css
    .a >>> b {...}
```

### v-show 是不是重排
是的。因为v-show改变了display属性。当渲染树中的一部分，因为元素的尺寸，布局，隐藏等改变而需要重新构建