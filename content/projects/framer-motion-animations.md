---
title: "Framer Motion for Game UI Animations"
date: 2025-11-18
tags: ["Framer Motion", "React", "animation", "frontend"]
categories: ["Projects"]
summary: "Creating smooth, declarative animations in React game interfaces using Framer Motion."
---

## What is Framer Motion?

Framer Motion is a **React animation library** with a declarative API. Compared to native CSS animations, it makes complex interactive animations much easier to implement.

## Core Concepts

### 1. Motion Components

`motion` is the core of Framer Motion. Any HTML element can be prefixed with `motion.`:

```tsx
import { motion } from 'framer-motion';

<motion.div
  initial={{ opacity: 0 }}    // Initial state
  animate={{ opacity: 1 }}    // Target state
  exit={{ opacity: 0 }}       // Exit state
>
  Content
</motion.div>
```

### 2. Common Properties

```tsx
<motion.div
  // Initial state
  initial={{ opacity: 0, y: 20 }}
  
  // Animation target
  animate={{ opacity: 1, y: 0 }}
  
  // Transition config
  transition={{ 
    duration: 0.5,      // Duration (seconds)
    delay: 0.2,         // Delay (seconds)
    ease: "easeOut"     // Easing function
  }}
  
  // Hover state
  whileHover={{ scale: 1.05 }}
  
  // Click state
  whileTap={{ scale: 0.95 }}
  
  // Exit animation
  exit={{ opacity: 0, y: -20 }}
>
  Animated Element
</motion.div>
```

### 3. Easing Functions

| Name | Effect |
|------|--------|
| `"linear"` | Constant speed |
| `"easeIn"` | Slow start |
| `"easeOut"` | Slow end |
| `"easeInOut"` | Slow start and end |
| `[0.6, 0, 0.4, 1]` | Custom bezier curve |

### 4. Variants

Variants organize complex animation states:

```tsx
const containerVariants = {
  hidden: { opacity: 0 },
  visible: {
    opacity: 1,
    transition: {
      staggerChildren: 0.1  // Stagger child animations
    }
  }
};

const itemVariants = {
  hidden: { opacity: 0, y: 20 },
  visible: { opacity: 1, y: 0 }
};

<motion.ul
  variants={containerVariants}
  initial="hidden"
  animate="visible"
>
  <motion.li variants={itemVariants}>Item 1</motion.li>
  <motion.li variants={itemVariants}>Item 2</motion.li>
  <motion.li variants={itemVariants}>Item 3</motion.li>
</motion.ul>
```

### 5. AnimatePresence

Handle component **enter/exit** animations:

```tsx
import { motion, AnimatePresence } from 'framer-motion';

function MessageList({ messages }) {
  return (
    <AnimatePresence>
      {messages.map(msg => (
        <motion.div
          key={msg.id}
          initial={{ opacity: 0, x: 100 }}
          animate={{ opacity: 1, x: 0 }}
          exit={{ opacity: 0, x: -100 }}
        >
          {msg.content}
        </motion.div>
      ))}
    </AnimatePresence>
  );
}
```

## Game UI Examples

### Start Screen Fade-In

```tsx
<motion.div
  initial={{ opacity: 0 }}
  animate={{ opacity: 1 }}
  transition={{ duration: 1 }}
  className="min-h-screen"
>
  <motion.h1
    initial={{ opacity: 0, y: -30 }}
    animate={{ opacity: 1, y: 0 }}
    transition={{ delay: 0.3, duration: 0.8 }}
    className="text-6xl font-bold"
  >
    Boundary
  </motion.h1>
</motion.div>
```

### Button Hover Effect

```tsx
<motion.button
  whileHover={{ 
    scale: 1.05,
    boxShadow: "0 20px 40px rgba(0,0,0,0.2)"
  }}
  whileTap={{ scale: 0.95 }}
  transition={{ type: "spring", stiffness: 300 }}
  className="px-8 py-4 bg-blue-500 rounded-lg"
>
  Start Game
</motion.button>
```

### Page Transitions

```tsx
const pageTransition = {
  initial: { opacity: 0, scale: 0.9 },
  animate: { opacity: 1, scale: 1 },
  exit: { opacity: 0, scale: 1.1 },
  transition: { duration: 0.5 }
};

<AnimatePresence mode="wait">
  {phase === 'cover' && (
    <motion.div key="cover" {...pageTransition}>
      <CoverScreen />
    </motion.div>
  )}
  {phase === 'intro' && (
    <motion.div key="intro" {...pageTransition}>
      <IntroScreen />
    </motion.div>
  )}
</AnimatePresence>
```

## Animation Techniques

### Spring Animation

```tsx
<motion.div
  animate={{ x: 100 }}
  transition={{
    type: "spring",
    stiffness: 100,    // Higher = faster
    damping: 10,       // Higher = less bounce
    mass: 1            // Mass
  }}
/>
```

### Loop Animation

```tsx
<motion.div
  animate={{
    y: [0, -10, 0],       // Keyframes
    opacity: [0.5, 1, 0.5]
  }}
  transition={{
    duration: 2,
    repeat: Infinity,     // Infinite loop
    repeatType: "loop"
  }}
/>
```

### Scroll-Based Animation

```tsx
import { useScroll, useTransform } from 'framer-motion';

function ParallaxSection() {
  const { scrollYProgress } = useScroll();
  const y = useTransform(scrollYProgress, [0, 1], [0, -100]);
  
  return <motion.div style={{ y }}>Parallax Effect</motion.div>;
}
```

## Performance Tips

- **Prefer `transform` and `opacity`** - GPU accelerated
- **Avoid animating `width`, `height`, `top`, `left`** - causes reflows
- Use `will-change` CSS property to hint browser

## Common Issues

### Exit animation not working?

Ensure:
1. Wrapped with `AnimatePresence`
2. Element has unique `key` prop
3. `AnimatePresence` `mode` is set correctly

## Learning Resources

- [Framer Motion Official Docs](https://www.framer.com/motion/)
- [Motion Examples](https://www.framer.com/motion/examples/)

---

**Last Updated**: 2025-11-18
