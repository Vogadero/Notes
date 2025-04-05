# CommonJS 与 ES Module 深度对比与使用指南

[Toc]

---

## 模块化发展背景
### 1.1 CommonJS 起源
- **诞生时间**：2009年由Ryan Dahl引入Node.js
- **核心目标**：解决JS全局污染和依赖混乱问题
- **应用场景**：服务端开发（Node.js）、Browserify打包工具

### 1.2 ES Module 演进
- **标准化时间**：ES6（2015年）正式成为浏览器原生模块标准
- **核心优势**：静态分析、异步加载、Tree Shaking支持
- **发展推动**：AMD/CMD规范铺垫，官方标准化统一方案

---

## 核心差异对比
### 2.1 模块加载机制
| 特性         | CommonJS           | ES Module       |
| ------------ | ------------------ | --------------- |
| 加载时机     | 运行时动态加载     | 编译时静态解析  |
| 加载方式     | 同步加载           | 异步加载        |
| 代码位置限制 | 可动态嵌入条件语句 | 仅限顶层作用域  |
| 模块缓存     | 基于文件路径缓存   | 基于URL路径缓存 |

### 2.2 值传递机制
| 特性         | CommonJS                   | ES Module              |
| ------------ | -------------------------- | ---------------------- |
| 基础类型     | 值拷贝（内存独立）         | 只读引用（编译时绑定） |
| 引用类型     | 共享内存地址               | 动态绑定（实时更新）   |
| 导出修改影响 | 需重新赋值`module.exports` | 原模块修改实时同步     |

### 2.3 语法特性对比
```javascript
// CommonJS
const mod = require('./module');
module.exports = { ... };

// ES Module
import mod from './module.mjs';
export default { ... };
```

------

## 实现原理剖析

### 3.1 CommonJS 核心机制

- **模块包装器**：通过`(function(exports, require, module, __filename, __dirname){...})`封装
- **缓存策略**：`Module._cache`实现模块单例
- **循环引用处理**：未完成模块返回空对象，后续补全（DFS遍历）
- **动态加载原理**：运行时解析依赖关系

### 3.2 ES Module 核心机制

- **静态分析阶段**：构建模块依赖图（DAG）
- **实时绑定机制**：通过模块环境记录（Module Environment Record）实现
- **动态加载方案**：`import()`返回Promise实现懒加载
- **严格模式强制**：模块顶层this指向undefined

------

## 使用场景与选择建议

### 4.1 适用场景对比

|       场景        |        推荐方案         |          原因说明           |
| :---------------: | :---------------------: | :-------------------------: |
| Node.js旧版本维护 |        CommonJS         | 保证向后兼容性（<v12.17.0） |
|   新Node.js项目   |        ES Module        |  标准化、Tree Shaking支持   |
|   浏览器端开发    |        ES Module        |   原生支持、代码分割优化    |
|   混合模块交互    | CJS→ESM互用需`.cjs`扩展 |     Node.js的互操作限制     |

### 4.2 迁移注意事项

1. **文件扩展名规范**：`.mjs`显式声明ES模块
2. **package.json配置**：设置`"type": "module"`
3. **动态导入替代方案**：`import()`替代`require()`
4. **默认导出差异**：ESM的`default`属性访问方式

------

## 社区趋势与兼容性

### 5.1 发展趋势

- **Node.js官方路线**：v13.2+稳定支持，v14+生产环境可用
- **主流库迁移现状**：inquirer/ora等知名库已转向纯ESM
- **工具链支持**：Webpack5+/Rollup全面支持ESM编译

### 5.2 兼容性挑战

|    问题类型    |         解决方案         |
| :------------: | :----------------------: |
| 旧版本Node兼容 |     保留CJS入口文件      |
|  第三方库兼容  | 使用Babel/TypeScript转译 |
|   浏览器兼容   |  搭配Webpack等打包工具   |

------

## 最佳实践与迁移策略

### 6.1 新项目开发建议

1. 优先使用ES Module标准
2. 配置`package.json`的`type`字段
3. 使用ESLint强制模块规范
4. 结合Vite/Rollup等现代构建工具

### 6.2 旧项目迁移步骤

1. **渐进式迁移**：逐个文件改为`.mjs`扩展
2. **混合模式运行**：通过`--experimental-vm-modules`标志
3. **依赖库检测**：使用`esm`包过渡方案
4. **构建工具升级**：配置Babel预设`@babel/preset-env`

> **扩展阅读**：[ECMAScript Modules 规范](https://tc39.es/ecma262/#sec-modules) | [Node.js ESM文档](https://nodejs.org/api/esm.html)