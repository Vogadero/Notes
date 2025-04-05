# Vue CLI 3 ESLint 配置指南：解决 "no-undef" 错误及规则详解

## 一、问题背景
**错误现象**：  
在 Vue CLI 3 项目中遇到 ESLint 报错 `no-undef`，表示代码中存在未定义变量。

**根本原因**：  
ESLint 的严格模式会检测所有未通过 `var`/`let`/`const` 声明或不在全局变量列表中的标识符。

---

## 二、解决方案

### 方法 1：禁用 no-undef 规则（不推荐）
```javascript
// .eslintrc.js
module.exports = {
  rules: {
    'no-undef': 'off' // 0
  }
}
```

### 方法 2：声明全局变量（推荐）

```javascript
// .eslintrc.js
module.exports = {
  globals: {
    $: "readonly",
    _: "readonly",
    Vue: "readonly"
  }
}
```

------

## 三、ESLint 规则分类详解

### 变量相关规则

|     规则名称     | 推荐值 |            说明            |
| :--------------: | :----: | :------------------------: |
|    `no-undef`    | error  |       禁止未声明变量       |
| `no-unused-vars` | error  |       禁止未使用变量       |
|   `no-shadow`    | error  | 禁止变量覆盖外部作用域变量 |

### 代码质量规则

|   规则名称    | 推荐值 |         说明          |
| :-----------: | :----: | :-------------------: |
| `no-console`  |  warn  | 开发环境允许 console  |
| `no-debugger` | error  | 禁止生产环境 debugger |
|   `eqeqeq`    | error  |  强制使用 === 和 !==  |

### 代码风格规则

| 规则名称 | 推荐值 |      说明       |
| :------: | :----: | :-------------: |
|  `semi`  | error  |  强制分号结尾   |
| `quotes` | error  | 使用单引号（'） |
| `indent` | error  |   4 空格缩进    |

### Vue 专项规则

```javascript
// 需安装 eslint-plugin-vue
module.exports = {
  extends: ['plugin:vue/recommended']
}
```

------

## 四、最佳实践建议

1. **优先声明全局变量**
   通过 `globals` 配置明确声明第三方库或环境变量，而非直接禁用检测

2. **渐进式配置策略**

   ```javascript
   rules: {
     'no-console': process.env.NODE_ENV === 'production' ? 'error' : 'warn'
   }
   ```

3. **扩展推荐配置**

   ```javascript
   extends: [
     'eslint:recommended',
     'plugin:vue/essential',
     '@vue/standard'
   ]
   ```

4. **自动修复功能**

   ```bash
   vue-cli-service lint --fix
   ```

------

## 五、注意事项

1. 修改配置后需**重启 IDE** 才能生效

2. 慎用 `"off"`（0）完全禁用规则，建议使用 `"warn"`（1）

3. 对于浏览器环境变量，推荐配置：

   javascript

   复制

   ```javascript
   env: {
     browser: true,
     node: true
   }
   ```

> 完整配置参考：[ESLint 官方规则文档](https://eslint.org/docs/latest/rules/)