# Webpack基础配置与优化技巧总结

## 一、Webpack 4基础配置

### 1. 核心概念

- **模块打包**：处理JS/CSS/图片等资源依赖关系
- **入口(entry)**：默认`src/index.js`
- **输出(output)**：默认生成`dist/main.js`
- **Loader系统**：处理非JS文件转换
- **插件(plugins)**：扩展打包功能

### 2. 基础配置步骤

```javascript
// webpack.config.js
const path = require('path');

module.exports = {
  entry: './src/index.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js'
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ['style-loader', 'css-loader']
      },
      {
        test: /\.(png|jpg|gif)$/,
        use: ['file-loader']
      }
    ]
  },
  plugins: [
    new HtmlWebpackPlugin({ template: './src/index.html' })
  ]
};
```

### 3. 关键配置项

1. **Babel配置**
   - `@babel/core` + `babel-loader`
   - `@babel/preset-env`兼容语法
   - 配置`.babelrc`文件
2. **CSS处理**
   - `style-loader`+`css-loader`基础组合
   - `postcss-loader`自动添加前缀
   - `sass-loader`预处理支持
3. **图片处理**
   - `file-loader`文件复制
   - `url-loader`转base64（需设置limit参数）

### 4. 开发优化

- devServer配置

  ```javascript
  devServer: {
    contentBase: './dist',
    hot: true,
    port: 8080
  }
  ```

- **HMR热更新**

- SourceMap配置

  ```javascript
  devtool: 'cheap-module-source-map'
  ```

## 二、Webpack 5新特性

### 1. 重大改进

|       特性        |                  说明                   |
| :---------------: | :-------------------------------------: |
|     持久缓存      | 通过`cache: { type: 'filesystem' }`配置 |
|     资源模块      |     替代`file-loader`/`url-loader`      |
| Module Federation |             实现微前端架构              |
| Tree Shaking增强  |       支持嵌套模块的无效代码消除        |

### 2. 配置升级示例

```javascript
// webpack5资源处理新方式
module: {
  rules: [
    {
      test: /\.(png|jpe?g)$/i,
      type: 'asset',
      parser: {
        dataUrlCondition: {
          maxSize: 8 * 1024 // 8kb
        }
      }
    }
  ]
}
```

### 3. 性能优化

1. **构建速度提升**
   - 持久缓存使二次构建速度提升90%
   - 默认开启文件缓存（`node_modules/.cache`）
2. **包体积优化**
   - 改进的Tree Shaking算法
   - 支持`sideEffects`标记
3. **新Loader机制**
   - 资源模块类型（asset/resource/inline/source）
   - 内置JSON解析

## 三、通用最佳实践

### 开发环境配置

1. 使用`webpack-merge`分离环境配置

2. 配置环境变量

   ```javascript
   new webpack.DefinePlugin({
     'process.env.NODE_ENV': JSON.stringify('development')
   })
   ```

### 生产环境优化

1. 代码压缩

   ```javascript
   optimization: {
     minimize: true,
     minimizer: [new TerserPlugin()],
   }
   ```

2. 代码分割

   ```javascript
   splitChunks: {
     chunks: 'all'
   }
   ```

### 调试配置

1. Source Map策略
   - 开发环境：`eval-cheap-source-map`
   - 生产环境：`hidden-source-map`