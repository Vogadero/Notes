# :class="{}"与:class="[]"的区别

在 Vue.js 中，`:class` 是一个用于绑定类名的指令。你可以使用对象语法或数组语法来动态地绑定类名。

### 使用对象语法

当你使用对象语法时，`:class="{}"` 中的对象的键是类名，而值是一个布尔表达式。如果表达式的值为真，则该类名会被添加到元素上；如果为假，则不会添加。

例如：

```vue
<template>
  <div :class="{ active: isActive, 'text-danger': hasError }">Hello World</div>
</template>

<script>
export default {
  data() {
    return {
      isActive: true,
      hasError: false
    };
  }
}
</script>
```

在这个例子中，如果 `isActive` 为真，则 `div` 元素将拥有 `active` 类名；如果 `hasError` 为真，则 `div` 元素将拥有 `text-danger` 类名。

### 使用数组语法

数组语法允许你应用一个类名数组到元素上。数组的每个元素是一个字符串，代表一个类名。

例如：

```vue
<template>
  <div :class="[activeClass, errorClass]">Hello World</div>
</template>

<script>
export default {
  data() {
    return {
      activeClass: 'active',
      errorClass: 'text-danger'
    };
  }
}
</script>
```

在这个例子中，`div` 元素将拥有 `active` 和 `text-danger` 这两个类名。

你还可以将对象语法和数组语法混合使用，以在数组中嵌入对象：

```vue
<template>
  <div :class="[{ active: isActive }, errorClass]">Hello World</div>
</template>

<script>
export default {
  data() {
    return {
      isActive: true,
      errorClass: 'text-danger'
    };
  }
}
</script>
```

在这个混合使用的例子中，如果 `isActive` 为真，则 `div` 元素将拥有 `active` 类名，并且无论如何都会拥有 `text-danger` 类名。

### 总结

- `:class="{}"` 使用对象语法，允许你根据布尔表达式动态地添加或移除类名。
- `:class="[]"` 使用数组语法，允许你应用一个类名数组到元素上。

选择使用哪种语法取决于你的具体需求。如果你需要根据条件动态地添加或移除单个类名，对象语法可能更合适。如果你需要一次性应用多个类名，或者想要混合使用对象和数组，那么数组语法或混合使用可能更合适。