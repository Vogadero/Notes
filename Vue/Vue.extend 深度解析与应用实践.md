# Vue.extend 深度解析与应用实践

[Toc]

---

## 概述
**Vue.extend** 是 Vue 的全局 API，用于创建 Vue 构造器的“子类”，实现动态组件挂载和全局命令式组件。其核心特点包括：

- **灵活挂载**：允许通过 JS 动态创建组件实例并挂载到任意 DOM 节点。
- **复用性**：适用于需要多次独立实例化的组件场景（如弹窗、通知）。
- **源码级继承**：基于 Vue 原型链扩展，继承全局配置（如 mixin、指令）。

---

## 核心原理（源码解析）
### 1. 构造函数与继承
```javascript
// 源码核心逻辑（简化版）
Vue.extend = function (extendOptions) {
  const Super = this; // 基类（Vue）
  const Sub = function VueComponent(options) {
    this._init(options); // 调用 Vue 原型链的初始化方法
  };
  
  // 原型继承
  Sub.prototype = Object.create(Super.prototype);
  Sub.prototype.constructor = Sub;
  
  // 合并选项（全局配置 + 局部配置）
  Sub.options = mergeOptions(Super.options, extendOptions);
  
  // 缓存机制（避免重复创建）
  const cachedCtors = extendOptions._Ctor || (extendOptions._Ctor = {});
  cachedCtors[Super.cid] = Sub;
  
  return Sub;
};
```

### 2. 关键特性

- **选项合并**：通过 `mergeOptions` 将全局配置（如 `Vue.component` 注册的组件）与局部配置合并。
- **原型代理**：自动代理 `props` 和 `computed` 到实例原型，避免重复定义。
- **缓存优化**：相同 `extendOptions` 的构造器会被缓存复用。

### 3. 初始化流程

- **`_init()` 方法**：实例化时调用，完成组件状态初始化（props、data、生命周期等）。
- **挂载触发**：若未传递 `el` 选项，需手动调用 `$mount()`。

------

## 使用方式

### 1. 基础用法

```javascript
const Profile = Vue.extend({
  template: '<p>{{firstName}} {{lastName}}</p>',
  data() { return { firstName: 'Walter', lastName: 'White' }; }
});

// 实例化并挂载
new Profile().$mount('#mount-point');
```

### 2. 高级配置

|    **功能**    |                         **实现方式**                         |
| :------------: | :----------------------------------------------------------: |
| **传递 Props** |     `new Component({ propsData: { message: 'Hello' } })`     |
|  **绑定事件**  |               `instance.$on('event', handler)`               |
|  **插槽支持**  | 动态设置 `$scopedSlots`: `instance.$scopedSlots.footer = () => [h('div')]` |

------

## 应用场景

### 1. 全局命令式组件

#### **ElementUI 的 $message**

- **实现原理**：通过 `Vue.extend` 创建弹窗组件实例，挂载到 `body` 后动态销毁。

- 代码示例：

  ```javascript
  this.$message({ message: '提示内容', type: 'success' });
  ```

#### **自定义 Loading 组件**

```javascript
// 创建构造器
const LoadingConstructor = Vue.extend(Loading);

// 全局挂载方法
Vue.prototype.$showLoading = (options) => {
  const instance = new LoadingConstructor({ propsData: options });
  document.body.appendChild(instance.$mount().$el);
};
```

### 2. 动态渲染组件

```javascript
// 根据接口数据渲染不同组件
const ComponentClass = Vue.extend(getDynamicComponent());
new ComponentClass({ propsData: dynamicProps }).$mount(targetEl);
```

### 3. Vue.extend vs Vue.component

|   **特性**   |      **Vue.extend**      |      **Vue.component**       |
| :----------: | :----------------------: | :--------------------------: |
| **注册方式** | 返回构造器，需手动实例化 | 全局注册，模板中直接使用标签 |
| **适用场景** |     动态挂载、多实例     |    静态组件、少量全局组件    |
|  **复用性**  |  高（独立实例互不干扰）  |        低（单例模式）        |

------

## 总结

1. **核心价值**
   - 提供底层组件构造能力，适合动态化、高定制场景。
   - 帮助深入理解 Vue 组件系统的初始化流程和选项合并机制。
2. **最佳实践**
   - 优先用于需要 **JS 动态调用** 的组件（如通知、弹窗）。
   - 结合 `$mount()` 和手动 DOM 操作实现精准挂载控制。
3. **延伸学习**
   - 阅读 ElementUI 的 `Message` 组件源码，学习生产级实现。
   - 掌握 `mergeOptions` 的合并策略，理解组件配置优先级。