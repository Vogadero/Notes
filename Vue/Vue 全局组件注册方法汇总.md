# Vue 全局组件注册方法汇总

[Toc]

---

### Vue2 手动注册全局组件
**核心方法**：

```javascript
// main.js
import Vue from 'vue'
import ComponentName from '@/components/ComponentName.vue'

Vue.component('ComponentName', ComponentName)
```

**特点**：

- 直接在入口文件声明注册
- 适合少量组件注册
- 组件名需手动指定
- 代码重复率高，维护成本高

------

### Vue2 自动批量注册组件

**实现方案**：

```javascript
// baseComponents/index.js
const components = require.context('./', false, /\.vue$/)

export default Vue => {
  components.keys().forEach(fileName => {
    const componentName = fileName.replace(/^\.\//, '').replace(/\.\w+$/, '')
    Vue.component(componentName, components(fileName).default)
  })
}

// main.js
import baseComponents from './baseComponents'
Vue.use(baseComponents)
```

**关键点**：

- 使用 `require.context` 实现自动化
- 文件名即组件名（需规范命名）
- 支持热更新，新增组件无需修改代码
- 需遵循目录规范：`src/baseComponents/*.vue`

------

### Vue2 组件注册方法对比

|    方法     |   适用场景   | 维护成本 | 扩展性 |     典型应用     |
| :---------: | :----------: | :------: | :----: | :--------------: |
| component() | 单个组件注册 |    高    |   差   |   少量核心组件   |
|    use()    | 批量组件注册 |    低    |   好   | 组件库/插件系统  |
|  插件机制   |  企业级项目  |    中    |  优秀  | ElementUI 式注册 |

**插件式注册示例**：

```javascript
// components/index.js
export default {
  install(Vue) {
    Vue.component('ComponentA', ComponentA)
    Vue.component('ComponentB', ComponentB)
  }
}

// main.js
import Components from './components'
Vue.use(Components)
```

------

### Vue3 + Vite 自动注册方案

**实现方案**：

```typescript
// components/index.ts
const components = import.meta.glob('./*.vue')

export default {
  install(app) {
    Object.entries(components).forEach(([path, component]) => {
      const name = path.slice(path.lastIndexOf('/')+1, -4)
      app.component(name, defineAsyncComponent(component))
    })
  }
}

// main.ts
import MyGlobalCom from '@/components'
createApp(App).use(MyGlobalCom)
```

**技术要点**：

- 使用 Vite 的 `import.meta.glob`
- 支持 TypeScript 类型检查
- 异步组件加载（defineAsyncComponent）
- 文件名规范要求：`kebab-case` 命名

------

### 最佳实践建议

1. **版本适配**：

   - Vue2 项目推荐插件式批量注册
   - Vue3 项目优先使用 Vite 的自动注册方案

2. **命名规范**：

   - 组件文件采用 `kebab-case` 命名
   - 全局组件添加统一前缀（如 `BaseButton`）

3. **性能优化**：

   - 高频组件使用同步加载
   - 低频组件采用异步加载

4. **目录结构**：

   ```
   src/
   ├── components/
   │   ├── base/        # 全局基础组件
   │   │   ├── BaseButton.vue
   │   │   └── index.js
   │   └── features/   # 业务组件
   ```

5. **注意事项**：

   - 避免全局注册高频变动组件
   - 生产环境建议 Tree-Shaking
   - 类型声明文件需单独处理（TS 项目）