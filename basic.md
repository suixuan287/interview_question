# 基础

### cpu、进程、线程的关系
+ 一个进程可以包括多个线程，多个线程共享进程资源  
+ 进程是cpu资源分配的最小单位  
+ 线程是cpu调度的最小单位  
+ 不同进程之间可以通信，代价很大
+ 单线程与多线程，都是指在一个进程内的单和多

### 渲染进程包含哪些线程
+ GUI渲染线程：负责渲染页面，布局和绘制；与js引擎线程互斥
+ JS引擎线程：负责处理解析和执行js脚本程序；只有一个js引擎线程(单线程)
+ 事件触发线程：控制事件循环；满足触发条件时，将事件放入js引擎所在的执行队列中
+ 定时触发器线程：setInterval和setTimeout所在的线程；定时任务不是由js引擎计时；计时完毕后通知事件触发线程
+ 异步http请求线程：浏览器有一个单独的线程用于处理ajax请求

### 为什么js是单线程
同时操作dom的时，在多线程不加锁的情况下，最终会导致dom渲染的结果不可预期

### 为什么gui渲染线程与js引擎线程互斥
js是可以操作dom的，如果同时修改元素属性并同时渲染页面，那么渲染线程前后获得的元素可能不一致  
因此当js引擎线程执行时gui渲染引擎会被挂起

### Event Loop
任务分为宏任务和微任务  
会把宏任务和微任务添加到宏任务队列和微任务队列中  
执行一个宏任务过程中如果遇到微任务，就将它添加到微任务队列中  
宏任务执行完毕后，立即执行当前微任务队列中的所有微任务  
当前宏任务执行完毕后，开始检查渲染，然后GUI线程接管渲染  
渲染完毕后，js线程继续接管，开始下一个宏任务

### 宏任务、微任务有哪些
+ 宏任务：主代码块、setTimeout、setInterval等
+ 微任务：promise、process.nextTick等


### webpack构建优化
+ 分包：设置externals，将react, react-dom等基础包通过cdn引入和externals
+ 分离基础脚本，使用dllPlugin和dllReferencePlugin将不经常更新的模块提前构建，每次构建只针对有变化的模块
+ 缩小构建范围，loader不解析node_modules
+ 开发环境不做目录内容清理，计算文件hash，提取css文件等
+ babel-loader开启缓存（加上cacheDirectory: true或使用transform-runtime插件）
+ 使用commonsChunkPlugin提取公共模块
+ 使用noParse 排除不需要解析的模块
+ 使用模块化引入 import xx form 'x/xx'

### 图片加载优化
使用window.addEventListener捕捉error事件，然后把默认图片赋值给target

### 跨域原理
跨域是指浏览器不能执行其他网站的脚本。它是由浏览器的同源策略造成的，是浏览器对js实施的安全限制  
地址A加载的页面，不能访问地址B的服务， AB不同源。  
同源：域名、协议、端口均相同


### 跨域解决方法
+ jsonp
+ 通过修改document.domain来跨子域  
A页面通过iframe嵌入了B页面，两个页面是同主域的，这时可以同时设置两个页面的document.domain为主域。
可能两个页面的domain就是一致，但是必须显示地设置domain
+ 使用window.name window.name只能是字符串，最大2m
+ window.postMessage方法 接收端使用 window.onmessage方法接受
+ CORS 

