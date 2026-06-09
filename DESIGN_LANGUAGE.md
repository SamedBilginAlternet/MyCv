# SAMED.BILGIN_ — Design Language

## Concept

**Terminal as canvas.** The content already speaks in CLI metaphors (`> whoami`, `[01 / WHOAMI]`, coordinates, live clock). The design formalize this — not as costume, but as structure. Every visual decision should feel like it was _output_ by a system, not styled by a template.

One rule above all: **if it looks like a portfolio template, remove it.**

---

## Color

```
--bg:          #080808   /* near-black, warmer than pure #000 */
--surface:     #0F0F0F   /* card / section backgrounds */
--border:      #1E1E1E   /* all borders */
--text:        #D9D4C7   /* warm off-white, like aged paper on CRT */
--text-dim:    #52504A   /* secondary text, timestamps, labels */
--accent:      #C8F03C   /* electric lime — THE one color */
--accent-dim:  #7A952A   /* lime at lower intensity */
--danger:      #FF4D1C   /* error states, active/hover on destructive */
--bg-code:     #0F0F0F   /* inline code / tag backgrounds */
```

**Rules:**
- `--accent` is used for: cursor blink, active nav item, `>` prompt symbol, hover underlines, one hero word, CTA button fill
- Never use `--accent` on more than 3 elements visible at once
- No gradients. No `background: linear-gradient(...)` anywhere
- No `rgba()` glass effects
- Background is flat `--bg`. Sections separated by 1px `--border` lines, not background color changes

---

## Typography

**One font family. All monospace.**

```css
font-family: 'JetBrains Mono', 'Fira Code', monospace;
```

Every single text element — headings, body, nav, labels — uses JetBrains Mono. This is the radical choice. Most portfolios mix a display serif with a body sans. Using only mono makes every character feel precise and intentional.

```
--font-xs:    0.65rem  / 1.6  /* tags, coordinates, timestamps */
--font-sm:    0.8rem   / 1.6  /* body secondary, labels */
--font-base:  1rem     / 1.75 /* body */
--font-lg:    1.25rem  / 1.4  /* section intros */
--font-xl:    2rem     / 1.2  /* section headers */
--font-2xl:   3.5rem   / 1.0  /* hero name */
--font-3xl:   6rem     / 0.95 /* hero name large screens */
```

**Weight:**
- `400` — body, labels, tags
- `700` — section numbers, active states
- No italic. No underline except hover states.

**Letter-spacing:**
- Section headers (`[02 / HAKKIMDA]`): `letter-spacing: 0.15em`
- Tags (`[Spring Boot]`): `letter-spacing: 0.08em`
- Body: `letter-spacing: 0` — don't space out reading text

**Text transforms:**
- Nav items: `uppercase`
- Section numbers: `uppercase`
- Body copy: normal case (the all-caps headers already create hierarchy)

---

## Spacing

8px base unit. All spacing is a multiple of 8.

```
--space-1:   8px
--space-2:   16px
--space-3:   24px
--space-4:   32px
--space-6:   48px
--space-8:   64px
--space-12:  96px
--space-16:  128px
--space-24:  192px
```

Section padding: `--space-16` top and bottom on desktop, `--space-8` on mobile.

---

## Border & Radius

```
border: 1px solid var(--border);
border-radius: 0;    /* zero everywhere, no exceptions */
```

Cards, buttons, tags, inputs — all sharp 90° corners. The monospace grid implies right angles.

---

## Layout

```
max-width: 1100px;
margin: 0 auto;
padding: 0 var(--space-4);
```

- Single column for mobile
- Two-column grid for `[HAKKIMDA]` stats section: `grid-template-columns: 1fr 1fr`
- Three-column for stack tags: `flex-wrap: wrap` with `gap: var(--space-1)`
- Timeline: single column, left border line as track

**Fixed nav:** Top bar, full width, `--bg` background, `border-bottom: 1px solid var(--border)`. Height: 48px.

---

## Components

### Navigation
```
SAMED.BILGIN_   01.Hakkımda  02.İşler  03.Projeler  04.İletişim   [en | TR]
```
- Logo left: monospace, `--text`, cursor underscore `_` blinks in `--accent`
- Nav links: `--text-dim` default → `--accent` on hover, no background
- Hover effect: `--accent` color only, no underline, no scale
- Language toggle: `[en | TR]` — bracketed, rightmost

### Cursor (custom)
```css
cursor: none;
```
Replace with a 12×12px crosshair made of two 1px lines in `--accent`. On clickable elements: crosshair turns to `█` block cursor in `--accent`.

### Hero Section
```
> whoami
SAMED          ← --text, --font-3xl, weight 400
BİLGİN        ← --text, --font-3xl, weight 400
_              ← --accent, blinks at 1s interval
```
- `> whoami` in `--text-dim`, `--font-sm`
- Name rendered letter by letter on load (50ms per character) — no repeated typing loop, just initial render
- Subtitle line: `--text-dim`, `--font-sm`, `|` cursor blinks after TypeScript
- CTA buttons: outlined (`border: 1px solid --text`), `--font-xs`, `uppercase`, `letter-spacing: 0.1em`. On hover: `border-color: --accent`, `color: --accent`
- Coordinates + time: fixed bottom-left of hero, `--text-dim`, `--font-xs`

