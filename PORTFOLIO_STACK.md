# Portfolio Website — Library Stack

## Core Framework
- **Next.js 14** (App Router) — routing, SSG, image optimization
- **TypeScript** — type safety

---

## Animation & Motion

### Framer Motion
```bash
npm install framer-motion
```
- Page transitions, scroll-triggered animations, drag gestures
- `useScroll` + `useTransform` for parallax effects
- `AnimatePresence` for route transitions
- `motion.div` for declarative animations

### GSAP (GreenSock)
```bash
npm install gsap @gsap/react
```
- Complex timeline animations
- ScrollTrigger plugin for scroll-based sequences
- Text scramble / split text effects

### Remotion
```bash
npm install remotion @remotion/player
```
- Embed video-like animated sequences in your hero section
- Render your intro/about section as a React-based "video"
- Use `<Player>` component to play compositions inline

### React Spring
```bash
npm install @react-spring/web
```
- Physics-based animations (bounce, tension, friction)
- Great for interactive hover/click micro-interactions

---

## 3D & WebGL

### Three.js + React Three Fiber
```bash
npm install three @react-three/fiber @react-three/drei
```
- 3D background scenes, floating objects, particle systems
- `@react-three/drei` gives ready-made helpers (OrbitControls, Stars, Float)

### Spline
```bash
npm install @splinetool/react-spline
```
- Embed interactive 3D scenes designed in Spline (no Three.js code needed)
- Great for hero section 3D objects

---

## UI Components & Styling

### Tailwind CSS
```bash
npm install tailwindcss
```
- Utility-first, pairs perfectly with all animation libs

### shadcn/ui
```bash
npx shadcn@latest init
```
- Headless, accessible components (Dialog, Tooltip, Sheet, etc.)
- Copy-paste into your project, fully customizable

### Aceternity UI
- Pre-built animated components (Spotlight, TypewriterEffect, BackgroundBeams, CardHover)
- https://ui.aceternity.com — copy components directly

### Magic UI
- Animated borders, shimmer buttons, meteors, bento grid
- https://magicui.design — copy components directly

---

## Text Effects

### Typewriter Effect
```bash
npm install typewriter-effect
```

### React Countup
```bash
npm install react-countup
```
- Animated number counters for stats (e.g. "50+ projects")

---

## Scroll & Parallax

### Lenis (Smooth Scroll)
```bash
npm install lenis
```
- Buttery smooth scroll — pairs perfectly with GSAP ScrollTrigger

### React Intersection Observer
```bash
npm install react-intersection-observer
```
- Trigger animations when elements enter viewport

---

## Particle & Background Effects

### TSParticles
```bash
npm install @tsparticles/react @tsparticles/slim
```
- Particle backgrounds, confetti, fireworks

### Vanta.js
```bash
npm install vanta
```
- Animated mesh/fog/wave backgrounds (requires Three.js)

---

## Recommended Page Sections & What to Use

| Section | Libraries |
|---|---|
| Hero | Remotion Player + Framer Motion + Spline 3D |
| About | GSAP text reveal + React Spring |
| Projects | Framer Motion layout animations + Aceternity card hover |
| Skills | React Countup + Framer Motion progress bars |
| Timeline | GSAP ScrollTrigger + Lenis |
| Contact | shadcn/ui form + Framer Motion |
| Background | TSParticles or Three.js stars |

---

## Suggested Install (all at once)

```bash
npm install framer-motion gsap @gsap/react three @react-three/fiber @react-three/drei \
  @splinetool/react-spline remotion @remotion/player lenis \
  react-intersection-observer react-countup typewriter-effect \
  @tsparticles/react @tsparticles/slim @react-spring/web
```

---

## Tips

- Use **Lenis + GSAP ScrollTrigger** together — they are the industry standard combo for scroll animations
- Keep **Remotion** for your hero/intro only — it's heavy
- **Aceternity UI** and **Magic UI** save hours — use them for cards, borders, and spotlight effects
- Lazy load Three.js scenes with `next/dynamic` to keep initial load fast
