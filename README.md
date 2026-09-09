# Marle & Co — Neighborhood Coffee & Artisanal Bakery

[![Design System](https://img.shields.io/badge/Design%20Tokens-CSS%20Custom%20Properties-2E4A3D?style=flat-square)](file:///c:/Users/user/Downloads/demo%20restau/index.html#L10-L18)
[![Typography](https://img.shields.io/badge/Typography-Fraunces%20%2B%20Nunito%20Sans-C9714B?style=flat-square)](https://fonts.google.com)
[![Animation Engine](https://img.shields.io/badge/Animation-GSAP%203%20%2B%20ScrollTrigger-1F3329?style=flat-square)](https://greensock.com/gsap/)
[![Standard](https://img.shields.io/badge/Workflow-ChernyCode-FBF4E6?style=flat-square&logoColor=2B241C)](file:///c:/Users/user/Downloads/demo%20restau/AGENTS.md)

> A modern, high-performance web experience for **Marle & Co**, an artisanal neighborhood coffee roastery and bakery. Built with pure semantic HTML5, modern CSS3 custom properties, hardware-accelerated 3D transforms, and custom scroll-driven SVG masking.

---

## Table of Contents

1. [Project Vision & Aesthetic Direction](#project-vision--aesthetic-direction)
2. [Visual Identity & Design Tokens](#visual-identity--design-tokens)
3. [Key Interactive UI Features](#key-interactive-ui-features)
   - [Sticky Morphing Pill Navbar](#1-sticky-morphing-pill-navbar)
   - [Croissant SVG Mask Scroll Reveal](#2-croissant-svg-mask-scroll-reveal)
   - [3D Coverflow Seasonal Showcase](#3-3d-coverflow-seasonal-showcase)
   - [Interactive Side Rail Telemetry](#4-interactive-side-rail-telemetry)
   - [Single-Accordion FAQ Component](#5-single-accordion-faq-component)
   - [Location & Hours Telemetry](#6-location--hours-telemetry)
4. [Iteration Provenance & Architecture Evolution](#iteration-provenance--architecture-evolution)
5. [Quickstart & Usage](#quickstart--usage)
6. [Conventional Commits & ChernyCode Standards](#conventional-commits--chernycode-standards)
7. [Repository File Map](#repository-file-map)

---

## Project Vision & Aesthetic Direction

**Marle & Co** represents a departure from sterile, corporate web templates in favor of a tactile, editorial dining and cafe aesthetic. Located on 118 Grove Street, Marle & Co operates as a small-batch coffee roastery and corner bakery founded on the principle of unhurried hospitality: *"Coffee that remembers your order before you finish saying it."*

```
                     +----------------------------------+
                     |           Marle & Co             |
                     |  Neighborhood Coffee & Bakery    |
                     +----------------------------------+
                                      |
       +------------------------------+-----------------------------+
       |                              |                             |
       v                              v                             v
[ Warm Editorial Tone ]     [ Artisanal Tactility ]       [ Kinetic Delight ]
Fraunces display serif,      Terracotta clay accents,      Morphing pill nav,
steam-cream backdrop,        SVG croissant mask,           3D coverflow carousel,
espresso ink typography      dotted menu separators        curved telemetry rail
```

The interface balances warm organic tones with crisp 60fps micro-interactions, delivering an inviting, community-first ambiance across mobile, tablet, and desktop viewports.

---

## Visual Identity & Design Tokens

The visual language is codified in [index.html](file:///c:/Users/user/Downloads/demo%20restau/index.html#L10-L18) through CSS Custom Properties, forming a resilient design system:

### Color Palette

| Token Name | Hex / Value | Role & Semantic Usage | Swatch |
| :--- | :--- | :--- | :--- |
| `--cream` | `#FBF4E6` | Base canvas background; soft unbleached parchment & warm milk tone | `rgb(251, 244, 230)` |
| `--forest` | `#2E4A3D` | Primary brand botanical green; order buttons, active pagination dots | `rgb(46, 74, 61)` |
| `--forest-deep` | `#1F3329` | Deep midnight evergreen; hero containers, display headings, dark cards | `rgb(31, 51, 41)` |
| `--clay` | `#C9714B` | Warm terracotta clay accent; price badges, founder quotes, telemetry dot | `rgb(201, 113, 75)` |
| `--ink` | `#2B241C` | High-contrast espresso body copy; readable without harsh absolute black | `rgb(43, 36, 28)` |
| `--line` | `rgba(43, 36, 28, 0.14)` | Hairline dividers, dotted menu leaders, structural cards | Semi-transparent |

### Typography

```css
/* Display / Headings */
font-family: 'Fraunces', serif;
font-optical-sizing: auto;
font-weight: 500, 600;
letter-spacing: -0.01em;

/* Body / Navigation / Metadata */
font-family: 'Nunito Sans', sans-serif;
font-weight: 400, 600, 700;
line-height: 1.6;
```

- **Fraunces**: A variable optical-size serif with French vintage charm, imparting warm personality to headings (`h1`, `h2`, `h3`, prices, and quotes).
- **Nunito Sans**: A balanced geometric humanist sans-serif providing crystal-clear legibility across descriptions, telemetry lines, and interactive button targets.

---

## Key Interactive UI Features

### 1. Sticky Morphing Pill Navbar

The navigation system begins as an in-flow, full-width glassmorphism container (`max-width: 1100px`, `border-radius: 22px`). As the user scrolls past the top hero trigger, it smoothly transitions into a floating, compact pill (`border-radius: 999px`) anchored at `top: 16px`.

```
[ In-Flow State ]
+-------------------------------------------------------------------------+
| Marle & Co        Menu    Reviews    Our story    Visit    [Order ahead]|
+-------------------------------------------------------------------------+
                                    |
                                    v (Scroll down)
[ Sticky Pill State ]
                     +---------------------------------------+
                     | Marle & Co  Menu  Reviews ... [Order] |
                     +---------------------------------------+
```

- **Zero-Lag Detection**: Implemented via a native `IntersectionObserver` targeting a 1px `#navSentinel` rather than a scroll event listener, avoiding layout thrashing.
- **Hardware Acceleration**: Transitions use `cubic-bezier(.4, 0, .2, 1)` across `max-width`, `padding`, `border-radius`, and `box-shadow`.
- **Integrated Smooth Scrolling**: Navigational anchor links invoke GSAP's `ScrollToPlugin` with dynamic navbar height offsets, preventing coordinate desynchronization with pinned ScrollTrigger sequences.

### 2. Croissant SVG Mask Scroll Reveal

Preceding the main layout is an editorial reveal sequence ([index.html:L306-L312](file:///c:/Users/user/Downloads/demo%20restau/index.html#L306-L312)).

- **Mask Definition**: Embedded as an inline SVG vector silhouette shaped as an artisanal croissant:
  ```css
  --croissant-mask: url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 200 120' fill='%23000000'><path d='M20,70 C50,10 150,10 180,70 C150,95 50,95 20,70 Z' transform='rotate(-12 100 60)'/></svg>");
  ```
- **ScrollTrigger Pinning**: Pins the viewport for `innerHeight * 2.2` distance.
- **Non-Linear Expansion**: As scroll progress advances from `0` to `1`, the mask width scales dynamically:
  $$\text{MaskWidth} = \text{InitialSize} + (\text{progress})^{2.2} \times 3600\text{px}$$
  This creates an organic zoom effect where the bakery atmosphere expands from within the croissant silhouette to consume the full viewport.

### 3. 3D Coverflow Seasonal Showcase

The menu section features an interactive 3D Coverflow carousel ([index.html:L522-L614](file:///c:/Users/user/Downloads/demo%20restau/index.html#L522-L614)):

- **Perspective Rendering**: Hosted in a container with `perspective: 1300px`.
- **Dynamic Transforms**: Computes card offsets relative to the active index:
  - Center: `translateX(0px) scale(1) rotateY(0deg)`
  - Flanking ($\pm 1$): `translateX(\pm 190px) scale(0.8) rotateY(\mp 22deg)`
  - Distal ($\pm 2$): `translateX(\pm 340px) scale(0.62) rotateY(\mp 34deg)`
- **Dynamic Blurred Backdrop**: An ambient background image mirrors the active pastry or beverage with `filter: blur(22px) brightness(0.58)`.
- **Multi-Input Controls**:
  - Touch gesture support (swipe threshold $> 45\text{px}$)
  - Keyboard navigation (`ArrowLeft` / `ArrowRight` when carousel is in view)
  - Interactive pagination pill dots with active expansion
  - Auto-advance timer (5000ms) with hover suspension (`mouseenter`/`mouseleave`)

### 4. Interactive Side Rail Telemetry

For wide desktop screens ($> 1300\text{px}$), a fixed SVG side rail ([index.html:L294-L300](file:///c:/Users/user/Downloads/demo%20restau/index.html#L294-L300)) occupies the left gutter:

- **SVG Bezier Path**: A custom wave curve `M22,0 C34,45 8,95 20,150 ... C32,525 10,565 20,600`.
- **Interpolated Draw**: Uses `stroke-dashoffset` driven by `requestAnimationFrame` to draw the path as the page descends.
- **Path Dot Tracker**: An SVG circle follows coordinates sampled directly via `SVGGeometryElement.getPointAtLength()`.
- **Mouse Proximity Reaction**: Detects cursor distance within 70px of the rail, expanding the stroke width and dot radius via CSS transitions.

### 5. Single-Accordion FAQ Component

The *"Good to know"* module provides accessible answers regarding seating, Wi-Fi, milk options, and pet policy.
- Enforces single-item expansion: clicking one item smoothly collapses any open sibling.
- Updates WAI-ARIA `aria-expanded` attributes dynamically.
- Animates heights via exact DOM calculation (`element.scrollHeight` / `inner.offsetHeight`).

### 6. Location & Hours Telemetry

The visit section includes structured store metadata:
- **Operating Hours**: Everyday, 7:00 AM – 4:00 PM
- **Address**: 118 Grove Street
- **Telemetry Links**: Direct click-to-call `(718) 555-0148`, email `hello@marleandco.com`, and directions trigger.

---

## Iteration Provenance & Architecture Evolution

The project preserves a complete design audit trail spanning 34 iterations (`index_0.html` through `index_33.html`), culminating in the unified production master [index.html](file:///c:/Users/user/Downloads/demo%20restau/index.html).

```
[ Phase 1: Brand Concept Exploration (index_0 - index_17) ]
  ├── "Aubier" (index_0, 2, 5, 8, 11, 14, 16): Fine dining French table, wine/ivory palette
  ├── "Ledgerline" (index_4, 7, 10, 13): Freelance invoicing SaaS landing page
  └── "Marle & Co" (index_1, 3, 6, 9, 12, 15, 17): Neighborhood cafe & bakery [CHOSEN]
                                 │
[ Phase 2: Component Prototyping (index_18 - index_20) ]
  └── Introduction of 3D Coverflow carousel engine and structured menu rows
                                 │
[ Phase 3: Animation & Scroll Physics (index_21 - index_22) ]
  ├── index_21: Integrated GSAP 3 + ScrollTrigger with Croissant SVG mask zoom
  └── index_22: Dynamic Sticky Morphing Pill Navbar with IntersectionObserver
                                 │
[ Phase 4: Production Optimization & Telemetry (index_23 - index_33) ]
  ├── index_23 - 28: Inlined high-fidelity hero media (data URI) for zero-latency asset load
  ├── index_29 - 31: SVG curved side rail scroll indicator with cursor magnetic proximity
  └── index_32 - 33: Synchronized GSAP ScrollToPlugin anchor jumps & FAQ accessibility
                                 │
                                 ▼
                     [ Master Release: index.html ]
```

### Iteration Matrix

| Range | Concept Focus | Core Additions | File Size |
| :--- | :--- | :--- | :--- |
| `index_0.html` | Aubier | French culinary table, dark `#161311` palette, serif typography | 5.8 KB |
| `index_1.html` | Marle & Co | Initial neighborhood coffee layout, warm cream tokens | 5.9 KB |
| `index_4.html` | Ledgerline | SaaS fintech pricing & invoice tracking exploratory | 10.4 KB |
| `index_18.html` | Marle & Co | First 3D Coverflow stage implementation (`perspective: 1300px`) | 16.0 KB |
| `index_21.html` | Marle & Co | ScrollTrigger integration; Croissant SVG mask zoom effect | 19.3 KB |
| `index_22.html` | Marle & Co | Sticky morphing pill navigation bar (`.is-stuck` state) | 22.8 KB |
| `index_23.html` | Marle & Co | Base64 embedded photographic hero asset for instant rendering | 306 KB |
| `index_29.html` | Marle & Co | Curved SVG side rail telemetry indicator with mouse reactivity | 282 KB |
| `index_33.html` / `index.html` | **Master Release** | Full feature set, accessibility attributes, fallback smooth scrolling | 288 KB |

---

## Quickstart & Usage

The application is entirely self-contained with zero runtime build dependencies.

### Option 1: Direct File Launch

Double-click or launch [index.html](file:///c:/Users/user/Downloads/demo%20restau/index.html) in any modern browser (Chrome, Firefox, Safari, Edge).

On Windows PowerShell:
```powershell
Start-Process "c:\Users\user\Downloads\demo restau\index.html"
```

### Option 2: Local HTTP Server

To enable optimal CDN script caching and browser devtools telemetry, serve via a local HTTP server:

#### Python 3
```powershell
cd "c:\Users\user\Downloads\demo restau"
python -m http.server 8080
```
Navigate to `http://localhost:8080` in your web browser.

#### Node.js (npx)
```powershell
cd "c:\Users\user\Downloads\demo restau"
npx serve .
```

#### VS Code Live Server
Right-click [index.html](file:///c:/Users/user/Downloads/demo%20restau/index.html) in the Explorer and select **"Open with Live Server"**.

---

## Conventional Commits & ChernyCode Standards

This repository adheres strictly to **Boris Cherny's Workflow (ChernyCode)** and **Conventional Commits**:

### ChernyCode Principles
1. **Planning First**: Formulate an architectural specification before modifying source files.
2. **Deterministic Verification**: Verify syntax, responsive layouts, and console telemetry before finalizing tasks.
3. **Continuous Memory**: Document project conventions and user preferences in [AGENTS.md](file:///c:/Users/user/Downloads/demo%20restau/AGENTS.md).
4. **Code Simplification**: Eliminate dead code, unneeded dependencies, and redundant styles.
5. **Subagent Specialization**: Leverage specialized agents (`doc-generator`, `verifier`, `code-reviewer`).

### GitHub Identity
- **Configured Account**: `mahdi2983` (Enforced for all commits and remote pushes).

### Conventional Commit Format
```text
<type>(<scope>): <subject>

[optional body]

[optional footer(s)]
```

#### Supported Types
- `feat`: A new user-facing feature (e.g., Coverflow swipe support).
- `fix`: A bug fix (e.g., fixing pill nav desynchronization on mobile).
- `docs`: Documentation updates (e.g., README and ARCHITECTURE).
- `style`: Visual styling, CSS token refinement without logic change.
- `refactor`: Code restructurings without behavioral modifications.
- `perf`: Performance improvements (e.g., IntersectionObserver throttling).
- `chore`: Maintenance tasks, git configuration.

---

## Repository File Map

```text
c:\Users\user\Downloads\demo restau\
├── .gitignore              # Ignores OS, IDE, and temporary log files
├── AGENTS.md               # Boris Cherny workflow and GitHub account preferences
├── ARCHITECTURE.md         # In-depth architectural & CSS mechanics documentation
├── README.md               # Master technical project documentation
├── index.html              # PRODUCTION MASTER: Unified Marle & Co application
├── index_0.html - 33.html  # Historical iteration audit trail
└── wonderful-wedding-table-amazing-restaurant.jpg # Original master photography
```

---

*Authored for Marle & Co by technical documentation engineering.*
