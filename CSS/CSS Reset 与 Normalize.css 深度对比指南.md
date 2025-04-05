# CSS Reset 与 Normalize.css 深度对比指南

## 一、核心概念解析

### 1. Reset CSS
#### 定义与目标
- **核心理念**：强制清零浏览器默认样式
- **核心代码示例**：
```css
html, body, div,... /* 80+元素选择器 */ {
    margin: 0;
    padding: 0;
    border: 0;
    font-size: 100%;
    font: inherit;
    vertical-align: baseline;
}
```

#### 关键特性

- 清除所有元素的`margin/padding/border`
- 统一字体大小为100%
- 消除列表样式（`list-style: none`）
- 表格边框合并优化

### 2. Normalize.css

#### 设计哲学

- **核心目标**：修复跨浏览器渲染差异
- **版本特性**：v8.0.1 包含100+修复项

#### 典型修复案例

```css
/* 修复iOS字体缩放 */
html {
    -webkit-text-size-adjust: 100%;
}

/* 统一hr样式 */
hr {
    box-sizing: content-box;
    height: 0;
    overflow: visible;
}

/* 修正表单控件继承 */
button, input {
    overflow: visible;
    font-family: inherit;
}
```

## 二、技术方案对比矩阵

|     对比维度     |       Reset CSS        |       Normalize.css       |
| :--------------: | :--------------------: | :-----------------------: |
| **样式处理方式** |    清零所有默认样式    |   修复并保留合理默认值    |
|   **代码体积**   |   约300行（未压缩）    |   约400行（含详细注释）   |
|  **浏览器支持**  |     所有现代浏览器     | 精确适配各版本浏览器特性  |
|   **可定制性**   |   需完全重新定义样式   |    支持模块化按需引入     |
|   **维护成本**   | 高（需持续覆盖新元素） |    低（官方持续更新）     |
|  **语义化支持**  |   破坏部分语义化样式   | 保持HTML5语义元素默认表现 |
| **典型应用场景** |   高度定制化视觉设计   | 跨平台一致性要求高的项目  |

## 三、技术实现差异详解

### 1. 排版系统处理

- **Reset方案**：

```css
body { line-height: 1; }
h1 { font-size: 100%; } /* 清除标题权重 */
```

- **Normalize方案**：

```css
html { line-height: 1.15; } /* 基线修正 */
h1 { font-size: 2em; margin: 0.67em 0; } /* 保留层级关系 */
```

### 2. 表单控件处理

- **Reset方案**：

```css
input, button { margin: 0; } /* 简单清零 */
```

- **Normalize方案**：

```css
[type="search"] {
    -webkit-appearance: textfield; /* 修复Safari样式 */
    outline-offset: -2px; /* 焦点轮廓修正 */
}
```

### 3. 兼容性处理

- **Normalize特有修复**：

```css
main { display: block; } /* IE兼容 */
template { display: none; } /* 现代浏览器隐藏 */
```

## 四、工程化选择建议

### 1. 选择 Reset CSS 当

- 设计系统与浏览器默认样式差异超过70%
- 需要支持IE8等老旧浏览器
- 项目有完整的UI规范体系

### 2. 选择 Normalize.css 当

- 需要保留语义化HTML结构
- 项目涉及复杂表单交互
- 需要支持多平台一致性（Web/移动端）

### 3. 混合方案推荐

```css
/* 现代项目推荐配置 */
@import "normalize.css";

/* 追加必要重置 */
:root {
    --base-font: system-ui;
    --line-height: 1.6;
}

* {
    box-sizing: border-box;
    text-decoration-skip-ink: auto;
}

svg:not([fill]) { 
    fill: currentColor; 
}
```

## 五、版本演进趋势

1. **Reset CSS 发展**
   - v2.0 → 增加HTML5元素支持
   - 未来 → 可能引入CSS变量配置
2. **Normalize.css 发展**
   - v8.x → 新增`details/summary`元素支持
   - 未来 → 集成CSS Grid修复

## 六、性能优化建议

```bash
# 按需引入模块（通过PostCSS）
npm install normalize.css --save
```

```css
/* 仅引入表单模块 */
@import "normalize.css/forms.css";
@import "normalize.css/typography.css";
```

## 七、调试技巧

1. **样式覆盖检测**

   ```js
   // 检测未重置元素
   Array.from(document.all).forEach(el => {
       const style = getComputedStyle(el);
       if(style.margin !== '0px') console.log(el);
   });
   ```

2. **层叠上下文分析**

```css
* { 
    outline: 1px solid rgba(255,0,0,0.1); 
}
```