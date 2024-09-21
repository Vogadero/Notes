# `for-in`循环和`for-of`循环

在JavaScript中，`for-in`循环和`for-of`循环都用于遍历数据结构，但它们的使用场景和表现有所不同。主要区别在于`for-in`循环用于遍历对象的可枚举属性（包括其原型链上的属性），而`for-of`循环则用于遍历可迭代对象（如数组、字符串、Map、Set等）的值。

### for-in 循环

`for-in`循环主要用于遍历对象的属性，包括其原型链上的可枚举属性。如果你只想遍历对象自身的属性，而不是原型链上的属性，可以使用`Object.hasOwnProperty()`方法。

#### 遍历对象

```javascript
const person = {
    firstName: "John",
    lastName: "Doe",
    age: 30,
    greet: function() {
        console.log(`Hello, my name is ${this.firstName} ${this.lastName}`);
    }
};

for (let key in person) {
    if (person.hasOwnProperty(key)) {
        console.log(key + ": " + person[key]);
    }
}
```

#### 遍历数组（不推荐）

虽然`for-in`循环也可以遍历数组，但通常不推荐这样做，因为它会遍历数组的所有可枚举属性，包括非数字索引的属性和原型链上的属性。此外，`for-in`不会保证属性的遍历顺序。

```javascript
const arr = [1, 2, 3];
arr.foo = "hello";

for (let key in arr) {
    if (arr.hasOwnProperty(key) && !isNaN(+key)) {
        console.log(arr[key]);
    }
}
```

### for-of 循环

`for-of`循环用于遍历可迭代对象的值，如数组、字符串、Map、Set等。它不会遍历对象的属性，而是直接遍历对象的值。

#### 遍历数组

```javascript
const arr = [1, 2, 3];

for (let value of arr) {
    console.log(value);
}
```

#### 遍历字符串

```javascript
const str = "Hello";

for (let char of str) {
    console.log(char);
}
```

#### 遍历Map和Set

```javascript
const map = new Map();
map.set('a', 1);
map.set('b', 2);

for (let [key, value] of map) {
    console.log(key + ': ' + value);
}

const set = new Set();
set.add(1);
set.add(2);

for (let value of set) {
    console.log(value);
}
```

总结：

- 使用`for-in`循环遍历对象的可枚举属性（包括原型链上的属性），如果要遍历对象自身的属性，需结合`hasOwnProperty()`方法。
- 使用`for-of`循环遍历可迭代对象的值，如数组、字符串、Map、Set等，直接获取对象的值，而不是属性名或键。