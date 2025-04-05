# Vite 环境变量使用与配置总结

## 一、基本概念与核心机制
### 1.1 环境变量定义
- **定义**：根据代码运行环境变化的全局变量（如生产/开发环境接口地址不同）
- **Webpack vs Vite**：
  - Webpack：通过`process.env`在浏览器中访问Node环境变量
  - Vite：浏览器直接运行代码，使用`import.meta.env`暴露环境变量

### 1.2 Vite 内建变量
```javascript
import.meta.env.MODE    // 当前模式（development/production）
import.meta.env.BASE_URL // 部署基础路径
import.meta.env.PROD    // 是否生产环境（布尔值）
import.meta.env.DEV     // 是否开发环境（布尔值）
import.meta.env.SSR     // 是否服务端渲染（布尔值）
```

## 二、环境变量配置机制

### 2.1 配置文件加载规则

|   文件类型    |      加载条件       | 优先级 |
| :-----------: | :-----------------: | :----: |
|    `.env`     |    所有环境加载     |  最低  |
| `.env.[mode]` | 匹配运行模式时加载  |  最高  |
| `.env.local`  | 本地覆盖（git忽略） |  次高  |

**加载顺序**：`.env` → `.env.[mode]` → `.env.local` → `.env.[mode].local`

### 2.2 变量暴露规则

- **客户端可见**：仅以`VITE_`开头的变量会暴露给浏览器端
- **服务端可见**：所有环境变量可在`vite.config.js`中访问

### 2.3 模式配置

```json
// package.json
{
  "scripts": {
    "dev": "vite",                // 默认加载development
    "build": "vite build",        // 默认加载production 
    "test": "vite --mode test"    // 自定义模式
  }
}
```

## 三、进阶配置技巧

### 3.1 自定义配置文件路径

```javascript
// vite.config.js
export default defineConfig({
  envDir: 'config/envs'  // 所有.env文件存放在项目根目录/config/envs下
})
```

### 3.2 修改变量前缀

```javascript
// vite.config.js
export default defineConfig({
  envPrefix: ['CUSTOM_', 'VITE_']  // 支持多前缀配置
})
```

### 3.3 服务端访问变量

```javascript
// vite.config.js
import { defineConfig, loadEnv } from 'vite'

export default defineConfig(({ mode }) => {
  const env = loadEnv(mode, process.cwd(), '') // 加载所有环境变量
  console.log(env.SECRET_KEY) // 访问非VITE_前缀变量
  return { /* config */ }
})
```

## 四、安全注意事项

1. **敏感信息保护**：非`VITE_`前缀变量不会暴露到客户端
2. **空前缀禁止**：`envPrefix`不可设为空字符串，避免信息泄露
3. **本地覆盖**：`.env.local`文件应加入`.gitignore`

## 五、常见问题解决方案

### 5.1 浏览器环境变量未生效

- 检查变量前缀是否为`VITE_`
- 确认运行模式匹配`.env.[mode]`文件名
- 验证配置文件加载路径是否正确

### 5.2 服务端获取变量异常

```javascript
// 正确访问方式
const env = loadEnv(process.env.NODE_ENV, process.cwd(), 'VITE_')
console.log(env.VITE_API_URL)
```

### 5.3 多环境管理方案

```shell
.env.staging
.env.uat
.env.preview
```

通过`--mode`参数指定不同环境配置

## 六、最佳实践建议

1. **命名规范**：使用`VITE_APP_[NAME]`格式保持一致性
2. **类型转换**：手动处理布尔值/数值类型转换
3. **文档维护**：创建`.env.example`文件说明配置项
4. **环境隔离**：开发/生产环境使用完全独立的配置集

> 注：本总结基于Vite 4.x版本，具体实现细节请以[Vite官方文档](https://vitejs.dev/guide/env-and-mode.html)为准。