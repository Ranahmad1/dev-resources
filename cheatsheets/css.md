# CSS3 Cheatsheet

## Selectors
```css
*           { }   /* All elements */
div         { }   /* Element */
.class      { }   /* Class */
#id         { }   /* ID */
div p       { }   /* Descendant */
div > p     { }   /* Direct child */
div + p     { }   /* Adjacent sibling */
div ~ p     { }   /* General sibling */
a:hover     { }   /* Pseudo-class */
p::before   { }   /* Pseudo-element */
[type=text] { }   /* Attribute */
:is(h1,h2,h3) { } /* Matches any */
:not(.active) { } /* Negation */
:nth-child(2n) { } /* Even elements */
```

## Box Model
```css
.box {
  width: 200px;
  height: 100px;
  padding: 10px 20px;         /* top/bottom  left/right */
  margin: 10px auto;          /* center horizontally */
  border: 1px solid #ccc;
  border-radius: 8px;
  box-sizing: border-box;     /* include padding in width */
  outline: 2px dashed red;    /* outside border, no layout effect */
  overflow: hidden;           /* visible | hidden | scroll | auto */
}
```

## Flexbox
```css
.container {
  display: flex;
  flex-direction: row;          /* row | column | row-reverse | column-reverse */
  justify-content: center;      /* flex-start | flex-end | center | space-between | space-around | space-evenly */
  align-items: center;          /* flex-start | flex-end | center | stretch | baseline */
  align-content: flex-start;    /* when wrapping: controls row alignment */
  flex-wrap: wrap;              /* nowrap | wrap | wrap-reverse */
  gap: 16px;                    /* shorthand: row-gap column-gap */
}

.item {
  flex: 1;                      /* flex-grow flex-shrink flex-basis */
  flex-grow: 1;
  flex-shrink: 0;
  flex-basis: 200px;
  align-self: flex-start;       /* override container align-items */
  order: 2;                     /* default 0, lower = earlier */
}
```

## CSS Grid
```css
.grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);           /* 3 equal columns */
  grid-template-columns: 200px auto 1fr;           /* mixed units */
  grid-template-rows: 80px 1fr auto;
  gap: 20px;                                       /* row-gap column-gap */
  grid-template-areas:
    "header header header"
    "sidebar main   main"
    "footer  footer footer";
}

.header  { grid-area: header; }
.sidebar { grid-area: sidebar; }
.main    { grid-area: main; }

/* Placing items manually */
.item {
  grid-column: 1 / 3;   /* start / end */
  grid-row: 1 / 2;
}

/* Auto responsive — no media queries needed */
grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
```

## Typography
```css
body {
  font-family: 'Segoe UI', system-ui, -apple-system, sans-serif;
  font-size: clamp(14px, 2vw, 18px);    /* responsive font size */
  font-weight: 400;                      /* 100–900 */
  font-style: italic;
  line-height: 1.6;
  letter-spacing: 0.5px;
  word-spacing: 2px;
  text-align: left;                      /* center | right | justify */
  text-transform: uppercase;             /* lowercase | capitalize */
  text-decoration: underline;
  text-shadow: 1px 1px 2px rgba(0,0,0,0.3);
  white-space: nowrap;                   /* pre | pre-wrap | normal */
  overflow: hidden;
  text-overflow: ellipsis;               /* truncate with ... */
}
```

## Colors & Backgrounds
```css
.element {
  /* Colors */
  color: #333;
  color: rgb(51, 51, 51);
  color: rgba(0, 0, 0, 0.5);
  color: hsl(210, 100%, 50%);
  color: oklch(0.6 0.15 230);   /* modern, wide gamut */

  /* Background */
  background-color: #f5f5f5;
  background-image: url('bg.jpg');
  background-size: cover;             /* contain | auto | 100% */
  background-position: center;
  background-repeat: no-repeat;
  background-attachment: fixed;       /* parallax effect */

  /* Gradients */
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  background: radial-gradient(circle at center, #fff 0%, #000 100%);
  background: conic-gradient(from 0deg, red, yellow, green, red);
}
```

