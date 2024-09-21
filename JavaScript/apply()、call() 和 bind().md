## `apply()`、`call()` 和 `bind()`

`apply()`、`call()` 和 `bind()` 是 JavaScript 中三个非常有用的函数，它们都用于改变函数运行时的作用域（即函数体内的 `this` 指向），但它们之间有一些关键的区别。

### 1. apply()

`apply()` 方法调用一个具有给定 `this` 值的函数，以及以一个数组（或类数组对象）的形式提供的参数。

**语法**：
```javascript
func.apply(thisArg, [argsArray])
```
- `thisArg`：在 `func` 函数运行时使用的 `this` 值。注意：在严格模式下，指定的 `this` 值不会被改变（即 `this` 始终是函数运行时所在的对象）。
- `argsArray`：一个数组或者类数组对象，其中的数组元素将作为单独的参数传给 `func` 函数。如果该参数的值为 `null` 或 `undefined`，则 `this` 会指向全局对象（在严格模式下为 `undefined`）。

**示例**：
```javascript
function greet(greeting, punctuation) {
    console.log(greeting + ', ' + this.name + punctuation);
}

const person = {
    name: 'John'
};

greet.apply(person, ['Hello', '!']); // 输出: Hello, John!
```

### 2. call()

`call()` 方法的作用和 `apply()` 方法类似，也是调用一个具有给定 `this` 值的函数，但 `call()` 方法接受的是参数列表，而不是单一的数组。

**语法**：
```javascript
func.call(thisArg, arg1, arg2, ...)
```
- `thisArg`：在 `func` 函数运行时使用的 `this` 值。
- `arg1, arg2, ...`：传递给函数的参数，这些参数将依次传递给 `func` 函数。

**示例**：
```javascript
function greet(greeting, punctuation) {
    console.log(greeting + ', ' + this.name + punctuation);
}

const person = {
    name: 'Jane'
};

greet.call(person, 'Hi', '.'); // 输出: Hi, Jane.
```

### 3. bind()

`bind()` 方法创建一个新的函数，在 `bind()` 被调用时，这个新函数的 `this` 被指定为 `bind()` 的第一个参数，而其余参数将作为新函数的参数，供调用时使用。

**语法**：
```javascript
const newFunc = func.bind(thisArg, arg1, arg2, ...)
```
- `thisArg`：当新函数被调用时，`this` 的值设置为 `thisArg`。
- `arg1, arg2, ...`：新函数的参数列表，这些参数在 `newFunc` 被调用时，会预设在 `func` 的参数列表中。

**示例**：
```javascript
function greet(greeting, punctuation) {
    console.log(greeting + ', ' + this.name + punctuation);
}

const person = {
    name: 'Alice'
};

const boundGreet = greet.bind(person, 'Hello');
boundGreet('!'); // 输出: Hello, Alice!
```

### 区别总结

- **`apply()`** 和 **`call()`** 都是直接调用函数，并允许你显式地设置函数体内的 `this` 值，以及传递参数（`apply()` 以数组形式，`call()` 以参数列表形式）。
- **`bind()`** 不会立即调用函数，而是返回一个新的函数，这个新函数的 `this` 被永久绑定到了 `bind()` 的第一个参数，而其余参数则作为新函数的预设参数，等待被调用。