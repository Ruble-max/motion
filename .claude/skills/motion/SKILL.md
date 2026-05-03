---
name: motion
description: Use the Motion animation library (motiondivision/motion) to add animations to JavaScript and React projects. Covers the `animate()` API, React `motion` components, gestures, scroll-linked animations, layout animations, and the `mini` bundle for size-sensitive code. Trigger when the user wants to animate DOM elements, add transitions, build interactive gestures, or imports from `motion`, `motion/react`, or `motion/mini`.
---

# Motion

Motion is the animation library at `node_modules/motion` (https://github.com/motiondivision/motion). It works in two modes:

- **Vanilla JS** — `import { animate } from "motion"` for any DOM element.
- **React** — `import { motion } from "motion/react"` for declarative components, gestures, and layout animations.

## Vanilla JS — `animate()`

```js
import { animate } from "motion"

animate("#box", { x: 100, opacity: 1 }, { duration: 0.6, ease: "easeOut" })

// Keyframes
animate("li", { y: [0, -20, 0] }, { duration: 1, repeat: Infinity })

// Controls
const controls = animate(element, { rotate: 360 })
controls.pause()
controls.play()
controls.stop()
await controls.finished
```

Supports CSS variables, transforms, colors, SVG attrs, independent transforms (`x`, `y`, `scale`, `rotate`).

## React — `motion` components

```jsx
import { motion } from "motion/react"

<motion.div
  initial={{ opacity: 0, y: 20 }}
  animate={{ opacity: 1, y: 0 }}
  exit={{ opacity: 0 }}
  transition={{ duration: 0.4, ease: "easeOut" }}
  whileHover={{ scale: 1.05 }}
  whileTap={{ scale: 0.95 }}
/>
```

Mount/unmount animations need `AnimatePresence`:

```jsx
import { AnimatePresence, motion } from "motion/react"

<AnimatePresence>
  {open && <motion.div key="modal" exit={{ opacity: 0 }} />}
</AnimatePresence>
```

## Layout animations

```jsx
<motion.div layout />          // animates layout changes automatically
<motion.div layoutId="card" /> // shared element transitions across components
```

## Gestures & scroll

```jsx
import { motion, useScroll, useTransform } from "motion/react"

const { scrollYProgress } = useScroll()
const opacity = useTransform(scrollYProgress, [0, 1], [1, 0])

<motion.div drag dragConstraints={{ left: 0, right: 100 }} style={{ opacity }} />
```

## Bundle size — `mini`

For LCP-critical code, prefer the mini bundle (~2.5kb):

```js
import { animate } from "motion/mini"
```

Mini supports the core `animate()` API but drops independent transforms, complex easings, and some advanced features. Use the full bundle when you need them.

## Server components (Next.js / React Server Components)

`motion/react` is client-only. In RSC contexts:

- Mark the file `"use client"`, **or**
- Use `motion/react-m` for tree-shakable proxy components, then wrap your app in `<LazyMotion features={domAnimation}>` to load features on demand.

```jsx
import { LazyMotion, domAnimation, m } from "motion/react"

<LazyMotion features={domAnimation}>
  <m.div animate={{ opacity: 1 }} />
</LazyMotion>
```

## Common pitfalls

- **`exit` doesn't fire** — the element must be a direct child of `<AnimatePresence>` and have a stable `key`.
- **Layout animations jitter** — wrap children that should not stretch with `<motion.div layout="position">`.
- **Tailwind transforms conflict** — Motion writes to `transform` directly; remove conflicting Tailwind classes like `translate-*` on animated elements.
- **SSR hydration mismatch** — set `initial={false}` to skip the first animation, or gate animations behind a mount effect.

## Reference

- Docs: https://motion.dev/docs
- Source: https://github.com/motiondivision/motion
- Installed version: see `package.json` (`motion` dependency)
