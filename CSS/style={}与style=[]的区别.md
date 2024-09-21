# :style="{}"与:style="[]"的区别

在 Vue.js 中，`:style` 是一个用于绑定内联样式的指令。`:style="{}"` 和 `:style="[]"` 的主要区别在于它们如何处理样式的键值对。

### **`:style="{}"`**

当你使用对象语法 `:style="{}"` 时，你可以直接在对象中定义 CSS 属性和对应的值。这种语法允许你以 JavaScript 对象的形式来绑定样式。

示例：


```vue
<template>
  <div :style="{ color: activeColor, fontSize: fontSize + 'px' }">Hello World</div>
</template>

<script>
export default {
  data() {
    return {
      activeColor: 'red',
      fontSize: 30
    };
  }
}
</script>
```
在上面的示例中，`color` 和 `fontSize` 是 CSS 属性，而 `activeColor` 和 `fontSize` 是 Vue 实例的 data 属性。Vue 会自动将这些数据属性的值转换为 CSS 属性的值，并应用到元素上。

### **`:style="[]"`**

当你使用数组语法 `:style="[]"` 时，你可以传入一个数组，数组的每个元素都是一个对象，这样你就可以应用多个样式对象到同一个元素上。每个对象中的样式键值对都会被合并并应用到元素上。

示例：


```vue
<template>
  <div :style="[baseStyles, overridingStyles]">Hello World</div>
</template>

<script>
export default {
  data() {
    return {
      baseStyles: {
        color: 'blue',
        fontSize: '20px'
      },
      overridingStyles: {
        fontSize: '30px' // 这将覆盖 baseStyles 中的 fontSize
      }
    };
  }
}
</script>
```
在上面的示例中，`baseStyles` 和 `overridingStyles` 都是对象，并且它们都被包含在一个数组中。Vue 会合并这两个对象中的样式键值对，并应用到元素上。如果有冲突的样式属性（如 `fontSize`），则后一个对象中的值会覆盖前一个对象中的值。

总结：

* `:style="{}"` 允许你以对象的形式直接绑定样式。
* `:style="[]"` 允许你传入一个数组，其中每个元素都是一个样式对象，从而实现样式的合并和覆盖。