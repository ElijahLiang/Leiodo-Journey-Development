---
title: "AI API Integration for Game NPCs"
date: 2025-11-18
tags: ["AI", "LLM", "API", "game-development"]
categories: ["Projects"]
summary: "Integrating large language model APIs for intelligent NPC dialogue in games."
---

## Overview

This project integrates the **DeepSeek** large language model API to enable intelligent NPC dialogue. The API uses OpenAI-compatible format.

## Core Concepts

### Large Language Models (LLM)

LLMs are deep learning-based AI models capable of:
- Understanding natural language input
- Generating coherent text responses
- Role-playing specific characters
- Performing various text tasks

### API Call Flow

```
+-----------+     +-----------+     +-----------+
| User Input| --> |Build Req  | --> |Send Req   |
+-----------+     +-----------+     +-----------+
                                          |
                                          v
+-----------+     +-----------+     +-----------+
|Display Res| <-- |Parse Res  | <-- |Receive Res|
+-----------+     +-----------+     +-----------+
```

### Message Format

OpenAI-compatible APIs use **role-based messages**:

```typescript
const messages = [
  {
    role: 'system',    // System prompt (character setup)
    content: 'You are Yang Guang, a warm senior...'
  },
  {
    role: 'user',      // User message
    content: 'Hello!'
  },
  {
    role: 'assistant', // AI response
    content: 'Hi there! How have you been?'
  },
  {
    role: 'user',
    content: "I'm okay..."
  }
];
```

## Implementation

### Environment Variables

```bash
# .env.local
DEEPSEEK_API_KEY=sk-xxxxxxxxxxxxxxxxxxxxxxxx
DEEPSEEK_BASE_URL=https://api.deepseek.com/v1
DEEPSEEK_MODEL=deepseek-chat
```

### API Route (Next.js)

```typescript
// src/app/api/chat/route.ts
import { NextRequest, NextResponse } from 'next/server';

export async function POST(request: NextRequest) {
  const body = await request.json();
  const { userInput, characterPrompt, conversationHistory } = body;

  // Build message list
  const messages = [
    { role: 'system', content: characterPrompt },
    ...conversationHistory.map(msg => ({
      role: msg.isUser ? 'user' : 'assistant',
      content: msg.content
    })),
    { role: 'user', content: userInput }
  ];

  // Call API
  const response = await fetch(`${process.env.DEEPSEEK_BASE_URL}/chat/completions`, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'Authorization': `Bearer ${process.env.DEEPSEEK_API_KEY}`
    },
    body: JSON.stringify({
      model: process.env.DEEPSEEK_MODEL || 'deepseek-chat',
      messages,
      temperature: 0.7,    // Creativity (0-2, higher = more random)
      max_tokens: 1000     // Max response length
    })
  });

  const data = await response.json();
  
  if (data.choices && data.choices[0]) {
    return NextResponse.json({
      success: true,
      reply: data.choices[0].message.content
    });
  }

  return NextResponse.json({
    success: false,
    error: data.error?.message || 'API Error'
  });
}
```

### Frontend Call

```typescript
async function sendMessage(input: string) {
  const response = await fetch('/api/chat', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      userInput: input,
      characterPrompt: currentCharacter.systemPrompt,
      conversationHistory: messages
    })
  });
  
  const data = await response.json();
  
  if (data.success) {
    addMessage({
      content: data.reply,
      isUser: false
    });
  }
}
```

## Character Prompt Design

### System Prompt Structure

```typescript
const characterPrompt = `You are Yang Guang, a senior.

【Identity】
- Neighbor type, wears glasses, casual clothes, bookish
- Genuine and warm to people
- Close relationship with player, frequent communication

【Core Principles】
1. Sincere but not deliberate
2. Willing to listen
3. Don't judge the player
4. Not a savior, just someone who understands

【Dialogue Style】
- Gentle, friendly, like a good senior
- Proactively asks how player is doing
- 15-50 characters, longer for important topics

【Behavior Pattern】
- Close relationship, frequent communication
- Proactively cares about player's state`;
```

### Difficulty Adjustment

Different difficulty levels modify character system prompts:

| Difficulty | Yang Guang | Teacher | Bully |
|------------|------------|---------|-------|
| Paradise | Close, proactive care | Understanding | Isolated, no influence |
| Normal | Occasional chat | Neutral, professional | Continuous verbal attacks |
| Hell | Barely notices player | Oppressive, grades-focused | Public humiliation, physical threats |

## API Parameters

| Parameter | Description | Recommended |
|-----------|-------------|-------------|
| `temperature` | Creativity/randomness | 0.7 (balanced) |
| `max_tokens` | Max response tokens | 500-1000 |
| `top_p` | Nucleus sampling | 0.9 |
| `frequency_penalty` | Repetition penalty | 0.5 |
| `presence_penalty` | Topic diversity | 0.5 |

## Alternative AI APIs

| Provider | API | Notes |
|----------|-----|-------|
| OpenAI | GPT-4/3.5 | Most powerful, needs VPN in China |
| DeepSeek | deepseek-chat | China-accessible, cost-effective |
| Zhipu AI | GLM-4 | China-accessible, multimodal |
| Baidu | ERNIE Bot | China-accessible, free tier |
| Alibaba | Qwen | China-accessible, free tier |

## Common Issues

### 401 Error?

API Key invalid or expired. Check:
1. Key in `.env.local` is correct
2. Key has remaining quota
3. Key is not disabled

### Response doesn't match character?

Improve system prompt:
1. More explicit character description
2. Add "things NOT to do" list
3. Provide example dialogues
4. Use emphasis markers like 【Important】

---

**Last Updated**: 2025-11-18
