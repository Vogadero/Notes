# CSS实现loading省略号动画

```html
<span class="loading">等待中</span>

<style>
    .loading {
      color: red;
    }
    .loading:after {
      overflow: hidden;
      display: inline-block;
      /* vertical-align: bottom; */
      animation: ellipsis 1.5s infinite;
      content: "\2026"; /* ascii code for the ellipsis character */
      /* \2026 == ... */ 
      /* content: "..." */
    }
    @keyframes ellipsis {
      from {
        width: 2px;
      }
      to {
        width: 15px;
      }
    }
</style>
```

