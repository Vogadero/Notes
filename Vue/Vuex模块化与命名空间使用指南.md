# Vuex模块化与命名空间使用指南

[Toc]

---

### 一、为什么需要模块化
1. **单一状态树限制**
   - Vuex使用单一状态树，所有状态集中在一个store对象
   - 复杂应用会导致store对象臃肿
2. **解决方案**
   - 将store分割为独立模块(module)
   - 每个模块可包含：
     - `state` | `mutations` | `actions` | `getters`
     - 嵌套子模块(modules)

---

### 二、模块化配置
1. **模块文件结构**
   ```bash
   store/
   ├── index.js       # 主入口文件
   └── modules/
       ├── countOPTIONS.js
       └── personOPTIONS.js

1. **模块示例配置(countOPTIONS.js)**

   ```javascript
   export default {
     namespaced: true,
     state: {
       sum: 0,
       school: '北京大学'
     },
     mutations: {
       ADD(state, value) {
         state.sum += value
       }
     },
     actions: {
       oddAdd({ state, commit }, value) {
         if (state.sum % 2) {
           commit('ODDADD', value)
         }
       }
     },
     getters: {
       bigSum(state) {
         return state.sum * 10
       }
     }
   }
   ```

2. **主store注册模块(index.js)**

   ```javascript
   import Vuex from 'vuex'
   import countOPTIONS from './modules/countOPTIONS'
   
   export default new Vuex.Store({
     modules: {
       countOPTIONS
     }
   })
   ```

------

### 三、命名空间(namespaced)

1. **开启方式**

   ```javascript
   export default {
     namespaced: true, // 关键配置
     // ...其他配置
   }
   ```

2. **命名空间作用**

   - 自动调整模块内成员的访问路径
   - 提高模块封装性和复用性
   - 解决不同模块的命名冲突

------

### 四、访问模块数据

1. 直接访问方式

   - State:

     ```javascript
     $store.state.模块名.属性名
     // 示例：$store.state.countOPTIONS.sum
     ```

   - Getters:

     ```javascript
     $store.getters['模块名/getter名']
     // 示例：$store.getters['countOPTIONS/bigSum']
     ```

   - Mutations/Actions:

     ```javascript
     // Mutation
     $store.commit('模块名/mutation名', payload)
     // Action
     $store.dispatch('模块名/action名', payload)
     ```

------

### 五、辅助函数用法

1. **基本引入**

   ```javascript
   import { mapState, mapActions, mapGetters, mapMutations } from 'vuex'
   ```

2. **mapState**

   ```javascript
   // 数组写法（推荐）
   computed: {
     ...mapState('countOPTIONS', ["sum", "student"])
   }
   
   // 对象写法
   ...mapState('countOPTIONS', {
     mySum: state => state.sum
   })
   ```

3. **mapGetters**

   ```javascript
   computed: {
     ...mapGetters('countOPTIONS', ["bigSum"])
   }
   ```

4. **mapMutations**

   ```javascript
   methods: {
     ...mapMutations('countOPTIONS', ["ADD", "SUBTRACT"])
   }
   ```

5. **mapActions**

   ```javascript
   methods: {
     ...mapActions('countOPTIONS', ["oddAdd", "waitAdd"])
   }
   ```

------

### 最佳实践建议

1. 始终启用`namespaced: true`
2. 优先使用数组形式辅助函数
3. 复杂模块建议采用对象解构参数
4. 避免超过3级模块嵌套
5. 统一模块文件命名规范（推荐`xxxModule.js`）