### CORS
cors是跨域资源共享，是一种ajax跨域请求资源的方式。仅支持IE10以上  
cors需要浏览器和服务器同时支持。  
其实整个cors通信过程，都是浏览器自动完成，不需要用户参与。浏览器一单发现ajax跨域，就会自动添加一些附加的header信息。实现cors通信的关键是服务器。  
cors请求分为两种，简单请求(simple request)和非简单请求(not-so-simple request)
+ 简单请求：
    - 方法：head,get,post
    - header信息有只有以下几种：Accept, Accept-Language, Content-Language, Last-Event-ID, Content-Type
    过程：浏览器发送cors请求，会在header中增加 origin: a.com, 如果服务器接收这个请求，就在Access-Control-Allow-Origin头部回发同样的源信息 Access-Control-Allow-Origin: a.com  
    ps: 如果origin不在允许范围内，服务器会返回一个正常的http回应，这种错误无法通过状态码识别，因为http请求状态码可能是200  
    服务器响应的header信息里有以下字段：  
        * Access-Control-Allow-Origin: 必须；如果是* 表示接受任意域名的请求
        * Access-Control-Allow-Credentials: 可选；boolean； 表示是否允许发送cookie
        * Access-Control-Expos-Headers： 可选；指定扩展的header字段

+ 非简单请求 一般是特殊要求的请求，put、delete 或者content-type: application/json

### 基本数据类型和引用数据类型的区别
+ 基本数据类型：Number, String, Boolean, Null, Undefined
+ 引用数据类型：Object, Array, Function等
不同：
+ js中基本数据类型是保存在栈内存(Stack)中；引用数据类型保存在堆内存(Heap)中  
+ 访问机制：基本数据类型可以直接访问；引用数据类型在访问时需要先得到这个对象在堆内存中的地址，再按照这个地址去获得这个对象的值，也就是按引用访问
+ 复制变量(变量赋值)：基本数据在复制时会将原始值的副本给新变量，但是他们俩是完全独立的，只是拥有了相同的value； 引用数据在复制时，原数据会把自身保存的地址赋值给新变量，这两个变量指向了堆内存中的同一个对象
+ 参数传递：基本数据只把变量里的value传递给新参数，新参数和原数据互不影响；引用数据再参数传递时传递的是内存地址

### 变量提升
var 命令，变量可以在声明之前引用，值为undefined；

### 暂时性死区
在代码块内, 使用let命令声明变量之前，该变量都是不可用的。在语法上称为暂时性死区


### js继承
1. 原型链继承
2. 构造函数继承
3. 组合继承
4. 寄生组合式继承
5. class extends

### cookie操作
document.cookie 获取到一个string

### virtual DOM
js对象模拟dom树 -> 比较两颗virtual dom树的差异 -> 把差异应用到真正的dom树上

### KVC
key value coding

### GC


### webview

### jscore
jscore是webkit默认内嵌的js引擎。由Apple用C开发  
jscore其实就是给APP提供了一个js可以解释执行的运行环境与资源  
分为JSContext, JSManagedValue, JSValue, JSVirtualMachine(JSVM)  
+ JSContext  
一个jsContext表示一次js的执行环境（执行上下文）。所有的js代码都必须在一个jsContext中执行。  
可以通过创建一个jsContext去调用js脚本，访问一些js定义的值和函数，同时也提供了让js访问native对象、方法的接口  
+ JSManagedValue
+ JSValue
+ JsVirtualMachine JSVM js虚拟机  
jsvm主要做两个事情
    - 把js代码编译成byteCode(字节码)
    - 解释器：运行js编译生成的byteCode（字节码）
    - runtime(运行时): 负责运行时的内存空间开辟、管理等 
+ JSExport
实现jsExport协议可以开放oc类和他们的实例方法、类方法、属性给js调用。只有在jsExport里面开放的方法js才能调用

### null和undefined区别
null：没有对象，该处没有值
+ 作为函数的参数，表示该函数的参数不是对象
+ 作为对象原型链的终点
undefined: 缺少值，此处应该有一个值，但是还未定义
+ 变量被声明，但是没赋值，就是undefined
+ 调用函数，应该提供的参数没有提供，该参数等于undefined
+ 对象没有赋值的属性，该属性的值为undefined
+ 函数没有返回值时，默认返回undefined

