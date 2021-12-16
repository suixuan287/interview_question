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

