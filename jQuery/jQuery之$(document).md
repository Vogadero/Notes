# jQuery之$(document)

- `$(document)`是一个选择器，选中的是整个html所有元素的集合。
- `$(document).ready()`里面的元素或事件都将会在DOM完成加载之后立即加载，并且在页面内容加载之前。

> ​	使用 `$(document).ready()`，你能让你的事件在window加载之前加载或触发。所有你写在这个方法里面的都准备在最早的时刻加载或触发。也就是说，一旦DOM在浏览器中注册后，`$(document).ready()`里的代码就开始执行。这样用户在第一眼看见页面元素时，特效就可以运行了。

- jquery中用on来绑定事件

```js
$(document).on('click','.classname',function(){});
$('.classname').on('click',function(){});
```

