---
title: "React Fundamentals for Game UI"
date: 2025-11-18
tags: ["React", "JavaScript", "frontend", "game-development"]
categories: ["Projects"]
summary: "Core React concepts applied to building game interfaces - components, state, hooks, and more."
---

## What is React?

React is a **JavaScript UI library** developed by Facebook (Meta) for building user interfaces. It uses a **component-based** approach, breaking pages into independent, reusable components.

## Core Concepts

### 1. Components

Components are React's core building blocks:

```tsx
// Function Component (Recommended)
function Welcome({ name }: { name: string }) {
  return <h1>Hello, {name}</h1>;
}

// Using the component
<Welcome name="Player" />
```

### 2. JSX Syntax

JSX is a JavaScript syntax extension that lets you write HTML-like code in JS:

```tsx
const element = (
  <div className="container">
    <h1>Boundary</h1>
    <p>AI Mental Health Game</p>
  </div>
);
```

### 3. Props (Properties)

Props are data passed from parent to child components:

```tsx
// Parent component
<MessageBubble 
  content="Hello!" 
  isUser={true} 
  characterName="Yang Guang"
/>

// Child component
function MessageBubble({ content, isUser, characterName }) {
  return (
    <div className={isUser ? 'user-msg' : 'npc-msg'}>
      {content}
    </div>
  );
}
```

### 4. State

State is mutable data within a component:

```tsx
import { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);
  
  return (
    <button onClick={() => setCount(count + 1)}>
      Clicks: {count}
    </button>
  );
}
```

### 5. Hooks

Hooks let function components use state and lifecycle features:

| Hook | Purpose |
|------|---------|
| `useState` | Manage component state |
| `useEffect` | Handle side effects (API calls, subscriptions) |
| `useCallback` | Memoize function references |
| `useMemo` | Memoize computed values |
| `useRef` | Get DOM references or hold mutable values |

```tsx
// useEffect example: fetch data on mount
useEffect(() => {
  async function fetchData() {
    const response = await fetch('/api/chat');
    const data = await response.json();
    setMessages(data);
  }
  fetchData();
}, []); // Empty array = run only on mount
```

## Game Application Example

```tsx
'use client'; // Next.js client component marker

import { useState, useCallback } from 'react';
import StartScreen from '@/components/StartScreen';

export default function Home() {
  const [gamePhase, setGamePhase] = useState<'start' | 'playing'>('start');
  const [difficulty, setDifficulty] = useState<DifficultyLevel>('normal');
  
  const handleGameStart = useCallback((selectedDifficulty: DifficultyLevel) => {
    setDifficulty(selectedDifficulty);
    setGamePhase('playing');
  }, []);
  
  if (gamePhase === 'start') {
    return <StartScreen onStart={handleGameStart} />;
  }
  
  return <GameUI difficulty={difficulty} />;
}
```

## Common Questions

### Why use `useState` instead of regular variables?

Regular variables don't trigger re-renders when changed. The `setState` function returned by `useState` triggers component re-renders to update the UI.

### What is the useEffect dependency array?

The dependency array tells React when to re-run the effect:
- `[]` empty array: Run only on mount
- `[count]`: Run when count changes
- Not provided: Run on every render

## Learning Resources

- [React Official Docs](https://react.dev/)
- [React New Tutorial](https://react.dev/learn)

---

**Last Updated**: 2025-11-18
