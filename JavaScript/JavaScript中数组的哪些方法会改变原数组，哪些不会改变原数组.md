# JavaScript中数组的哪些方法会改变原数组，哪些不会改变原数组

在JavaScript中，数组的方法可以分为两大类：

- 会改变原数组的方法（称为“变异方法”）和不会改变原数组的方法（称为“非变异方法”）。

### 会改变原数组的方法

1. **`push()`** - 在数组的末尾添加一个或多个元素，并返回新的长度。
   ```javascript
   let arr = [1, 2, 3];
   arr.push(4);
   console.log(arr); // 输出: [1, 2, 3, 4]
   ```

2. **`pop()`** - 删除数组的最后一个元素，并返回那个元素。
   ```javascript
   let arr = [1, 2, 3];
   arr.pop();
   console.log(arr); // 输出: [1, 2]
   ```

3. **`shift()`** - 删除数组的第一个元素，并返回那个元素。
   ```javascript
   let arr = [1, 2, 3];
   arr.shift();
   console.log(arr); // 输出: [2, 3]
   ```

4. **`unshift()`** - 在数组的开头添加一个或多个元素，并返回新的长度。
   ```javascript
   let arr = [2, 3];
   arr.unshift(1);
   console.log(arr); // 输出: [1, 2, 3]
   ```

5. **`splice()`** - 通过删除或替换现有元素或者原地添加新的元素来修改数组，并以数组形式返回被修改的内容。
   ```javascript
   let arr = [1, 2, 3, 4];
   arr.splice(2, 1, 'a', 'b');
   console.log(arr); // 输出: [1, 2, 'a', 'b', 4]
   ```

6. **`sort()`** - 对数组的元素进行排序，并返回数组。
   ```javascript
   let arr = [3, 1, 4, 1, 5, 9];
   arr.sort();
   console.log(arr); // 输出: [1, 1, 3, 4, 5, 9]
   ```

7. **`reverse()`** - 颠倒数组中元素的顺序，并返回数组。
   ```javascript
   let arr = [1, 2, 3];
   arr.reverse();
   console.log(arr); // 输出: [3, 2, 1]
   ```

8. **`copyWithin()`** - 浅复制数组的一部分到同一数组中的另一个位置，并返回它，不改变原数组的长度。
   ```javascript
   let arr = [1, 2, 3, 4, 5];
   arr.copyWithin(0, 3);
   console.log(arr); // 输出: [4, 5, 3, 4, 5]
   ```

9. **`fill()`** - 用一个固定值来填充一个数组中从起始索引到终止索引内的全部元素，不包括终止索引。
   ```javascript
   let arr = [1, 2, 3, 4];
   arr.fill('a');
   console.log(arr); // 输出: ['a', 'a', 'a', 'a']
   ```

### 不会改变原数组的方法

1. **`slice()`** - 返回一个新的数组对象，这一对象是一个由 `begin` 和 `end` 决定的原数组的浅拷贝（包括 `begin`，不含 `end`）。
   ```javascript
   let arr = [1, 2, 3, 4];
   let newArr = arr.slice(1, 3);
   console.log(newArr); // 输出: [2, 3]
   console.log(arr); // 输出: [1, 2, 3, 4]
   ```

2. **`map()`** - 创建一个新数组，其结果是该数组中的每个元素是调用一次提供的函数后的返回值。
   ```javascript
   let arr = [1, 2, 3];
   let newArr = arr.map(x => x * 2);
   console.log(newArr); // 输出: [2, 4, 6]
   console.log(arr); // 输出: [1, 2, 3]
   ```

3. **`filter()`** - 创建一个新数组，其包含通过所提供函数实现的测试的所有元素。
   ```javascript
   let arr = [1, 2, 3];
   let newArr = arr.filter(x => x > 1);
   console.log(newArr); // 输出: [2, 3]
   console.log(arr); // 输出: [1, 2, 3]
   ```

4. **`concat()`** - 用于合并两个或多个数组。此方法不会改变现有数组，而仅仅会返回被合并数组的一个副本。
   ```javascript
   let arr1 = [1, 2, 3];
   let arr2 = [4, 5, 6];
   let newArr = arr1.concat(arr2);
   console.log(newArr); // 输出: [1, 2, 3, 4, 5, 6]
   console.log(arr1); // 输出: [1, 2, 3]
   ```

5. **`flat()`** - 按照一个可指定的深度递归遍历数组，并将所有元素与遍历到的子数组中的元素合并为一个新数组返回。
   ```javascript
   let arr = [1, [2, 3], [4, [5, 6]]];
   let newArr = arr.flat(2);
   console.log(newArr); // 输出: [1, 2, 3, 4, 5, 6]
   console.log(arr); // 输出: [1, [2, 3], [4, [5, 6]]]
   ```

6. **`flatMap()`** - 首先使用映射函数映射每个元素，然后将结果压缩成一个新数组。它相当于 `map()` 接着 `flat()` 的效果。
   ```javascript
   let arr = [1, 2, 3, 4];
   let newArr = arr.flatMap(x => [x, x * 2]);
   console.log(newArr); // 输出: [1, 2, 2, 4, 3, 6, 4, 8]
   console.log(arr); // 输出: [1, 2, 3, 4]
   ```