## Positioning
```css
/* static (default) | relative | absolute | fixed | sticky */

.relative { position: relative; top: 10px; left: 20px; }

.absolute {
  position: absolute;     /* relative to nearest positioned ancestor */
  top: 0; right: 0;
  z-index: 10;
}

.fixed {
  position: fixed;        /* relative to viewport */
  bottom: 20px; right: 20px;
}

.sticky {
  position: sticky;
  top: 0;                 /* sticks at top when scrolling */
}

/* Center with absolute */
.centered {
  position: absolute;
  top: 50%; left: 50%;
  transform: translate(-50%, -50%);
}

/* Center with flex (preferred) */
.flex-center {
  display: flex;
  justify-content: center;
  align-items: center;
}
```

## Transitions & Animations
```css
/* Transition */
.btn {
  background: #007bff;
  transition: background 0.3s ease, transform 0.2s ease;
}
.btn:hover {
  background: #0056b3;
  transform: scale(1.05);
}

/* Keyframe Animation */
@keyframes fadeInUp {
  from { opacity: 0; transform: translateY(30px); }
  to   { opacity: 1; transform: translateY(0); }
}

.animate {
  animation: fadeInUp 0.5s ease forwards;
  animation-delay: 0.2s;
  animation-iteration-count: 1;   /* infinite | number */
  animation-direction: normal;    /* reverse | alternate */
  animation-fill-mode: forwards;  /* none | backwards | both */
}
```

## Media Queries (Responsive)
```css
/* Mobile-first approach (recommended) */
/* Base styles = mobile */

@media (min-width: 576px)  { /* sm - large phones   */ }
@media (min-width: 768px)  { /* md - tablets        */ }
@media (min-width: 992px)  { /* lg - laptops        */ }
@media (min-width: 1200px) { /* xl - desktops       */ }
@media (min-width: 1400px) { /* xxl - large screens */ }

/* Other queries */
@media (prefers-color-scheme: dark)  { /* dark mode    */ }
@media (prefers-reduced-motion: reduce) { /* no animations */ }
@media print { /* print styles */ }

/* Example */
.container { padding: 16px; }           /* mobile */
@media (min-width: 768px) {
  .container { padding: 32px; }         /* tablet+ */
}
```

## CSS Variables (Custom Properties)
```css
:root {
  /* Colors */
  --color-primary:   #007bff;
  --color-secondary: #6c757d;
  --color-success:   #28a745;
  --color-danger:    #dc3545;
  --color-bg:        #f8f9fa;
  --color-text:      #212529;

  /* Spacing */
  --space-xs: 4px;
  --space-sm: 8px;
  --space-md: 16px;
  --space-lg: 32px;
  --space-xl: 64px;

  /* Typography */
  --font-base: 'Segoe UI', system-ui, sans-serif;
  --font-mono: 'Fira Code', 'Courier New', monospace;
  --font-size-base: 16px;

  /* Borders & Shadows */
  --radius-sm: 4px;
  --radius-md: 8px;
  --radius-lg: 16px;
  --shadow-sm: 0 1px 3px rgba(0,0,0,0.12);
  --shadow-md: 0 4px 12px rgba(0,0,0,0.15);
  --shadow-lg: 0 8px 32px rgba(0,0,0,0.2);
}

.button {
  background: var(--color-primary);
  border-radius: var(--radius-md);
  padding: var(--space-sm) var(--space-md);
  box-shadow: var(--shadow-sm);
}
```

## Glassmorphism
```css
.glass {
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  border: 1px solid rgba(255, 255, 255, 0.15);
  border-radius: 16px;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.15);
}
```

## Useful Snippets
```css
/* Smooth scroll */
html { scroll-behavior: smooth; }

/* Custom scrollbar */
::-webkit-scrollbar { width: 8px; }
::-webkit-scrollbar-track { background: #f1f1f1; }
::-webkit-scrollbar-thumb { background: #888; border-radius: 4px; }

/* Truncate text */
.truncate {
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}

/* Clamp multiline */
.clamp-3 {
  display: -webkit-box;
  -webkit-line-clamp: 3;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

/* Aspect ratio box */
.video-wrap { aspect-ratio: 16 / 9; }

/* Hide visually but keep accessible */
.sr-only {
  position: absolute;
  width: 1px; height: 1px;
  padding: 0; margin: -1px;
  overflow: hidden;
  clip: rect(0,0,0,0);
  white-space: nowrap;
  border: 0;
}
```
