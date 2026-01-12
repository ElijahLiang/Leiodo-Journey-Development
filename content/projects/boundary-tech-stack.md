---
title: "Boundary Project Tech Stack"
date: 2025-11-20
tags: ["React", "Next.js", "TypeScript", "Electron", "AI", "game-development"]
categories: ["Projects"]
summary: "Technical architecture and implementation details of the Boundary AI-driven mental health game."
---

## Project Overview

**Boundary** is an AI-driven mental health interactive game that uses large language models (LLMs) to create realistic NPC characters. Players interact with virtual characters through dialogue, exploring topics of interpersonal relationships, emotional expression, and psychological adjustment.

## Technical Architecture

```
+-------------------------------------------------------------+
|                      UI Layer                                |
|  +-----------+ +-------------+ +---------------------------+ |
|  | React 18  | | Tailwind CSS| |     Framer Motion         | |
|  | Components| | Styling     | |     Animation             | |
|  +-----------+ +-------------+ +---------------------------+ |
+-------------------------------------------------------------+
|                    Framework Layer                           |
|  +---------------------------------------------------------+ |
|  |                    Next.js 14                           | |
|  |     (React + Router + SSG Export + API Routes)          | |
|  +---------------------------------------------------------+ |
+-------------------------------------------------------------+
|                   Multi-Platform Layer                       |
|  +------------+ +--------------+ +------------------------+  |
|  |  Electron  | |  Capacitor   | |     Web Browser        |  |
|  | Win / Mac  | | iOS/Android  | |                        |  |
|  +------------+ +--------------+ +------------------------+  |
+-------------------------------------------------------------+
|                      AI Service Layer                        |
|  +---------------------------------------------------------+ |
|  |              DeepSeek API (LLM)                         | |
|  +---------------------------------------------------------+ |
+-------------------------------------------------------------+
```

## Project Structure

```
boundary-project/
├── src/                    # Source code
│   ├── app/               # Next.js App Router pages
│   │   ├── api/chat/     # API routes (backend)
│   │   ├── page.tsx      # Main page
│   │   ├── layout.tsx    # Root layout
│   │   └── globals.css   # Global styles
│   ├── components/        # React components
│   │   ├── StartScreen.tsx
│   │   ├── ChatInput.tsx
│   │   └── ...
│   ├── lib/               # Utilities and config
│   │   ├── characters.ts  # Character definitions
│   │   └── aiEngine.ts    # AI engine
│   └── types/             # TypeScript types
│       └── game.ts
├── public/                 # Static assets
│   ├── avatars/           # Character avatars
│   └── backgrounds/       # Background images
├── electron/               # Electron main process
│   └── main.js
├── package.json            # Project config
├── tailwind.config.js      # Tailwind config
├── next.config.js          # Next.js config
└── tsconfig.json           # TypeScript config
```

## Core Technologies

### React 18

React is a JavaScript UI library for building user interfaces using a **component-based** approach.

```tsx
// Function Component + Hooks
const ChatMessage: React.FC<MessageProps> = ({ content, role }) => {
  const [isTyping, setIsTyping] = useState(false);
  
  useEffect(() => {
    // Typewriter effect
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

### Next.js 14

Next.js adds server-side rendering, file-system routing, and API routes to React.

Key features used:
- **App Router**: File-based routing system
- **Static Export**: Generate pure static files for multi-platform distribution
- **API Routes**: Backend endpoints for AI integration

### TypeScript

Strong typing improves code quality and developer experience.

```typescript
export type DifficultyLevel = 'paradise' | 'normal' | 'hell';

export interface Character {
  id: string;
  name: string;
  nameZh: string;
  sprite: string;
  expressions: Record<string, string>;
  systemPrompts: Record<DifficultyLevel, string>;
}
```

### Tailwind CSS

Utility-first CSS framework for rapid styling:

```tsx
<button className="
  w-64 py-4 rounded-xl
  bg-gradient-to-r from-emerald-500 to-cyan-500
  text-white text-lg font-bold
  shadow-lg shadow-emerald-500/30
  hover:shadow-xl hover:scale-105
  transition-all duration-300
">
  Paradise Mode
</button>
```

### Framer Motion

Animation library for React with declarative API:

```tsx
<motion.div
  initial={{ opacity: 0, y: 20 }}
  animate={{ opacity: 1, y: 0 }}
  exit={{ opacity: 0, y: -20 }}
  transition={{ duration: 0.5 }}
>
  {content}
</motion.div>
```

### AI API Integration

```typescript
// DeepSeek API call
const response = await fetch('/api/chat', {
  method: 'POST',
  body: JSON.stringify({
    messages: conversationHistory,
    character: currentCharacter,
  }),
});
```

## Cross-Platform Strategy

### "Write Once, Run Everywhere"

1. Build with Next.js static export (`output: 'export'`)
2. Wrap with Electron for desktop (Windows/Mac)
3. Wrap with Capacitor for mobile (iOS/Android)
4. Deploy as web app for browsers

### Challenge: API Routes in Static Export

Static export doesn't include API routes, so we implement a local HTTP server in Electron's main process to handle API requests.

## Learning Resources

- [React Documentation](https://react.dev/)
- [Next.js Documentation](https://nextjs.org/docs)
- [TypeScript Handbook](https://www.typescriptlang.org/docs/)
- [Tailwind CSS](https://tailwindcss.com/docs)
- [Framer Motion](https://www.framer.com/motion/)
- [Electron](https://www.electronjs.org/docs)

---

**Last Updated**: 2025-11-20
