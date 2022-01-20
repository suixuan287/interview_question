### TEG
支付宝小程序的渲染引擎
小程序的优化手段
h5端用什么优化
externals 具体怎么配置
splitChunk原理
iframe之间通信方式
http缓存
具体描述ETAG
vue双向绑定原理
页面渲染流程
css会不会影响dom加载
怎么做前端监控
白屏监控怎么做
发布流程
笔试：
```
    console.log('begin')
    new Promise((resolve) => {
        console.log('promise0')
    })
    setTimeout(() => {
        console.log('timeout1')
        new Promise((resolve, reject) => {
            console.log('promise2')
            setTimeout(() => {
                console.log('timeout2')
            })
            resolve()
        }).then(() => {
            console.log('promise3')
        })
    }, 0)
    console.log('end')
``` 
实现一个方法：
a(2,3,4) = 9
a(2)(3)(4) = 9
a(2)(3,4) = 9