### 判断js数据类型的方式
+ typeof: typeof null >> 'object'; 引用数据类型都会返回object; function 返回function
+ instanceof: a instanceof b; 只能判断是否属于实例关系，不能判断一个对象具体属于哪种类型；
+ toString： Object.prototype.toString.call('') [object, String]
+ constructor: ''.constructor == String; true.constructor == Boolean; null和undefined无constructor, 会报错

### 隐式转换
ToPrimitive(val, preferredType); 如果对象为data类型，则preferredType缺省为String； 否则为number
+ ({} + {}) = ?
    - 先进行 ToPrimitive(val, preferredType) 转换，没有指定preferredType，{}会使用默认的number进行转换
    - 执行valueOf()方法，{}.valueOf() 返回还是{}, 不是原始值
    - 执行toString方法，{}.toString() 返回'[object object]', 是原始值
    - '[object object][object object]'

+ 2 * {} = ?
    - * 只能对number类型进行运算，首先对{}进行ToNumber类型转换
    - {}.toString() 返回[object object]
    - [object, object] ToNumber运算， 转换为NaN
    - 2 * NaN = NaN

+ [] == !{}
    - !{} 为false
    - [] == false; ToNumber(false) = 0; [] == 0
    - ToPrimitive([], number) == ''
    - ToNumber('') = 0; 
    - 0 == 0 true

+ 🌰
``` 
    const a = {
        i: 1,
        toString: function () {
            return a.i++;
        }
    }
    if (a == 1 && a == 2 && a == 3) {
        console.log('hello world!');
    }
```
hello world

### js小数精度丢失
由于计算机是用二进制来存储和处理数字，不能精确表示浮点数，而js中没有相应的封装类来处理浮点数运算，直接计算会导致运算精度丢失  
0.1转化为二进制0.000110011...是一个无限循环小数
0.1+0.2 = 0.30000000000000004  
一般做法：（0.1 * 10 + 0.2 * 10)/10 = 0.3 

## 原型和原型链

### 构造函数
通过new 函数名 来实例化对象的函数叫构造函数。  
构造函数的主要功能为初始化对象，特点是和new一起用  
一般构造函数首字母大写

### new的实现原理
1. 创建一个新对象
2. 将新对象的_proto_指向构造函数的prototype对象
3. 将构造函数的作用域赋值给新对象(this指向新对象)
4. 执行构造函数中的代码(为这个对象添加属性)
5. 返回新对象

### 原型设计模式
原型实例指向创建对象的种类，并通过拷贝这些原型创建新的对象，是一种用来创建对象的模式。也就是创建一个对象作为另一个对象的prototype属性。  
原型模式最大的方便就是 创建对象的时候不一定非得使用类模板，可以将一个对象做模板生成新对象

### 原型规则
+ 所有的引用类型(数组，对象，函数)都具有对象特性，可自由扩展
+ 所有的引用类型都具有一个__proto__属性(隐式原型)，值是一个普通的对象
+ 所有的函数都具有一个prototype(显式原型)，属性是一个普通对象
+ 所有引用类型的隐式原型指向其构造函数的显式原型(obj.__proto__ === Object.prototype)
+ 当试图得到一个对象的某个属性时，如果本身没有这个属性，则会去它的__proto__属性中去找

### 原型链
当试图得到一个对象的某个属性时，如果本身没有这个属性，则会去它的__proto__属性中去找；如果Obj.__proto__中没有时，则会去obj.__proto__.__proto__(obj的构造函数的prototype的构造函数的prototype)中去找，直到找不到返回null

### instanceof 原理
```
    function instance_of(L, R) {
        var O = R.prototype; 
        L = L.__proto__;
        while (true) {    
            if (L === null)      
                return false;   
            if (O === L) 
                return true;   
            L = L.__proto__;  
        }
    }
```

### class的理解
static静态方法，不会被实例继承，只能直接通过类调用  
class 通过extends继承，子类必须先调用super()，拿到this对象才能做其他操作，普通方法中指向父类原型对象，可以获取到父类的属性  
static静态方法，会被子类继承，直接调用  
类不存在变量提升  

