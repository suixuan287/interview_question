### webpack构建流程
+ 初始化option
+ 开始编译
+ 从entry开始递归地分析依赖，对每个依赖模块进行build
+ 对模块位置进行解析
+ 开始构建某个模块
+ 将loder加载完成的module进行编译，生成AST
+ 遍历AST，当遇到require等一些调用表达式时，收集依赖
+ 所有依赖build完成，开始优化
+ 输出到dist目录