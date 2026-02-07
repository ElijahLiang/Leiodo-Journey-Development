---
title: "Yangguang: Vision-Driven NPCs Through Composed Language Models"
date: 2026-01-01
tags: ["Embodied-AI", "VLM", "LLM", "Unity3D", "NPC", "prompt-engineering", "game-AI"]
categories: ["Papers"]
summary: "An architecture combining Vision-Language Models and Large Language Models to create autonomous NPCs with genuine visual perception in Unity3D."
---

## Paper Info

|  |  |
|---|---|
| **Author** | Yibin Liang |
| **Affiliation** | School of International Innovation Creative Design, Shanghai University of Engineering Science |
| **Date** | January 2026 |
| **Status** | Preprint (未正式发表) |

---

## Abstract

This paper presents an architecture that combines Vision-Language Models (VLMs) and Large Language Models (LLMs) to create embodied AI agents in 3D virtual environments, implemented in Unity3D. The core question is: **how can we enable text-based interaction grounded in 3D scenes on real-time rendering platforms?** Rather than scripting behaviors, we give NPCs genuine autonomy to explore their surroundings. We detail the prompt engineering strategies that proved essential for coherent agent behavior, including perception prompts, decision prompts, and output format constraints. The resulting framework is modular and extensible — we hope it serves as a foundation for developers to build more interesting applications.

---

## Motivation

Traditional game AI relies on fixed scripts and behavior trees — once you change the scene, their behavior becomes completely predictable, and the game loses what makes play interesting: **improvisation, surprise, the unknown.**

This work takes a different path: give an NPC a camera, describe what it sees using a VLM, and let an LLM decide how to act. The result is an agent whose behavior **emerges from perception** rather than prescription.

> During experiments, something unexpected happened: the NPC walked to a window and stood there for a long time, gazing at the skybox outside. When asked what it was doing, it described the view in surprisingly poetic language. **This behavior was never programmed — it emerged from the agent's own perception and reasoning.**

---

## System Architecture

The system runs as **three layers**:

1. **Runtime Layer (Unity)** — The agent exists as a GameObject with a first-person camera and NavMeshAgent for movement
2. **Network Layer** — VLMClient sends screenshots to Qwen-VL-Plus for scene descriptions; ExternalLLM sends descriptions to DeepSeek-V2 for action decisions
3. **Cognitive Core** — The emergent "intelligence" that arises when visual perception feeds into language-based reasoning

### Core Loop: Observe → Think → Act

```
[Camera Capture] → [VLM: "What do I see?"] → [LLM: "What should I do?"] → [NavMesh Action]
       ↑                                                                          |
       └──────────────────────────────────────────────────────────────────────────┘
```

---

## Key Contribution: Prompt Engineering

The most critical part of this work is **prompt design**. Small changes in wording dramatically alter agent behavior.

### Perception Prompt Evolution

| Version | Prompt | Result |
|---|---|---|
| v1 ❌ | *"Describe this image in detail"* | Verbose, irrelevant details overwhelmed decision-making |
| v2 ❌ | *"List all objects visible"* | Dry inventories with no spatial relationships |
| v3 ✅ | *"You are an explorer seeing this for the first time..."* | Balanced informativeness with spatial grounding |

### Decision Prompt Structure

```
=== WHAT I SEE ===
{VLM description}

=== MY MEMORY ===
Recent locations: {last 5 places}
Objects encountered: {list}
Recent actions: {last 3 actions}

=== MY GOAL ===
High-level → Current task → Sub-goal

=== OUTPUT FORMAT ===
THOUGHT: [reasoning]
ACTION: [GO_TO(object) | EXPLORE | WAIT]
```

---

## Emergent Behaviors

These behaviors were **never explicitly programmed**:

- **Curiosity** — Agents generate thoughts like *"I see a hill I haven't explored"* and navigate toward it
- **Aesthetic appreciation** — The agent sometimes stops at scenic viewpoints and produces poetic descriptions
- **Memory-based avoidance** — Including recent actions in context causes agents to naturally avoid repetition
- **Terrain inference** — The LLM infers environmental properties from visual descriptions and adjusts behavior accordingly

---

## Performance

| Stage | Latency |
|---|---|
| Image capture & encode | ~60ms |
| VLM API call | 2–5s |
| LLM API call | 1–3s |
| Navigation dispatch | ~5ms |
| **Total cycle** | **3–8s** |

Running cost: ~$0.86/hour per agent at current API pricing.

---

## Video Demonstrations

- [First working prototype](https://youtu.be/0WpcWLnyKGc)
- [Early version — white-box test](https://youtu.be/DceI-MnvLxM)
- [Final version — NPC gazing out the window](https://youtu.be/dcj-9YucPV4)

---

## Citation

```bibtex
@article{liang2026yangguang,
  title={Yangguang: Vision-Driven NPCs Through Composed Language Models},
  author={Liang, Yibin},
  year={2026}
}
```
