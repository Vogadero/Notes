# Vue 3 的 `setup` 函数中，`context` 对象的属性和方法

在 Vue 3 的 `setup` 函数中，`context` 对象包含了多个有用的属性和方法，这些属性和方法可以帮助你更好地管理组件的生命周期、事件处理和插槽。以下是 `context` 对象的所有主要属性和方法：

## `context` 对象的属性和方法

1. **`attrs`**：

   - 类型：`{ [key: string]: any }`

   - 描述：包含所有未被 `props` 声明的属性。这些属性通常是传递给组件的原生属性或自定义属性。

   - 示例：

     ```typescript
     const { class, style, ...otherAttrs } = attrs;
     ```

2. **`slots`**：

   - 类型：`{ [name: string]: () => VNode[] }`

   - 描述：包含所有传递给组件的插槽。默认插槽可以通过 `slots.default` 访问。

   - 示例：

     ```typescript
     const defaultSlot = slots.default ? slots.default() : [];
     ```

3. **`emit`**：

   - 类型：`(event: string, ...args: any[]) => void`

   - 描述：用于触发自定义事件。通常与 `v-on` 指令配合使用。

   - 示例：

     ```typescript
     emit('update:modelValue', newValue);
     ```

4. **`expose`**：

   - 类型：`(exposed: Record<string, any>) => void`

   - 描述：用于暴露组件的公共属性和方法，使其可以在父组件中通过 `ref` 或 `template refs` 访问。

   - 示例：

     ```typescript
     expose({
       counter,
       increment
     });
     ```

5. **`parent`**：

   - 类型：`ComponentPublicInstance | null`

   - 描述：指向父组件的实例。可以用来访问父组件的属性和方法。

   - 示例：

     ```typescript
     const parentData = context.parent?.data;
     ```

6. **`root`**：

   - 类型：`ComponentPublicInstance`

   - 描述：指向根组件的实例。可以用来访问根组件的属性和方法。

   - 示例：

     ```typescript
     const rootData = context.root.data;
     ```

7. **`isCE`**：

   - 类型：`boolean`

   - 描述：指示当前组件是否是一个自定义元素（Custom Element）。

   - 示例：

     ```typescript
     if (context.isCE) {
       console.log('这是一个自定义元素');
     }
     ```

8. **`isVNodeCE`**：

   - 类型：`boolean`

   - 描述：指示当前组件是否是一个 VNode 自定义元素。

   - 示例：

     ```typescript
     if (context.isVNodeCE) {
       console.log('这是一个 VNode 自定义元素');
     }
     ```

9. **`appContext`**：

   - 类型：`AppContext`

   - 描述：包含应用级别的上下文信息，如全局属性和插件。

   - 示例：

     ```typescript
     const globalConfig = context.appContext.config;
     ```

## 示例

假设我们有一个子组件 `ChildComponent`，我们希望在父组件中访问它的 `counter` 属性和 `increment` 方法，并且使用 `context` 对象的其他属性。

### 子组件

```typescript
typescript<template>
  <div>
    <p>当前计数: {{ counter }}</p>
    <button @click="increment">增加计数</button>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue';

export default {
  name: 'ChildComponent',
  setup(props, context) {
    // 解构 context 参数
    const { attrs, slots, emit, expose, parent, root, isCE, isVNodeCE, appContext } = context;

    const counter = ref(0);

    const increment = () => {
      counter.value++;
    };

    // 暴露 counter 和 increment 方法
    expose({
      counter,
      increment
    });

    onMounted(() => {
      console.log('子组件已挂载');
      console.log('父组件的数据:', parent?.data);
      console.log('根组件的数据:', root.data);
      console.log('是否是自定义元素:', isCE);
      console.log('是否是 VNode 自定义元素:', isVNodeCE);
      console.log('应用上下文配置:', appContext.config);
    });

    return {
      counter,
      increment
    };
  }
};
</script>
```

### 父组件

```typescript
typescript<template>
  <div>
    <child-component ref="childRef"></child-component>
    <button @click="accessChildMethods">访问子组件方法</button>
  </div>
</template>

<script>
import { ref } from 'vue';
import ChildComponent from './ChildComponent.vue';

export default {
  components: {
    ChildComponent
  },
  setup() {
    const childRef = ref(null);

    const accessChildMethods = () => {
      if (childRef.value) {
        console.log('子组件的计数:', childRef.value.counter);
        childRef.value.increment();
      }
    };

    return {
      childRef,
      accessChildMethods
    };
  }
};
</script>
```

## 解释

1. **子组件**：
   - 解构 `context` 参数：
     - `attrs`：获取未被 `props` 声明的属性。
     - `slots`：获取传递给组件的插槽。
     - `emit`：触发自定义事件。
     - `expose`：暴露组件的公共属性和方法。
     - `parent`：访问父组件的实例。
     - `root`：访问根组件的实例。
     - `isCE`：检查是否是自定义元素。
     - `isVNodeCE`：检查是否是 VNode 自定义元素。
     - `appContext`：访问应用级别的上下文信息。
2. **父组件**：
   - 使用 `ref` 获取子组件的引用。
   - 通过 `childRef.value` 访问子组件的公共属性和方法。