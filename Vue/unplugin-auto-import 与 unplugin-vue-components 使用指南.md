# unplugin-auto-import 与 unplugin-vue-components 使用指南

## 插件简介
### 核心作用
- **unplugin-auto-import**  
  自动按需导入 JS/TS API（如 Vue 的 `ref`、`reactive`、第三方库方法等），减少手动导入代码量。
- **unplugin-vue-components**  
  自动按需解析并导入 Vue 组件（如 Element Plus 组件），避免全局注册或手动引入。

### 解决的问题
- 消除重复手动导入代码的繁琐操作。
- 优化项目体积（按需加载）。
- 支持 TypeScript 类型提示和 ESLint 校验。

---

## 安装与基础配置

### 安装依赖
```bash
pnpm add -D unplugin-auto-import unplugin-vue-components
# 或使用 npm
npm i -D unplugin-auto-import unplugin-vue-components
```

### 构建工具配置

#### Vite

```javascript
// vite.config.js
import AutoImport from "unplugin-auto-import/vite";
import Components from "unplugin-vue-components/vite";
import { ElementPlusResolver } from "unplugin-vue-components/resolvers";

export default {
  plugins: [
    AutoImport({
      resolvers: [ElementPlusResolver()],
    }),
    Components({
      resolvers: [ElementPlusResolver()],
    }),
  ],
};
```

#### Webpack

```javascript
// webpack.config.js
const AutoImport = require("unplugin-auto-import/webpack");
const Components = require("unplugin-vue-components/webpack");

module.exports = {
  plugins: [
    AutoImport({ /* 配置选项 */ }),
    Components({ /* 配置选项 */ }),
  ],
};
```

------

## 核心配置项解析

### AutoImport 配置

```javascript
AutoImport({
  // 自动导入规则
  imports: [
    // 预设（Vue、VueRouter、Pinia 等）
    "vue",
    "vue-router",
    // 自定义规则（以 axios 和 @vueuse/core 为例）
    {
      "axios": [["default", "axios"]], // 处理默认导出
      "@vueuse/core": ["useMouse", ["useFetch", "useMyFetch"]],
    },
  ],
  // 解析器（支持 UI 库如 Element Plus）
  resolvers: [ElementPlusResolver({ importStyle: "sass" })],
  // 生成 TypeScript 声明文件
  dts: "./auto-imports.d.ts",
  // 集成 ESLint
  eslintrc: {
    enabled: true,
    filepath: "./.eslintrc-auto-import.json",
    globalsPropValue: "readonly",
  },
});
```

### Components 配置

```javascript
Components({
  // 组件解析规则
  resolvers: [ElementPlusResolver()],
  // 生成组件类型声明文件
  dts: "./components.d.ts",
});
```

------

## 高级用法

### 自定义组件库支持

- 通过 `resolvers` 自定义解析函数，将组件名映射为导入路径。
- 示例：解析 `MyComponent` 到 `@/components/MyComponent.vue`。

### 处理默认导出库

- 对默认导出的库（如`axios`）需使用别名语法：

  ```javascript
  { "axios": [["default", "axios"]] }
  ```

### TypeScript 集成

1. 确保生成的`auto-imports.d.ts`和`components.d.ts`包含在`tsconfig.json`中：

   ```json
   {
     "include": ["auto-imports.d.ts", "components.d.ts"]
   }
   ```

### ESLint 集成

- 启用 `eslintrc.enabled` 生成全局变量声明文件。
- 在 ESLint 配置中引用生成的 JSON 文件。

------

## 注意事项

1. **路径匹配**
   使用 `include`/`exclude` 控制文件范围（默认包含 `.ts`、`.vue` 等）。
2. **缓存优化**
   配置 `cache: true` 加速重复构建（缓存路径可自定义）。
3. **Vue 模板支持**
   开启 `vueTemplate: true` 以支持模板内的自动导入。
4. **命名规范**
   组件名需使用 PascalCase（如 `MyComponent` 而非 `my-component`）。

---

> **提示**：实际配置需根据项目依赖和构建工具调整，建议参考官方文档 [unplugin-auto-import](https://github.com/antfu/unplugin-auto-import) 和 [unplugin-vue-components](https://github.com/antfu/unplugin-vue-components)。