### css文件加载是否影响dom渲染
css 是由单独的下载线程异步下载  
css加载不会阻塞DOM树解析,异步加载时DOM照常构建  
会阻塞render树渲染。(渲染时需等css加载完毕，因为render树需要css信息)

### 重排和重绘