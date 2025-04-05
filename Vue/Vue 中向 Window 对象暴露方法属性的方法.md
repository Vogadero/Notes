# Vue 中向 Window 对象暴露方法/属性的方法

> 本文整合了三种在 Vue 项目中向 `window` 对象暴露方法/属性的方案，包含直接暴露、原型链管理、安全属性定义等多种方式，并对比了它们的优缺点。

[Toc]

## 一、组件内直接暴露方法（简单方案）

### 实现方式
```javascript
// 组件内部实现
export default {
  mounted() {
    window.globalFn = () => {
      this.getDetail();
    }
  },
  methods: {
    getDetail() { /* 业务逻辑 */ }
  }
}
```

### 特点

- 优点
  - 实现简单，适合暴露方法较少的场景
- 缺点
  - 全局变量易冲突（暴露越多冲突风险越高）
  - 方法分散在各组件，难以统一管理

## 二、通过 Vue 实例集中管理（推荐方案）

### 实现步骤

1. **暴露 Vue 实例**

   ```javascript
   // main.js
   const vm = new Vue({ ... }).$mount('#app');
   window.vm = vm;
   ```

2. **创建方法管理文件**

   ```javascript
   // @/utils/export2vmFunction.js
   export function install(Vue) {
     Vue.prototype.globalFn1 = function() {
       const component = findComponentDownward(this, 'ComponentName');
       component.methodName();
     };
   }
   
   // 组件查找工具函数
   function findComponentDownward(context, name) { ... }
   ```

3. **挂载到 Vue 原型**

   ```javascript
   // main.js
   import vmFunction from '@/utils/export2vmFunction';
   Vue.use(vmFunction);
   ```

### 调用方式

```javascript
window.vm.globalFn1();
```

### 特点

- 优点
  - 全局变量仅 `vm` 一个，降低冲突风险
  - 方法集中管理，维护性强
  - 支持组件树查找（`findComponentDownward`）
- 缺点
  - 需要理解 Vue 原型链机制
  - 组件查找可能增加复杂度

## 三、通过独立 JS 文件定义属性

### 实现方式

1. **创建配置文件**

   ```javascript
   // public/config/expose.js
   window.appConfig = {
     apiUrl: 'https://api.example.com',
     debugMode: true
   };
   ```

2. **引入配置文件**

   ```html
   <!-- public/index.html -->
   <script src="/config/expose.js"></script>
   ```

### 安全增强方案（不可修改属性）

```javascript
// 使用 Object.defineProperty
Object.defineProperty(window, 'readOnlyConfig', {
  value: { key: 'immutable' },
  writable: false,
  configurable: false
});
```

------

## 四、其他补充方案

### 1. jQuery 扩展（需依赖 jQuery）

```javascript
$.extend(window, {
  methodA: () => { ... },
  methodB: () => { ... }
});
```

### 2. 构造函数模式

```javascript
function ExposeMethods() {
  this.method1 = () => { ... };
}
window.expose = new ExposeMethods();
```

### 3. 动态挂载属性

```javascript
// 组件生命周期中挂载
created() {
  window.customMethod = this.internalMethod;
}
```

## 方案对比表

|         方案          | 维护性 | 安全性 | 侵入性 |        适用场景        |
| :-------------------: | :----: | :----: | :----: | :--------------------: |
|    组件内直接暴露     |   低   |   低   |   高   |      快速原型开发      |
|   Vue 实例集中管理    |   高   |   中   |   低   |       中大型项目       |
|     独立 JS 文件      |   中   | 可配置 |   中   |      配置参数暴露      |
| Object.defineProperty |   高   |   高   |   低   | 需要防止篡改的关键配置 |

------

## 最佳实践建议

1. **优先使用 Vue 实例管理方案**
   适合需要暴露多个方法/属性的场景，通过原型链保持代码整洁。
2. **敏感属性使用安全定义**
   对关键配置使用 `Object.defineProperty` 防止意外修改。
3. **避免全局污染**
   即使使用 `window` 暴露，也应通过命名空间（如 `window.app = {}`）组织属性。
4. **混合使用策略**
   - 高频方法通过 Vue 实例暴露
   - 配置参数通过独立 JS 文件定义
   - 客户端调用的关键方法使用安全定义