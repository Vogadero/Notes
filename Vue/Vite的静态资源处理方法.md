# Vite的静态资源处理方法

## 📌 **方法对比表**

| 特性            | `import.meta.env.BASE_URL + 路径`           | `import 图片 + 内联`                      |
| :-------------- | :------------------------------------------ | :---------------------------------------- |
| **资源位置**    | 必须放在 `public/` 目录                     | 推荐放在 `src/assets/` 目录               |
| **构建处理**    | 原样复制到 dist 根目录                      | 会被 Vite 优化处理 (压缩/哈希)            |
| **路径解析**    | 需手动拼接 BASE_URL                         | 自动处理路径 (支持别名 `@/assets/`)       |
| **缓存策略**    | 需手动添加时间戳参数                        | 自动添加哈希 (文件变化时自动更新)         |
| **子目录部署**  | 自动适配 (如 `/guess-pokemon/fallback.gif`) | 自动适配 (需正确配置 vite base)           |
| **网络请求**    | 额外 HTTP 请求                              | 可能内联为 Base64 (视文件大小)            |
| **TS 类型支持** | 需手动声明                                  | 自动生成类型声明 (需配置 `vite-env.d.ts`) |
| **适用场景**    | 频繁变更的第三方资源                        | 项目自有静态资源                          |

------

## 🖼️ **具体差异解析**

### 1. `import.meta.env.BASE_URL` 方式

```typescript
img.src = `${import.meta.env.BASE_URL}fallback-pokemon.gif`;
```

- **实现原理**
  Vite 的 [BASE_URL](https://vitejs.dev/guide/build.html#public-base-path) 会根据构建配置自动生成

- 文件结构要求

  ```json
  public/
    └─ fallback-pokemon.gif
  ```

- **构建结果**
  直接复制到 `dist/` 根目录，路径保持原样

- **访问地址**
  `https://vogadero.github.io/guess-pokemon/fallback-pokemon.gif`

### 2. `import` + 内联方式

```typescript
import fallbackGif from '@/assets/fallback-pokemon.gif?url';
// ...
img.src = fallbackGif;
```

- **实现原理**
  使用 Vite 的 [静态资源处理](https://vitejs.dev/guide/assets.html)

- 文件结构要求

  ```json
  src/
    └─ assets/
        └─ fallback-pokemon.gif
  ```

- **构建结果**
  生成带哈希的文件名：`assets/fallback-pokemon-abc123.gif`

- **访问地址**
  `https://vogadero.github.io/guess-pokemon/assets/fallback-pokemon-abc123.gif`

------

## ⚡ **性能对比**

| 指标           | BASE_URL 方式    | import 方式            |
| :------------- | :--------------- | :--------------------- |
| 首屏加载       | 需额外请求       | 可能内联减少请求       |
| 缓存利用率     | 依赖手动缓存破坏 | 自动哈希管理           |
| 维护成本       | 需手动管理路径   | 路径自动解析           |
| 项目结构规范性 | 较松散           | 符合标准前端工程化规范 |

------

## ✅ **推荐方案**

根据你的项目现状 (GitHub Pages 子目录部署)，建议采用 **混合方案**：

```typescript
// 1. 使用 import 方式引入
import fallbackGif from '@/assets/fallback-pokemon.gif?url';

// 2. 错误处理
const handleImageError = (e: Event) => {
  const img = e.target as HTMLImageElement;
  img.src = fallbackGif;
  img.onerror = null;
};
```

**配套配置**：

```typescript
// vite.config.ts
export default defineConfig({
  base: '/guess-pokemon/',
  // ...
});

// src/vite-env.d.ts (新建)
/// <reference types="vite/client" />
declare module '*.gif' {
  const src: string;
  export default src;
}
```

------

## ⚠️ **注意事项**

1. 确保图片小于 4KB 才会被自动内联为 Base64 (通过 `build.assetsInlineLimit` 配置)
2. 使用 `?url` 后缀显式声明需要获取文件 URL
3. 部署后检查 `dist/assets/` 目录是否包含处理后的图片文件

采用这种方案既能享受工程化带来的路径自动管理优势，又能完美适配子目录部署场景。