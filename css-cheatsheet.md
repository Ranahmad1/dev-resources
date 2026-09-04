# CSS3 Cheatsheet

## Selectors
```css
* { }               /* All elements */
div { }             /* Element */
.class { }          /* Class */
#id { }             /* ID */
div p { }           /* Descendant */
div > p { }         /* Direct child */
div + p { }         /* Adjacent sibling */
div ~ p { }         /* General sibling */
a:hover { }         /* Pseudo-class */
p::first-line { }   /* Pseudo-element */
[type="text"] { }   /* Attribute */
```

## Box Model
```css
.box {
  width: 200px;
  height: 100px;
  padding: 10px 20px;      /* top/bottom left/right */
  margin: 10px auto;       /* center horizontally */
  border: 1px solid #ccc;
  border-radius: 8px;
  box-sizing: border-box;  /* Include padding in width */
  outline: 2px dashed red;
}
```

## Flexbox
```css
.container {
  display: flex;
  flex-direction: row;         /* row | column | row-reverse | column-reverse */
  justify-content: center;     /* flex-start | flex-end | center | space-between | space-around | space-evenly */
  align-items: center;         /* flex-start | flex-end | center | stretch | baseline */
  flex-wrap: wrap;             /* nowrap | wrap | wrap-reverse */
  gap: 16px;
}

.item {
  flex: 1;                     /* grow shrink basis */
  align-self: flex-start;
  order: 2;
}
```

## CSS Grid
```css
.grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);   /* 3 equal columns */
  grid-template-rows: auto;
  gap: 20px;
  grid-template-areas:
    "header header header"
    "sidebar main main"
    "footer footer footer";
}

.header { grid-area: header; }
.sidebar { grid-area: sidebar; }
.main { grid-area: main; }

/* Responsive grid */
grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
```

## Typography
```css
body {
  font-family: 'Segoe UI', Tahoma, sans-serif;
  font-size: 16px;
  font-weight: 400;      /* 100–900 */
  line-height: 1.6;
  letter-spacing: 0.5px;
  text-align: left;      /* center | right | justify */
  text-transform: uppercase; /* lowercase | capitalize */
  text-decoration: underline;
  color: #333;
}
```

## Colors & Backgrounds
```css
.element {
  color: #333;
  color: rgb(51, 51, 51);
  color: rgba(0, 0, 0, 0.5);
  color: hsl(200, 100%, 50%);

  background-color: #f5f5f5;
  background-image: url('bg.jpg');
  background-size: cover;       /* contain | auto */
  background-position: center;
  background-repeat: no-repeat;

  /* Gradient */
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  background: radial-gradient(circle, #fff, #000);
}
```

## Positioning
```css
.element {
  position: relative;  /* static | relative | absolute | fixed | sticky */
  top: 10px;
  left: 20px;
  z-index: 10;
}

/* Center with absolute */
.centered {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
```

## Transitions & Animations
```css
/* Transition */
.btn {
  transition: all 0.3s ease;
}
.btn:hover {
  background-color: #007bff;
  transform: scale(1.05);
}

/* Keyframe Animation */
@keyframes fadeIn {
  from { opacity: 0; transform: translateY(-20px); }
  to   { opacity: 1; transform: translateY(0); }
}

.animate {
  animation: fadeIn 0.5s ease forwards;
}
```

## Media Queries (Responsive)
```css
/* Mobile first approach */
@media (max-width: 576px) { /* xs - phones */ }
@media (max-width: 768px) { /* sm - tablets */ }
@media (max-width: 992px) { /* md - laptops */ }
@media (max-width: 1200px) { /* lg - desktops */ }

/* Example */
@media (max-width: 768px) {
  .container { flex-direction: column; }
  .sidebar { display: none; }
}
```

## CSS Variables
```css
:root {
  --primary: #007bff;
  --secondary: #6c757d;
  --font-size-base: 16px;
  --border-radius: 8px;
}

.button {
  background: var(--primary);
  border-radius: var(--border-radius);
}
```

## Glassmorphism Effect
```css
.glass {
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.2);
  border-radius: 16px;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
}
```
