---
title: "边界 / Boundary 项目技术栈"
date: 2025-12-01
draft: false
tags: ["React", "Next.js", "TypeScript", "Electron", "AI", "游戏开发"]
categories: ["项目经验"]
summary: "AI 心理健康互动游戏的全栈技术架构，支持 Web、桌面端和移动端多平台。"
showToc: true
TocOpen: true
---

## 项目简介

「边界 / Boundary」是一款 **AI 心理健康互动游戏**，玩家可以与多个角色进行对话，体验不同的社交场景。项目采用现代全栈技术构建，支持 **Web**、**桌面端（Windows/Mac）** 和 **移动端（iOS/Android）** 多平台运行。

---

## 技术架构图

```
┌─────────────────────────────────────────────────────────────┐
│                      用户界面层 (UI)                         │
│  ┌─────────────┐ ┌─────────────┐ ┌─────────────────────────┐│
│  │  React 18   │ │ Tailwind CSS│ │     Framer Motion       ││
│  │  组件系统   │ │   样式框架   │ │       动画库            ││
│  └─────────────┘ └─────────────┘ └─────────────────────────┘│
├─────────────────────────────────────────────────────────────┤
│                    应用框架层 (Framework)                    │
│  ┌─────────────────────────────────────────────────────────┐│
│  │                    Next.js 14                           ││
│  │     (React 框架 + 路由 + SSG 静态导出 + API Routes)     ││
│  └─────────────────────────────────────────────────────────┘│
├─────────────────────────────────────────────────────────────┤
│                     多平台部署层                             │
│  ┌──────────────┐ ┌──────────────┐ ┌──────────────────────┐ │
│  │   Electron   │ │  Capacitor   │ │     Web Browser      │ │
│  │  Windows/Mac │ │  iOS/Android │ │       浏览器         │ │
│  └──────────────┘ └──────────────┘ └──────────────────────┘ │
├─────────────────────────────────────────────────────────────┤
│                      AI 服务层                               │
│  ┌─────────────────────────────────────────────────────────┐│
│  │              DeepSeek API (大语言模型)                   ││
│  └─────────────────────────────────────────────────────────┘│
└─────────────────────────────────────────────────────────────┘
```

---

## 技术栈速查表

| 领域 | 技术 | 用途 |
|------|------|------|
| **前端框架** | React 18 | UI 组件化开发 |
| **元框架** | Next.js 14 | React 应用框架 |
| **类型系统** | TypeScript | 静态类型检查 |
| **样式** | Tailwind CSS | 原子化 CSS |
| **动画** | Framer Motion | React 动画库 |
| **桌面端** | Electron | 跨平台桌面应用 |
| **移动端** | Capacitor | 跨平台移动应用 |
| **AI API** | DeepSeek | 大语言模型调用 |

---

## 学习路径推荐

> 建议按照以下顺序学习：

1. **Node.js 与 npm** - 环境搭建
2. **React 基础** - 组件、状态、Hook
3. **TypeScript** - 类型系统
4. **Next.js** - 框架进阶
5. **Tailwind CSS** - 样式方案
6. **Electron / Capacitor** - 平台打包

---

## 项目目录结构

```
边界项目/
├── src/                    # 源代码目录
│   ├── app/               # Next.js App Router 页面
│   │   ├── api/chat/     # API 路由 (后端接口)
│   │   ├── page.tsx      # 主页面
│   │   ├── layout.tsx    # 根布局
│   │   └── globals.css   # 全局样式
│   ├── components/        # React 组件
│   │   ├── StartScreen.tsx
│   │   ├── ChatInput.tsx
│   │   └── ...
│   ├── lib/               # 工具函数和配置
│   │   ├── characters.ts  # 角色定义
│   │   └── aiEngine.ts    # AI 引擎
│   └── types/             # TypeScript 类型定义
│       └── game.ts
├── public/                 # 静态资源
│   ├── avatars/           # 角色头像
│   └── backgrounds/       # 背景图片
├── electron/               # Electron 主进程代码
│   └── main.js
├── package.json            # 项目配置和依赖
├── tailwind.config.js      # Tailwind 配置
├── next.config.js          # Next.js 配置
└── tsconfig.json           # TypeScript 配置
```

---

## 核心技术要点

### React 组件设计

```tsx
// 函数组件 + Hooks
const ChatMessage: React.FC<MessageProps> = ({ content, role }) => {
  const [isTyping, setIsTyping] = useState(false);
  
  useEffect(() => {
    // 打字机效果
  }, [content]);
  
  return (
    <motion.div
      initial={{ opacity: 0, y: 20 }}
      animate={{ opacity: 1, y: 0 }}
    >
      {content}
    </motion.div>
  );
};
```

### AI API 集成

```typescript
// DeepSeek API 调用
const response = await fetch('/api/chat', {
  method: 'POST',
  body: JSON.stringify({
    messages: conversationHistory,
    character: currentCharacter,
  }),
});
```

---

## 关键收获

1. **组件化思维** - 将 UI 拆分为可复用的独立单元
2. **类型安全** - TypeScript 让大型项目更易维护
3. **跨平台策略** - 一套代码，多端部署
4. **AI 集成** - LLM API 的最佳实践

---

*项目持续迭代中...*