7. **`every()`** - 测试数组的所有元素是否都通过了指定函数的测试。它返回一个布尔值。
   ```javascript
   let arr = [1, 2, 3];
   console.log(arr.every(x => x > 0)); // 输出: true
   console.log(arr); // 输出: [1, 2, 3]
   ```

8. **`some()`** - 测试数组中是不是至少有1个元素通过了被提供的函数测试。它返回一个布尔值。
   ```javascript
   let arr = [1, 2, 3];
   console.log(arr.some(x => x > 2)); // 输出: true
   console.log(arr); // 输出: [1, 2, 3]
   ```

9. **`reduce()`** - 对数组中的每个元素执行一个由您提供的reducer函数(升序执行)，将其结果汇总为单个返回值。
   ```javascript
   let arr = [1, 2, 3, 4];
   let result = arr.reduce((acc, val) => acc + val, 0);
   console.log(result); // 输出: 10
   console.log(arr); // 输出: [1, 2, 3, 4]
   ```

10. **`reduceRight()`** - 接受一个函数作为累加器，数组中的每个值（从右到左）开始缩减，最终计算为一个值。
    ```javascript
    let arr = [1, 2, 3, 4];
    let result = arr.reduceRight((acc, val) => acc + val, 0);
    console.log(result); // 输出: 10
    console.log(arr); // 输出: [1, 2, 3, 4]
    ```

11. **`find()`** - 返回数组中满足提供的测试函数的第一个元素的值。否则返回 `undefined`。
    ```javascript
    let arr = [1, 2, 3, 4];
    console.log(arr.find(x => x > 2)); // 输出: 3
    console.log(arr); // 输出: [1, 2, 3, 4]
    ```

12. **`findIndex()`** - 返回数组中满足提供的测试函数的第一个元素的索引。否则返回-1。这个方法不会改变原数组。

       ```javascript
       let arr = [1, 2, 3, 4];
       console.log(arr.findIndex(x => x > 2)); // 输出: 2
       console.log(arr); // 输出: [1, 2, 3, 4]
       ```

13. **`toString()`** - 返回一个字符串，表示指定的数组及其元素。

    ```javascript
    let arr = [1, 2, 3];
    console.log(arr.toString()); // 输出: "1,2,3"
    console.log(arr); // 输出: [1, 2, 3]
    ```

14. **`toLocaleString()`** - 返回一个字符串，表示指定的数组及其元素，该字符串是根据本地环境创建的。

    ```javascript
    let arr = [1, 2, 3];
    console.log(arr.toLocaleString()); // 输出可能因地区而异，例如在美国可能是 "1,2,3"
    console.log(arr); // 输出: [1, 2, 3]
    ```

15. **`join()`** - 将数组的所有元素连接成一个字符串。

    ```javascript
    let arr = [1, 2, 3];
    console.log(arr.join('-')); // 输出: "1-2-3"
    console.log(arr); // 输出: [1, 2, 3]
    ```

16. **`indexOf()`** - 返回在数组中可以找到给定元素的第一个索引，如果不存在，则返回-1。

    ```javascript
    let arr = [1, 2, 3];
    console.log(arr.indexOf(2)); // 输出: 1
    console.log(arr); // 输出: [1, 2, 3]
    ```

17. **`lastIndexOf()`** - 返回在数组中可以找到给定元素的最后一个索引，如果不存在，则返回-1。

    ```javascript
    let arr = [1, 2, 3, 2];
    console.log(arr.lastIndexOf(2)); // 输出: 3
    console.log(arr); // 输出: [1, 2, 3, 2]
    ```

18. **`forEach()`** - 对数组的每个元素执行一次提供的函数，但它不会改变原数组（尽管回调函数内部可以执行改变原数组的操作）。

    ```javascript
    let arr = [1, 2, 3];
    arr.forEach(element => console.log(element));
    // 输出:
    // 1
    // 2
    // 3
    console.log(arr); // 输出: [1, 2, 3]
    ```

19. **`entries()`** - 返回一个新的Array Iterator对象，该对象包含数组中每个索引的键/值对。

    ```javascript
    let arr = ['a', 'b', 'c'];
    let iterator = arr.entries();
    console.log(iterator.next().value); // 输出: [0, 'a']
    console.log(arr); // 输出: ['a', 'b', 'c']
    ```

20. **`keys()`** - 返回一个新的Array Iterator对象，该对象包含数组中每个索引的键。

    ```javascript
    let arr = ['a', 'b', 'c'];
    let iterator = arr.keys();
    console.log(iterator.next().value); // 输出: 0
    console.log(arr); // 输出: ['a', 'b', 'c']
    ```

21. **`values()`** - 返回一个新的Array Iterator对象，该对象包含数组中每个索引的值。

    ```javascript
    let arr = ['a', 'b', 'c'];
    let iterator = arr.values();
    console.log(iterator.next().value); // 输出: 'a'
    console.log(arr); // 输出: ['a', 'b', 'c']
    ```

    