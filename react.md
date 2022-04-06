# REACT

### react和vue区别
#### react
+ react整体是函数式的思想，把组件设计成纯函数，状态和逻辑通过参数传入。  
react中，是单向流数据。  
react在setState后会走重新渲染的流程，如果shouldComponentUpdate返回true就继续渲染；如果返回false，则不会重新渲染
+ All in js。通过js生成html、css
+ 类式的组件写法，更容易结合ts
+ 通过高阶组件扩展

#### vue
+ vue的思想是响应式，基于数据可变。通过对每个属性建立watcher来监听，当某个属性变化时，响应式地更新对应的虚拟dom  
当state特别多的时候，watcher会很多，会导致卡顿
+ 把html、css、js组合到一起，用各自的处理方式；vue有单文件组件，可以把html、js、写到一个文件中，html提供了模板引擎
+ 声明式写法，通过传入各种option，api和参数都很多。vue也可以结合vue-class-component实现类式的写法，但是要先通过decorator添加声明
+ 通过mixins扩展