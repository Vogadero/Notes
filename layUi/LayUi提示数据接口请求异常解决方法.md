# LayUi提示数据接口请求异常解决方法

![](images/01.jpg)

[TOC]

### 报错：

> ​	CORS策略已阻止从源“http://localhost:8080”访问“localhost:8888/register”处的xmlhttprequest:跨源请求仅支持协议方案：http、data、chrome、chrome扩展、https。

### 问题：

> 我的请求是file 协议 , 他不支持这种 所以请求数据异常

### 解决：

> 让HTML文件在服务器上运行

### 两种方法：

1. 打开自己项目文件夹，在终端中输入`npm install http-server -g hs`就可以启动web服务器了。
2. 在vscode中安装Live Server插件。