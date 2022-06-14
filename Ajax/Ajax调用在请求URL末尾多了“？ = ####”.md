# Ajax调用在请求URL末尾多了“？ = ####”

1. 一个简单的ajax命令，它会调用服务器的URL：

   ```js
   $.ajax({ type: "GET", url: "/action" });
   ```

   

2. 后台浏览器报错，日志中的响应显示为 `/action?_=1423024004825`，为何多了`“？ = ####”`

3. `“？ = ####”`是缓存破坏者。 当您将缓存设置设为false时，会将其附加到查询字符串中以使其成为新字符串，以便浏览器认为这是一个新请求，而不使用响应的缓存版本。要阻止它被追加，只需将设置更改为true（现在将使用缓存的响应）

   ```js
   jQuery.ajaxSetup({cache:true});
   ```

   

4. 还可以通过将缓存设置添加到选项中，根据每个请求进行设置

   ```js
   jQuery.ajax({
       type: "GET", 
       url: "/action",
       cache:true 
   });
   ```

   

5. 参考：[http://api.jquery.com/jQuery.ajax](https://api.jquery.com/jQuery.ajax)

   - 缓存：默认值：true，
   - 对于 dataType = 'script' 和 dataType = 'jsonp' 为 false
   - 类型：布尔值
   - 如果设置为false，将强制浏览器不缓存请求的页面。 
   - 注意：将缓存设置为false只能与HEAD和GET请求一起正常使用。 它通过在GET参数后面附加`“ _ = {timestamp}”`来工作。 对于其他类型的请求，不需要此参数，但在IE8中，当对GET已经请求的URL进行POST时，则不需要该参数。