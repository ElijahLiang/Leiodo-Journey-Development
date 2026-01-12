---
title: "Next.js App Router for Game Development"
date: 2025-11-18
tags: ["Next.js", "React", "fullstack", "game-development"]
categories: ["Projects"]
summary: "Using Next.js App Router for full-stack game development with static export and API routes."
---

## What is Next.js?

Next.js is a **full-stack React framework** by Vercel that adds:

- 📁 **File-system routing**: Folder structure = routes
- 🖥️ **Server-side rendering (SSR)**: Better first load and SEO
- 📦 **Static site generation (SSG)**: Pre-built HTML at build time
- 🔌 **API Routes**: Backend endpoints in the same project
- ⚡ **Automatic code splitting**: Load only what's needed

## Core Concepts

### 1. App Router

Next.js 14 uses App Router with file-system based routing:

```
src/app/
├── page.tsx          → / (home)
├── layout.tsx        → Layout component (shared)
├── globals.css       → Global styles
├── api/
│   └── chat/
│       └── route.ts  → /api/chat (API endpoint)
└── about/
    └── page.tsx      → /about page
```

### 2. Client vs Server Components

```tsx
// Server Component (default) - renders on server
// Cannot use useState, useEffect, etc.
export default function ServerComponent() {
  return <div>I render on the server</div>;
}

// Client Component - renders in browser
// Needs 'use client' directive
'use client';

import { useState } from 'react';

export default function ClientComponent() {
  const [count, setCount] = useState(0);
  return <button onClick={() => setCount(c => c + 1)}>{count}</button>;
}
```

### 3. API Routes

Create backend endpoints in the `app/api` directory:

```typescript
// src/app/api/chat/route.ts
import { NextRequest, NextResponse } from 'next/server';

export async function POST(request: NextRequest) {
  const body = await request.json();
  
  // Call AI API
  const response = await fetch('https://api.deepseek.com/v1/chat/completions', {
    method: 'POST',
    headers: {
      'Authorization': `Bearer ${process.env.DEEPSEEK_API_KEY}`,
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({
      model: 'deepseek-chat',
      messages: body.messages
    })
  });
  
  const data = await response.json();
  return NextResponse.json(data);
}
```

### 4. Static Export

Export Next.js app as pure static files:

```javascript
// next.config.js
module.exports = {
  output: 'export',        // Enable static export
  trailingSlash: true,     // Add trailing slashes
  images: {
    unoptimized: true      // Required for static export
  }
}
```

**Limitations of Static Export**:
- No SSR
- API Routes not included in export
- Need to handle API calls differently (e.g., Electron local server)

## Layout Component

```tsx
import type { Metadata } from 'next';
import './globals.css';

export const metadata: Metadata = {
  title: 'Boundary',
  description: 'AI Mental Health Interactive Game',
};

export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en">
      <body>{children}</body>
    </html>
  );
}
```

## Common Commands

```bash
# Development mode (hot reload)
npm run dev

# Build production
npm run build

# Start production server
npm run start
```

## Common Questions

### App Router vs Pages Router?

- **Pages Router**: Legacy, files in `pages/` directory
- **App Router**: New (recommended), files in `app/` directory, supports React Server Components

### Why does state disappear on page refresh?

React state lives in memory; page refresh reloads everything. For persistent state, use:
- `localStorage`
- URL parameters
- Database

## Learning Resources

- [Next.js Official Docs](https://nextjs.org/docs)
- [Next.js Learn](https://nextjs.org/learn)

---

**Last Updated**: 2025-11-18
