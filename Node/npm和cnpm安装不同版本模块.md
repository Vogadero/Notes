# npm安装不同版本模块

> npm install --save 模块名@版本（安装后写入package.json并默认保存到dependencies中（运行时依赖)
>
> npm install --save -dev 模块名@版本(安装后写入package.json并默认保存到devDependencies中（开发时依赖）

[按版本号安装](按版本号安装)

[全局安装](全局安装)
3. [本地安装](本地安装)
4. [信息写入](#信息写入)
5. [删除模块](删除模块)
6. [安装cnpm](安装cnpm)
7. [更新cnpm或npm](更新cnpm或npm)

## 1. 按版本号安装

- 在npm中安装固定的版本号package，只需要在其后加 ‘@版本号’
- 例如：react-router已经更新到4.x版本，想要下载2.x版本，可以通过下面命令

```shell
npm install --save-dev react-router@2.8.1
或 npm install --save react-router@2.8.1
```

## 2. 全局安装

```shell
npm install xxx -g  //模块将被下载安装到【全局目录】中
```

## 3. 本地安装

```shell
npm install xxx //则是将模块下载到当前命令行所在目录
```

## 4. 信息写入

```shell
npm install xxx --save   // 简写：npm install xxx -S
npm install xxx --save-dev  // 简写：npm install xxx -D
//安装的同时，将信息写入package.json中项目路径中
```

```shell
--save 将依赖包名称添加到 package.json 文件 dependencies 下
--save-dev 则添加到 package.json 文件 devDependencies 下
dependencies：运行时的依赖，发布后，即生产环境下还需要用的模块
devDependencies：开发时的依赖。里面的模块是开发时用的，发布时用不到它
```

```shell
--save 是你发布之后还依赖的东西
--save-dev 是你开发时候依赖的东西
```

- 比如babel 是发布时，将 ES6 代码编译成 ES5 ，那么 babel 就是devDependencies。
  Vue项目中vue-router，由于发布之后还是依赖vue-router，所以是dependencies。

## 5. 删除模块

```shell
npm uninstall 模块
 
删除本地模块时你应该思考的问题：是否将在package.json上的相应依赖信息也消除
npm uninstall 模块：删除模块，但不删除模块留在package.json中的对应信息
npm uninstall 模块 --save 删除模块，同时删除模块留在package.json中dependencies下的对应信息
npm uninstall 模块 --save-dev 删除模块，同时删除模块留在package.json中devDependencies下的对应信息
```

## 6. 安装cnpm

```shell
$ npm install -g cnpm --registry=https://registry.npm.taobao.org
//安装完后查看版本
$ cnpm -v
//成功后会有版本信息返回，不成功有可能是node版本低
```

- 成功安装后，可以直接用cnpm代替之前的npm

```shell
//安装模块
$ cnpm install [name]

//同步模块
$ cnpm sync connect
//直接通过 sync 命令马上同步一个模块, 只有 cnpm 命令行才有此功能

//其它命令,支持 npm 除了 publish 之外的所有命令
$ cnpm info connect
```

## 7. 更新cnpm或npm

```shell
$ cnpm update -g
$ npm update -g
```

