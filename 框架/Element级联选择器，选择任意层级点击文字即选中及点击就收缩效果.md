# Element级联选择器，选择任意层级点击文字即选中及点击就收缩效果

### 1. 点击文字即选中

```javascript
  mounted() {
          	  // 点击文字即选中
		this.timer =  setInterval(function() {
　　　　　　document.querySelectorAll(".el-cascader-node__label").forEach(el => {
　　　　　　　　el.onclick = function() {
　　　　　　　　　　if (this.previousElementSibling) this.previousElementSibling.click();
　　　　　　　　};
　　　　　　});
　　　　}, 1000);
    },
```

### 2. 点击就收缩效果

```vue
<el-cascader
    size='mini'
    :options="goodsOptions"
    v-model='goods_cid_list'
    :props="{ checkStrictly: true,
    value:'id',
    label:'name',
    children:'children'}"
    clearable
    @change='goodsOptionsChange'
    ref="cascaderHandle" 
    :show-all-levels="false" 
    filterable>
</el-cascader>
```

```vue
goodsOptionsChange(e){
	//监听值发生变化就关闭它
	this.$refs.cascaderHandle.dropDownVisible = false; 
	// 监听是否为最后一级，如果为最后一级，面板收起
	var children = this.$refs.cascaderHandle.getCheckedNodes();
     if(children[0].children.length < 1){   //判断有没有下级
        this.$refs.cascaderHandle.dropDownVisible = false; //监听值发生变化就关闭它
	  }
    },
```

### 3. 是否严格的遵守父子节点不互相关联(允许选择任意一级选项)

#### 3.1 允许选择任意一级选项

```vue
/* 指定级联选择器的配置对象 */
cascaderProps: {
    // 是否严格的遵守父子节点不互相关联,允许选择任意一级选项
    checkStrictly: true
},
```

```vue
// 在级联选择器change事件中,选中后关闭下拉框,监听值发生变化就关闭它
this.$refs.cascaderRef.dropDownVisible = false;
```

#### 3.2 不允许选择任意一级选项

```vue
/* 指定级联选择器的配置对象 */
cascaderProps: {
    // 是否严格的遵守父子节点不互相关联,不允许选择任意一级选项
    checkStrictly: false
},
```

```css
/* 级联选择器el-cascader，可以点击文字选择 */
.el-cascader-panel .el-radio {
    position: absolute;
    top: 10px;
    right: -10px;
    z-index: 10;

    width: 100%;
    height: 100%;
}

/* 隐藏单选框 */
.el-cascader-panel .el-radio__input {
    visibility: hidden;
}

/* 这个样式针对IE有用，不考虑IE的可以不用管*/
.el-cascader-panel .el-cascader-node__postfix {
    top: 10px;
}
```

```vue
// 在级联选择器change事件中,选中后关闭下拉框,监听值发生变化就关闭它
this.$refs.cascaderRef.dropDownVisible = false;
```

