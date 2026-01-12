---
title: "具身智能开发之旅：从 Raycast 到 VLM"
date: 2026-01-13
draft: false
tags: ["具身智能", "VLM", "LLM", "Unity", "游戏开发"]
categories: ["项目经验"]
summary: "探索具身 AI 的旅程——从启发式方法到视觉语言模型的演进。"
cover:
  image: ""
  alt: "Embodied AI Journey"
  caption: "The evolution of AI perception"
  relative: false
showToc: true
TocOpen: false
---

## Introduction

This project represents a solitary yet enlightening journey into Embodied AI. Starting in November 2025, I attempted to drive an NPC using LLMs within Unity. What began as a simple experiment evolved into a deep exploration of how AI can truly "see" and understand its environment.

## The Beginning: Heuristic Approaches

My initial approach relied on heuristic hacks like **Raycast** for perception:

```csharp
// Traditional approach - Raycast-based perception
RaycastHit hit;
if (Physics.Raycast(transform.position, transform.forward, out hit, maxDistance))
{
    string objectName = hit.collider.gameObject.name;
    // Convert to text description for LLM
}
```

This resulted in:
- ❌ Poor navigation
- ❌ Lack of true spatial awareness
- ❌ Brittle behavior trees

I briefly detoured into **ML-Agents**, only to realize that reinforcement learning created a "conditioned subject" — functional but incapable of the generalization I sought.

## The Breakthrough: Vision-Language Models

The breakthrough came in December with the integration of **Vision-Language Models (VLM)**. By connecting a first-person camera to Qwen-VL-Plus, the agent gained the ability to truly "see":

```
PERCEPTION PIPELINE:
Camera → RenderTexture → PNG Bytes → Base64 → VLM API → Scene Description
```

The VLM doesn't just detect objects — it understands spatial relationships, context, and even subtle visual cues.

## The Current State: End-to-End Loop

In January 2026, I successfully achieved an end-to-end loop:

1. **Perceive**: VLM interprets first-person visual input
2. **Reason**: LLM processes scene description with context
3. **Act**: Agent executes specific behaviors
4. **Express**: Emotional responses through animations

### Architecture Overview

```
┌─────────────────┐    ┌──────────────────┐    ┌────────────────┐
│  Unity Camera   │───▶│  Qwen-VL (VLM)   │───▶│  DeepSeek LLM  │
│  512x512 PNG    │    │  Scene Analysis   │    │  Decision      │
└─────────────────┘    └──────────────────┘    └────────────────┘
                                                       │
                                                       ▼
                                              ┌────────────────┐
                                              │  Action Layer  │
                                              │  GO_TO/EXPLORE │
                                              │  STOP/EMOTION  │
                                              └────────────────┘
```

## Key Learnings

### 1. Perception is Everything
Without true visual understanding, AI agents are fundamentally limited. VLMs bridge the gap between raw pixels and semantic understanding.

### 2. Memory Matters
The agent maintains three types of memory:
- **Environmental**: Known objects, visited places
- **Dialogue**: Conversation history
- **Goals**: Current task, sub-goals

### 3. Emergent Behaviors
With proper perception, behaviors emerge naturally:
- Curiosity-driven exploration
- Memory-informed avoidance
- Context-aware responses

## What's Next

While hardware constraints currently necessitate a script-driven action layer (preventing a full VLA implementation), this prototype validates the core architecture. Future explorations include:

- **World Models**: Predicting future states
- **Sim2Real**: Transferring to physical robots
- **Multi-agent**: Collaborative AI behaviors

## Conclusion

This journey taught me that the key to intelligent agents isn't more complex behavior trees — it's giving them the ability to truly perceive and understand their world. The combination of VLM + LLM provides a powerful foundation for creating agents that feel genuinely intelligent.

---

*This article is part of my [Embodied Intelligence Project](/projects/embodied-ai). Check out the demos and technical documentation there.*
