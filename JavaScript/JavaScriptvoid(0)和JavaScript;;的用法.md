# JavaScript:void(0)和JavaScript:;的用法

[TOC]

## 一、JavaScript:void(0)

- javascript:void(0) 中最关键的是 void 关键字， void 是 JavaScript 中非常重要的关键字，该操作符指定要计算一个表达式但是不返回值。
- JavaScript中void是一个操作符，该操作符指定要计算一个表达式但是不返回值。
- void 操作符用法格式如下：
  1. javascript:void (expression)
  2. javascript:void expression

- expression是一个要计算的 JavaScript 标准的表达式。表达式外侧的圆括号是可选的，但是写上去是一个好习惯。
- 我们可以使用void 操作符指定超级链接。表达式会被计算但是不会在当前文档处装入任何内容。
- 下面的代码创建了一个超级链接，当用户点击以后不会发生任何事。当用户点击链接时，void(0) 计算为 0，但在 JavaScript 上没有任何效果。也就是说，要执行某些处理，但是不整体刷新页面的情况下，可以使用void(0),但是在需要对页面进行refresh的情况下，那就要仔细了。

```
<a href="javascript:void(0)">单击此处什么也不会发生</a>
```

- 下面的代码创建了一个超级链接，用户单击时会提交表单。

```html
<a href="javascript:void(document.form.submit())">单此处提交表单</a>
```

- 其实我们可以这样用`<a href="javascript:void(document.form.submit())">`，这句话会进行一次submit操作。那什么情况下用void(0)比较多呢，无刷新，当然是Ajax了，看一下Ajax的web页面的话，一般都会看到有很多的void(0)，所以在使用void(0)之前,最好先想一想,这个页面是否需要整体刷新。

## 二、`<a>`标签中href="javascript:;"

- `<a>` 标签的 href 属性用于指定超链接目标的 URL，href 属性的值可以是任何有效文档的相对或绝对 URL，包括片段标识符和 JavaScript 代码段。这里的href="javascript:;"，其中javascript:是伪协议，它可以让我们通过一个链接来调用javascript函数.而采用这个方式 javascript:;可以实现A标签的点击事件运行时，如果页面内容很多，有滚动条时，页面不会乱跳，用户体验更好。
- javascript:是表示在触发`<a>`默认动作时，执行一段JavaScript代码，而 javascript:; 表示什么都不执行，这样点击`<a>`时就没有任何反应。
- javascript:;表示这是一个空连接。点击之后没任何反应。
- 类似的是#，但是一个#点击之后页面很长的情况下会滚到顶部；而javascript:;没这样的问题；当然###这样的效果就跟javascript:;一样了

## 三、href="#"与href="javascript:void(0)"的区别

- **#** 包含了一个位置信息，默认的锚是**#top** 也就是网页的上端。而**javascript:void(0)**, 仅仅表示一个死链接。在页面很长的时候会使用 **#** 来定位页面的具体位置，格式为：**# + id**。如果你要定义一个死链接请使用 javascript:void(0) 。

- 在做页面时，如果想做一个链接点击后不做任何事情，或者响应点击而完成其他事情，可以设置其属性 href = "#"，但是，这样会有一个问题，就是当页面有滚动条时，点击后会返回到页面顶端，用户体验不好。 

- 目前有如下几种解决办法： 

  1. 点击链接后不做任何事情 

     ```html
     <a href="javascript:void(0);" >test</a>   
     <a href="javascript:;" >test</a>   
     <a href="####" >test</a> //使用2个到4个#，见的大多是"####"，也有使用"#all"等其他的 
     ```

  2. 点击链接后，响应用户自定义的点击事件 

     ```html
     <a href="javascript:void(0)" onclick="doSomething()">test</a>   
     <a href="#" onclick="doSomething();return false;">什么问题都解决了,包括浏览器不兼容问题</a> //或者直接使用href=""   
     <a href="#" onclick="alert();event.returnValue=false;">test</a>  
     ```

- 说明： 

  1. javascript:void(0)这种伪协议，少写的好，如果你看过一些web标准的书就知道为什么了。（不懂，原话摘的，暂做记录） 

  2. 链接（href）直接使用javascript:void(0)在IE中可能会引起一些问题，比如：造成gif动画停止播放等，所以，最安全的办法还是使用“####”。为防止点击链接后跳转到页首，onclick事件return false即可。 

  3. 如果仅仅是想鼠标移过，变成手形，可以使用

     ```html
     <span style="cursor:pointer" onclick="foo()">Click Me!</span>  
     ```

- void是javascript的操作符，意思是：只执行表达式，但没有返回值， 

- void 操作符用法格式如下： 

  ```html
  javascript:void (expression)   
  javascript:void expression  
  ```

- 为了程序风格良好，建议使用第二种带上括号的 
- 我们可以使用void操作符指定超级链接，如`javascript:void(document.form.submit())`。表达式会被计算但是不会在当前文档处装入任何内容，void(0)计算为0，但在JavaScript上没有任何效果，也就是说 `<a href="javascript:void(0)">`的效果同`<a href="javascript:void(1)">`的效果是一样的。 
- 关键是只要知道void是javascipt自身的操作符，它表示的是只执行表达式，但没有返回值！ 
- 另外页面会自动调回顶端，是因为"#"默认的瞄点位置是top，所以会出现这种情况。