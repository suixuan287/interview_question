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