### Section Header
```
[02 / HAKKIMDA]
```
- `--text-dim`, `--font-xs`, `letter-spacing: 0.15em`, `uppercase`
- Followed by a 1px `--border` horizontal rule
- Section title below in `--font-xl` or `--font-2xl`

### Stat Cards
```
[01]          [02]          [03]          [04]
7             3+            3.32          2
Mikroservis   Yıl Java      Not ort.      Dil
```
- No card background — just number in `--font-2xl --accent`, label in `--font-xs --text-dim`
- Separated by `border-right: 1px solid --border`

### Stack Tags
```
[Java 17/21]  [Spring Boot]  [RabbitMQ]
```
- `border: 1px solid --border`
- `padding: 4px 10px`
- `--font-xs`, `--text-dim`
- On hover: `border-color: --accent`, `color: --accent`
- No background fill

### Project Cards
```
01                                   ← --accent, --font-xs
N11 Bootcamp — E-ticaret             ← --text, --font-lg
Mikroservisleri

[description text]

[Spring Boot] [Java 21] [React]      ← tags

GÖRÜNTÜLE →                          ← hover: --accent
```
- `border-top: 1px solid --border`
- No card box. Projects are separated by horizontal rules, not boxes.
- On hover of the title: `color: --accent` — no scale, no shadow, no glow

### Timeline
```
07 / 2024 — DEVAM
│
Full Stack Developer
Alternet Yazılım
```
- Left vertical track: `border-left: 1px solid --border`
- Date: `--text-dim`, `--font-xs`, `uppercase`
- Role: `--text`, `--font-base`, weight `700`
- Company: `--accent`, `--font-xs`

### Buttons / CTAs
```
[İŞLERİ GÖR]       [İLETİŞİME GEÇ]
```
- Brackets are visual, not literal — or keep them literal in monospace
- `border: 1px solid --text`, `color: --text`
- Hover: fill `--accent`, `color: --bg`, instant (no transition)
- No border-radius, no box-shadow

### Contact
```
[EMAIL]
samedbilgin322@gmail.com    copy →

[PHONE]
+90 553 029 8210

[LINKEDIN]
/in/samed-bilgin →
```
- Labels: `--text-dim`, `--font-xs`, `uppercase`, `letter-spacing: 0.1em`
- Values: `--text`, `--font-base`
- `copy` and `→` links: `--accent` on hover

---

## Motion

**Philosophy:** Motion should feel like system output — not decorative. One well-timed entrance sequence beats scattered micro-animations everywhere.

### Page Load Sequence
```
0ms    — nav fades in (opacity 0→1, 200ms)
100ms  — hero prompt "> whoami" appears
150ms  — name renders letter by letter (50ms/char)
600ms  — cursor blink starts
800ms  — subtitle appears
1000ms — CTA buttons appear
1200ms — coordinates appear
```

### Scroll Reveals
```css
/* Elements enter with: */
transform: translateY(16px);
opacity: 0;
/* → */
transform: translateY(0);
opacity: 1;
transition: all 400ms ease-out;
```
- Stagger siblings: 80ms delay between each
- Trigger at 80% viewport threshold
- No bounce, no spring — ease-out only

### Hover States
- Color transitions: `transition: color 80ms, border-color 80ms` — fast
- No `transform: scale()` on hover
- No glow, no box-shadow on hover

### Live Clock
```js
// Updates every second in hero footer
// Format: HH:MM:SS — monospace, no layout shift (tabular-nums)
font-variant-numeric: tabular-nums;
```

### Cursor Blink
```css
@keyframes blink {
  0%, 100% { opacity: 1; }
  50%       { opacity: 0; }
}
animation: blink 1s step-end infinite;
```

---

## What NOT to Do

| Banned | Why |
|---|---|
| `background: linear-gradient(...)` | Screams AI template |
| `backdrop-filter: blur(...)` | Glassmorphism is dead |
| `border-radius > 0` | Breaks the grid metaphor |
| `box-shadow` on cards | Use borders instead |
| Particle backgrounds | Cliché |
| Purple / blue palette | Every dev portfolio |
| Typing animation that loops | One render on load only |
| Inter, Roboto, Space Grotesk | Too generic |
| `transform: scale(1.05)` on hover | Feels like a template |
| Scroll progress bar | Decoration with no function |
| Three.js hero scene | Heavy, distracting |
| More than 3 `--accent` elements visible at once | Dilutes the accent |

---

## Implementation Notes

**Font loading:**
```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<!-- JetBrains Mono 400 + 700, latin + latin-ext (for Turkish chars) -->
```

**Lenis smooth scroll:** Yes — but low intensity (`lerp: 0.08`). Should feel slightly weighted, not floaty.

**GSAP ScrollTrigger:** For section reveals. Use `scrub: false` — snappy, not tied to scroll position.

**No Remotion in production build** — too heavy. Save for a standalone `/intro` page if desired.

**Turkish character support:** JetBrains Mono covers ğ ş ı ç ö ü — no fallback issues.

**Favicon:** A monospace `S_` in `--accent` on `--bg`. 32×32px.
