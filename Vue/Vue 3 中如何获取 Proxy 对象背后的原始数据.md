# Vue 3 中如何获取 `Proxy` 对象背后的原始数据

## 1. 使用 `toRaw` 方法

### 解释

1. **导入 `toRaw` 方法**：
   - `import { toRaw } from 'vue';`：从 Vue 3 中导入 `toRaw` 方法。
2. **获取原始数组**：
   - `const rawValue = toRaw(value);`：使用 `toRaw` 方法将 `Proxy` 对象转换为原始数组。
3. **条件判断和赋值**：
   - 如果 `mode` 等于 `'multiple'` 或 `item.props.mode` 等于 `'tags'`，则 `params[name]` 被赋值为 `rawValue`。
   - 否则，继续使用原来的逻辑：`rawValue.length ? rawValue.join(',') : undefined`。

### 示例代码

```tsx
import { toRaw } from 'vue';

const params = {};
const name = 'ids';
const value = [1, 2, 3]; // 假设这是 Proxy(Array) 类型
const mode = 'multiple'; // 或者 'tags'
const item = { props: { mode: 'tags' } };

console.log(mode, value);

const rawValue = toRaw(value); // 获取原始数组

params[name] = (mode === 'multiple' || item.props.mode === 'tags')
    ? rawValue
    : rawValue.length ? rawValue.join(',') : undefined;

console.log(params); // 输出: { ids: [1, 2, 3] }

// 如果 mode 和 item.props.mode 都不符合条件
const mode2 = 'single';
const item2 = { props: { mode: 'single' } };

params[name] = (mode2 === 'multiple' || item2.props.mode === 'tags')
    ? rawValue
    : rawValue.length ? rawValue.join(',') : undefined;

console.log(params); // 输出: { ids: '1,2,3' }

// 如果 value 为空数组
const value2 = [];

const rawValue2 = toRaw(value2); // 获取原始数组

params[name] = (mode2 === 'multiple' || item2.props.mode === 'tags')
    ? rawValue2
    : rawValue2.length ? rawValue2.join(',') : undefined;

console.log(params); // 输出: { ids: undefined }
```

## 2. 使用 `JSON.parse(JSON.stringify(value))`

### 解释

这种方法通过序列化和反序列化来获取原始数组。虽然效率较低，但在某些情况下可以作为一种简单的解决方案。

### 示例代码

```tsx
console.log(mode, value);

const rawValue = JSON.parse(JSON.stringify(value)); // 获取原始数组

params[name] = (mode === 'multiple' || item.props.mode === 'tags)
    ? rawValue
    : rawValue.length ? rawValue.join(',') : undefined;
```

## 3. 使用 `Array.from(value)`

### 解释

`Array.from` 方法可以将类数组对象或可迭代对象转换为数组。对于 `Proxy` 对象，它也可以工作。

### 示例代码

```tsx
console.log(mode, value);

const rawValue = Array.from(value); // 获取原始数组

params[name] = (mode === 'multiple' || item.props.mode === 'tags)
    ? rawValue
    : rawValue.length ? rawValue.join(',') : undefined;
```

## 4. 使用扩展运算符 `...`

### 解释

扩展运算符可以将数组或类数组对象展开为新的数组。

### 示例代码

```tsx
console.log(mode, value);

const rawValue = [...value]; // 获取原始数组

params[name] = (mode === 'multiple' || item.props.mode === 'tags)
    ? rawValue
    : rawValue.length ? rawValue.join(',') : undefined;
```

## 5. 使用 `Reflect.getPrototypeOf` 和 `Object.getOwnPropertyNames`

### 解释

这种方法稍微复杂一些，但可以更深入地理解 `Proxy` 对象的结构。

### 示例代码

```tsx
console.log(mode, value);

const rawValue = Reflect.getPrototypeOf(value).getOwnPropertyNames(value).reduce((acc, key) => {
    acc[key] = value[key];
    return acc;
}, []); // 获取原始数组

params[name] = (mode === 'multiple' || item.props.mode === 'tags)
    ? rawValue
    : rawValue.length ? rawValue.join(',') : undefined;
```

## 总结

> 选择哪种方法取决于你的具体需求和性能考虑。在大多数情况下，`toRaw` 方法是最优选择。

- **`toRaw` 方法**：推荐使用，因为它是最直接和高效的方法。
- **`JSON.parse(JSON.stringify(value))`**：简单但效率较低。
- **`Array.from(value)`**：适用于大多数情况，简单易用。
- **扩展运算符 `...`**：简洁且易于理解。
- **`Reflect.getPrototypeOf` 和 `Object.getOwnPropertyNames`**：适用于需要深入了解 `Proxy` 结构的场景。