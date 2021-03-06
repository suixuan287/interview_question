### 微信小程序实现原理
+ 渲染层：界面渲染相关的任务全部在webview线程里执行，小程序存在多个页面，所以渲染层存在多个webview线程
+ 逻辑层：采用jscore线程运行js脚本，在这个环境下执行的都是有关小程序业务逻辑的代码

小程序是基于双线程，任何在视图层和逻辑层之间的数据传递都是线程间的通信，会有一定的延迟，因此在小程序中，页面更新是异步操作  
异步会使得各个部分的运行时序变得复杂。比如在渲染首屏时，逻辑层与渲染层同时开始初始化，但是渲染层需要有逻辑层的数据才能把界面渲染出来  

### 小程序运行机制：冷启动
冷启动：用户首次打开或小程序被微信主动销毁后再次打开的情况，此时小程序需要重新加载启动

### 小程序运行机制：热启动
热启动：用户已经打开过小程序，然后在一定时间内再次打开该小程序，无需重新启动，只需将后台态的小程序切换到前台，这个过程就是热启动  
小程序进入后台后，客户端会维持一段时间的运行状态，超过一定时间后（5分钟）后会被微信主动销毁

### Exparser框架
exparser是微信小程序的组织框架，内置在小程序的基础库中。小程序的所有组件，包括内置和自定义组件，都由exparser组织管理

### 小程序性能优化
+ 精简代码，降低wxml结构和js代码的复杂性
+ 合理使用setData调用，减少setData次数和数据量，对于无关界面渲染的数据，不应在data中定义
+ 使用分包

### setData原理
视图层使用webview作为渲染载体，逻辑层是独立的jscore作为运行环境。  
webview和jscore都是独立的线程，通过两边提供的evaluateJavascript实现。即用户传输的数据，需要将其转换为字符串形式传递，同时把转换后的数据拼接成一份js脚本，再通过执行的js脚本传递到两边独立的环境。  
数据到达视图层并不是实时的，是异步的

### 支付宝小程序的渲染引擎
支付宝小程序使用webview渲染+native渲染两种模式