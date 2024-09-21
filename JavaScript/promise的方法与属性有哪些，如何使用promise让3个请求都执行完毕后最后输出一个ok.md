## promise的方法与属性有哪些，如何使用promise让3个请求都执行完毕后最后输出一个ok

Promise 是 JavaScript 中用于异步操作的对象，它代表了一个最终可能完成（fulfilled）或失败（rejected）的异步操作及其结果值。Promise 对象有以下主要方法和属性：

### 方法

1. **`Promise.all(iterable)`**
   - 接受一个 promise 数组作为参数，当所有的 promise 都成功完成时，它返回一个新的 promise，这个新的 promise 会把数组中的值作为单个数组元素返回。如果任何一个 promise 失败，则返回的 promise 会立即失败，并且它的返回值会传递给返回的 promise 的失败回调。

2. **`Promise.race(iterable)`**
   - 返回一个 promise，这个 promise 会在它参数中的第一个 promise 完成时完成，无论这个 promise 是成功完成还是失败完成。

3. **`Promise.reject(reason)`**
   - 返回一个以特定原因拒绝的 Promise。

4. **`Promise.resolve(value)`**
   - 返回一个以给定值解析后的 Promise。如果这个值是 promise，那么返回这个 promise；否则，返回一个新的、以给定值解析的 promise。

5. **`catch(onRejected)`**
   - 方法返回一个 promise，并且处理拒绝的情况。

6. **`then(onFulfilled, onRejected)`**
   - 方法返回一个新的 promise，这个 promise 会在当前 promise 解决（fulfilled）或拒绝（rejected）后解决。

7. **`finally(onFinally)`**
   - 方法返回一个 promise，这个 promise 在原始 promise 被解决（fulfilled）或拒绝（rejected）后，以相同的解决值或拒绝理由被解决。它允许你在 promise 被解决或拒绝之后执行一些清理工作，而不论 promise 的最终状态如何。

### 使用 Promise.all 让 3 个请求都执行完毕后最后输出一个 ok

假设你有三个异步请求函数 `fetchData1()`, `fetchData2()`, `fetchData3()`，它们都返回一个 promise，你可以这样使用 `Promise.all` 来确保它们都完成后输出 "ok"：

```javascript
function fetchData1() {
  return new Promise((resolve, reject) => {
    // 模拟异步操作
    setTimeout(() => {
      resolve('数据1');
    }, 1000);
  });
}

function fetchData2() {
  return new Promise((resolve, reject) => {
    // 模拟异步操作
    setTimeout(() => {
      resolve('数据2');
    }, 1500);
  });
}

function fetchData3() {
  return new Promise((resolve, reject) => {
    // 模拟异步操作
    setTimeout(() => {
      resolve('数据3');
    }, 800);
  });
}

Promise.all([fetchData1(), fetchData2(), fetchData3()])
  .then(results => {
    // 所有请求都已完成，results 是包含三个请求结果的数组
    console.log(results); // 输出: ['数据1', '数据2', '数据3']
    console.log('ok'); // 最后输出 ok
  })
  .catch(error => {
    // 如果有任何一个请求失败，则会执行到这里
    console.error('请求失败:', error);
  });
```

这样，`Promise.all` 会等待所有提供的 promise 完成，如果都成功完成，则它的 `.then()` 方法会被调用，并接收到一个包含所有 promise 结果的数组。如果有任何一个 promise 失败，则 `.catch()` 方法会被调用，并接收到那个失败 promise 的拒绝原因。