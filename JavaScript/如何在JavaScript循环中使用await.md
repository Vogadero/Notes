# 如何在JavaScript循环中使用await

> await只能和async搭配一起使用，在浏览器控制台可以直接单写await，在编辑器写代码的时候一定要套一个async使用

### 在forEach循环中的await

1. JavaScript 中的 forEach不支持 promise 感知，也不支持 async 和await，所以不能在 forEach 使用 await 

2. map/forEach内部使用了while结合callback方式来执行函数，await不会等待callback的执行

3. forEach 只支持同步代码

4. map/forEach是简单的执行下回调函数，并不会处理异步的情况。即：map/forEach 会同时创建出多个回调函数，多个回调函数被加上了各自的 async、await。各个函数之间是独立的，彼此的回调也是独立的；请求是异步的，彼此之间又没有关联，顺序也就自然无法保证。

   ```javascript
   while(index < arr.length){
     callback(item, index)
   }
   
   async ()=>{
     await sleep(1000); 
   } 
   async ()=>{ 
     await sleep(1000);
   } 
   async ()=>{ 
     await sleep(1000);
   }
   ```

### 总结

- 对于普通的 for-loop，所有的 await 都是串行调用的，可以放心使用，包括 while、for-in、for-of 等等;但是在有 callback 的 array 方法，如 forEach、map、filter、reduce 等等，有许多副作用，最好就别使用 await 