# Vue 2 响应式系统与数据更新机制总结

## 一、响应式原理概述
- **核心机制**：基于 `Object.defineProperty` 实现属性劫持。
- **限制**：
  - **对象**：无法自动检测动态添加/删除的属性。
  - **数组**：无法检测索引赋值（如 `arr[0] = val`）和 `length` 修改。
- **根级属性限制**：初始化时必须声明 `data` 中的根级属性，否则无法响应式。

## 二、对象的响应式处理
### 1. 动态添加/更新属性
- **`Vue.set` / `this.$set`**  
  语法：`Vue.set(target, key, value)` 或 `this.$set(target, key, value)`  
  用途：向嵌套对象添加新属性并触发视图更新。
  
- **`Object.assign` 替换对象**  
  错误示例：`Object.assign(this.obj, { newKey: 1 })`（不触发更新）  
  正确示例：`this.obj = Object.assign({}, this.obj, { newKey: 1 })`

### 2. 删除属性
- **`Vue.delete` / `this.$delete`**  
  语法：`Vue.delete(target, key)` 或 `this.$delete(target, key)`

## 三、数组的响应式处理
### 1. 数组变异方法（自动触发更新）
- **重写方法**：`push`, `pop`, `shift`, `unshift`, `splice`, `sort`, `reverse`。
- **底层原理**：Vue 重写数组原型链方法，拦截操作并通知依赖更新。

### 2. 索引操作或替换整个数组
- **索引赋值**：  
  错误示例：`this.arr[0] = newVal`（不触发更新）  
  正确方式：`this.$set(this.arr, 0, newVal)`
  
- **替换整个数组**：  
  方式1：`this.arr.splice(0, this.arr.length, ...newArr)`  
  方式2：`this.$set(this, 'arr', newArr)`

## 四、强制更新组件
- **`this.$forceUpdate()`**  
  - **用途**：强制重新渲染组件，绕过响应式检测。
  - **注意**：性能消耗大，仅作为最后手段（如依赖非响应式数据）。

## 五、高级技巧
### 1. 使用 `Vue.observable`
- **创建独立响应式对象**：  
  ```javascript
  import Vue from 'vue';
  const reactiveObj = Vue.observable({ key: 'value' });

- **特点**：直接修改属性（如 `reactiveObj.key = 'new'`）可自动触发更新。

## 六、常见问题与注意事项

### 1. `v-model` 绑定限制

- **必须绑定 LHS（左值表达式）**：
  错误示例：`v-model="arr[index]"`（若 `arr` 未预先声明响应式）
  正确示例：确保操作的是已定义的响应式数组或对象属性。

### 2. `computed` 属性依赖

- **必须依赖已存在的响应式数据**：
  错误示例：`computed` 依赖未初始化的对象属性（如 `this.obj.newKey`）。
  正确方式：动态属性需先通过 `this.$set` 声明。

### 3. 性能优化

- **避免频繁使用 `$forceUpdate`**：优先使用响应式 API。
- **大型数组操作**：优先使用原生 `splice` 而非 `Vue.set`（性能更优）。

## 七、底层实现细节（数组响应式）

### 1. 为何不用 `Object.defineProperty` 监听数组？

- **性能问题**：数组长度不确定，预先劫持所有索引消耗大。
- **特殊处理**：通过重写数组原型方法（变异方法）拦截操作。

### 2. 实现方式

- 原型链劫持：

  ```javascript
  const arrayProto = Array.prototype;
  const arrayMethods = Object.create(arrayProto);
  ['push', 'pop', ...].forEach(method => {
    const original = arrayProto[method];
    def(arrayMethods, method, function mutator(...args) {
      const result = original.apply(this, args);
      this.__ob__.dep.notify(); // 触发更新
      return result;
    });
  });
  ```

- **浏览器兼容**：支持 `__proto__` 则替换原型链，否则直接扩展数组方法。

------

**总结**：Vue 2 的响应式系统通过 `Object.defineProperty` 和数组方法重写实现，开发者需通过 `Vue.set`、变异方法等 API 处理动态数据，并注意性能与初始化声明问题。非必要情况下避免使用 `$forceUpdate`。