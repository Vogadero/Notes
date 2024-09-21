## script 和 script setup在 Vue 3 组件中的写法

`<script>` 和 `<script setup>` 在 Vue 3 组件中的写法有着显著的区别，主要体现在它们的语法糖和用途上。

### `<script>` 写法

传统的 Vue 组件使用 `<script>` 标签来定义组件的逻辑。在 Vue 3 中，你可以继续使用这种方式，并结合 Composition API（如 `ref`、`reactive` 等）来编写组件。在 `<script>` 标签中，你需要显式地定义一个 `export default` 对象，该对象可以包含组件的选项，如 `data`、`methods`、`computed` 等，在 Vue 3 中，你通常会使用 `setup` 函数来定义响应式数据和逻辑。

```vue
<script>
import { ref } from 'vue';

export default {
  setup() {
    const msg = ref('Hello World!');
    return {
      msg
    };
  }
}
</script>
```

### `<script setup>` 写法

Vue 3 引入了 `<script setup>` 语法糖，旨在提供更简洁、更直观的组件编写方式。使用 `<script setup>` 时，你不需要显式地定义 `export default` 对象，也不需要返回响应式数据或方法。相反，你可以直接在 `<script setup>` 标签内部定义响应式数据、计算属性、方法等，并且它们会自动暴露给模板。

```vue
<script setup>
import { ref } from 'vue'

const msg = ref('Hello World!')
</script>
```

### 为什么引入 `<script setup>`？

Vue 团队引入 `<script setup>` 的主要原因是为了简化组件的编写，使代码更加简洁和直观。`<script setup>` 提供了以下优势：

1. **更少的样板代码**：你不需要再写 `export default` 和 `setup` 函数，这减少了不必要的重复代码。
2. **更好的类型推断**：在 `<script setup>` 中，Vue 3 的 TypeScript 支持更加友好，因为它能够更准确地推断出模板中使用的变量和方法的类型。
3. **更简洁的组件结构**：将响应式数据、计算属性、方法等直接定义在 `<script setup>` 中，使得组件的结构更加清晰和易于理解。
4. **更好的开发体验**：结合 Vue 3 的其他新特性（如 Composition API），`<script setup>` 提供了更加灵活和强大的组件编写方式，提升了开发者的开发效率。

总之，`<script setup>` 是 Vue 3 引入的一项重要语法糖，它旨在通过简化组件的编写方式来提升开发者的开发体验和效率。

### 区别

#### 1. 语法和使用方式

* **`<script>`**：
  - 需要显式地定义一个 `export default` 对象，该对象包含组件的选项，如 `data`、`methods`、`computed` 等。但在 Vue 3 中，更常见的是使用 `setup` 函数来定义响应式数据和逻辑。
  - `setup` 函数是组件中用于组合式 API 的入口点，它会在组件实例化之前被调用，并返回一个对象，该对象中的属性会被合并到组件的上下文中，供模板使用。
  - 需要手动返回响应式数据、方法等给模板。

* **`<script setup>`**：
  - 是 Vue 3 引入的一个语法糖，用于简化组件的编写。它允许你在 `<script>` 标签内直接编写与组件逻辑相关的代码，而无需显式地定义 `export default` 和 `setup` 函数。
  - 响应式数据、计算属性、方法等可以直接在 `<script setup>` 中定义，它们会自动暴露给模板，无需手动返回。
  - 提供了更简洁、更直观的写法，减少了样板代码。

#### 2. 对组件开发的影响

* **`<script>`**：
  - 传统的写法，对于熟悉 Vue 2 的开发者来说可能更容易上手。
  - 在处理复杂的组件逻辑时，可能需要更多的样板代码和手动操作。

* **`<script setup>`**：
  - 简化了组件的编写，提高了开发效率。
  - 使得组件的逻辑更加清晰和易于理解。
  - 提供了更好的 TypeScript 支持，能够更准确地推断出模板中使用的变量和方法的类型。
  - 需要注意的是，`<script setup>` 目前还处于 Vue 3 的官方支持范围内，但可能在某些特殊情况下存在限制或兼容性问题。

#### 3. 其他特点

* **`<script>`**：
  - 支持 Vue 2 和 Vue 3 的选项式 API 和组合式 API。
  - 在 Vue 3 中，虽然推荐使用组合式 API，但选项式 API 仍然可用。

* **`<script setup>`**：
  - 专为 Vue 3 设计，与组合式 API 配合使用效果最佳。
  - 提供了自动解构 `props`、`context` 等功能，进一步简化了代码。
  - 允许使用 `onXXX` 形式的生命周期钩子函数，而无需在 `setup` 函数中手动注册。

综上所述，`<script>` 和 `<script setup>` 在 Vue 3 组件中各有特点，开发者可以根据项目的具体需求和个人的偏好来选择使用哪种方式。对于追求简洁和高效开发的场景，`<script setup>` 是一个不错的选择。