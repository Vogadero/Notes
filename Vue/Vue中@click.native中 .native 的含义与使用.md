# Vue中@click.native中 .native 的含义与使用

> **.native--侦听组件根元素上的原生事件**
>
> **作用： 给组件绑定原生事件**

### 背景

- @click是我们在vue开发中经常用到的事件绑定，而@实际上是 `v-on` 的简写，而 `v-on` 则是对 vue 的事件体系封装之后的 API接口
- 也就是说，在处理DOM原生事件的场合中需要添加额外的标识符
- 比如：如果使用`router-link`标签，加上@click事件，绑定的事件会无效，因为`router-link`的作用是单纯的路由跳转，会阻止click事件，如果不加 **.native,** 事件是不会触发的，因此需要加上 **.native** 才会触发事件
- 当你给一个vue组件绑定事件的时候，要加上native，如果是普通的html元素，就不需要

```vue
<template>
    <div id="app">
        <Button @click.native = 'goToNext'>点击跳转</Button>
    </div>
</template>
<script>
import Button from '../components/Button'
export default{
    components:{
        Button
    },
    data(){
        return{
        
        }
    }
    methods:{
        goToNext(){
            alert('hello--world')
        }
    }    
}
</script>
```

