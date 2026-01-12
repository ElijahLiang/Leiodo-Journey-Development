---
title: "My Embodied AI Development Journey"
date: 2025-01-10
tags: ["Unity", "AI", "VLM", "LLM", "embodied-intelligence", "game-development"]
categories: ["Projects"]
summary: "A chronicle of building intelligent NPCs in Unity, from heuristic hacks to vision-language models."
---

## Overview

This project represents a solitary yet enlightening journey into Embodied AI. The goal: create NPCs that can truly perceive, reason, and act in a 3D game environment.

## Development Timeline

### Phase 1: LLM-Driven NPCs (November 2025)

**Initial Approach**: Use Large Language Models to drive NPC behavior in Unity.

**Method**: Heuristic hacks like Raycast for perception

**Result**: Poor navigation and lack of true spatial awareness

**Lessons Learned**:
- Text-only LLMs cannot truly understand 3D space
- Raycast-based perception is brittle and limited
- Need a way for AI to "see" the environment

### ML-Agents Detour

**Attempt**: Use Unity's ML-Agents for reinforcement learning

**Realization**: RL creates a "conditioned subject" - functional but incapable of generalization

**Problem**: Training environment-specific behaviors doesn't transfer to new scenarios

### Phase 2: VLM Integration (December 2025)

**Breakthrough**: Integration of Vision-Language Models (VLM)

**Key Innovation**: The agent can now truly "see" by processing camera frames

**Architecture**:
- Unity captures game view
- VLM processes visual input
- LLM reasons about observations
- Actions sent back to Unity

### Phase 3: End-to-End Loop (January 2026)

**Achievement**: Complete perception-reasoning-action loop

**Capabilities**:
- Perceives environment through vision
- Reasons about observations
- Reacts with specific behaviors and emotions
- Facial expressions and walking animations
- Avatar that can interact with players

**Current Limitations**:
- Hardware constraints require script-driven action layer
- Full VLA (Vision-Language-Action) implementation not yet possible
- Models and scenes still rough
- Some bugs to overcome

## Technical Architecture

The system follows a multi-layer design:

1. **Perception Layer**: Camera capture → VLM processing
2. **Cognition Layer**: LLM reasoning about visual observations
3. **Action Layer**: Script-driven behaviors (temporary solution)
4. **Expression Layer**: Emotion and animation control

## Challenges Encountered

### 3D Embodiment
Implementing embodiment in 3D scenes proved much more challenging than anticipated:
- Walking navigation
- Database setup for knowledge persistence
- Real-time communication between Unity and AI backend

### Hardware Constraints
Current hardware doesn't support full end-to-end VLA models, requiring a hybrid approach with scripted action execution.

## Future Directions

1. **World Models**: Explore predictive models for better planning
2. **Sim2Real Robotics**: Transfer learnings to physical robots
3. **New Game Scenes**: Implement complete game designs
4. **Open Theater Game Theory**: Use this NPC system to support interactive narrative experiments

## Reflections

This prototype validates the core architecture for embodied AI in games. While not yet a complete VLA system, it demonstrates that:

- Vision gives AI true spatial awareness
- LLM reasoning can drive meaningful behavior
- Emotion and expression add depth to interactions

The basic requirements are now met, and future game designs can be realized in new scenes, providing support for the Open Theater Game Theory.

---

**Last Updated**: 2025-01-10
