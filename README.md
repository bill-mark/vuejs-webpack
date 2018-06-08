# vuejs-webpack
借助webpack自己搭建一个Vue脚手架

#1 cross-env
跨平台执行node命令插件

#2 DefinePlugin
NODE_ENV:isDev ? ' "development" ' :' "production" ' 
不加双引号调用时候会变成process.env.NODE_ENV = development 变量development是不存在的,会报错

#3 sourcemap
方便调试:将浏览器里的代码显示成开发者自己写的.代码映射