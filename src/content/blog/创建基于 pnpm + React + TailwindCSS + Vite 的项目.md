---
title: 创建基于 pnpm + React + TailwindCSS + Vite 的项目
date: 2024-08-27 15:24:10
categories: Code
tags:
  - React
  - Vite
  - TailwindCSS
  - pnpm
id: pnpm-React-TailwindCSS-Vite
# cover: https://i0.wp.com/uxiaohan.github.io/v2/2024/08/1724744026.webp
recommend: true
---

:::note
一步步指导你如何创建基于 pnpm + React + TailwindCSS + Vite 的项目
:::

## 1. 创建项目

```bash
# 使用 Vite 创建 React 项目
pnpm create vite my-react-app --template react-ts

# 进入项目目录
cd my-react-app
```

## 2. 安装 TailwindCSS 及相关依赖

```bash
# 安装 TailwindCSS 及其相关依赖
pnpm add -D tailwindcss postcss autoprefixer

# 初始化 TailwindCSS 配置文件
npx tailwindcss init -p
```

## 3. 配置 TailwindCSS

编辑 `tailwind.config.js` 文件：

```javascript
/** @type {import('tailwindcss').Config} */
export default {
  content: [
    "./index.html",
    "./src/**/*.{js,ts,jsx,tsx}",
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

创建或编辑 `src/index.css` 文件：

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

## 4. 更新项目文件

确保 `src/main.tsx` (或 `src/main.jsx`) 导入 CSS 文件：

```javascript
import React from 'react'
import ReactDOM from 'react-dom/client'
import App from './App.tsx'
import './index.css' // 确保这行存在

ReactDOM.createRoot(document.getElementById('root')!).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
)
```

## 5. 修改 App 组件测试 TailwindCSS

编辑 `src/App.tsx`：

```tsx
function App() {
  return (
    <div className="min-h-screen bg-gray-100 p-8">
      <h1 className="text-3xl font-bold text-blue-600 mb-4">
        Welcome to React with TailwindCSS!
      </h1>
      <p className="text-gray-700">
        This is a starter template using Vite, React, TypeScript and TailwindCSS.
      </p>
    </div>
  )
}

export default App
```

## 6. 添加常用开发依赖 (可选)

```bash
# 添加 React Router
pnpm add react-router-dom

# 添加图标库 (如 Heroicons)
pnpm add @heroicons/react

# 添加类名处理工具
pnpm add clsx

# 添加动画库
pnpm add framer-motion
```

## 7. 运行项目

```bash
pnpm dev
```

## 8. 项目结构建议

```
my-react-app/
├── node_modules/
├── public/
├── src/
│   ├── assets/
│   ├── components/
│   ├── hooks/
│   ├── pages/
│   ├── styles/
│   ├── utils/
│   ├── App.tsx
│   ├── main.tsx
│   └── index.css
├── index.html
├── package.json
├── pnpm-lock.yaml
├── postcss.config.js
├── tailwind.config.js
└── vite.config.ts
```

## 9. 额外配置 (可选)

在 `vite.config.ts` 中添加路径别名：

```typescript
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import path from 'path'

export default defineConfig({
  plugins: [react()],
  resolve: {
    alias: {
      '@': path.resolve(__dirname, './src'),
    },
  },
})
```

然后在 `tsconfig.json` 中添加：

```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"]
    }
  }
}
```

这样你就设置好了一个现代化的 React 开发环境，包含了：
- pnpm 作为包管理器
- Vite 作为构建工具
- React 作为 UI 框架
- TypeScript 作为类型系统
- TailwindCSS 作为样式解决方案