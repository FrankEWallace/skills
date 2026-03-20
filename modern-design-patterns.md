# Modern UI Design Patterns & Aesthetics

> Inspired by contemporary design systems like Qatalog, Linear, and Vercel

---

## Design Philosophy

### Key Principles from Modern Designs

1. **Breathing Space** - Generous whitespace creates elegance
2. **Subtle Hierarchy** - Typography scale creates visual rhythm
3. **Soft Shadows** - Depth without harshness
4. **Rounded Corners** - Modern, approachable feel
5. **Muted Colors** - Professional, not overwhelming
6. **Grid-Based Layouts** - Structured but flexible

---

## Typography Scale

### Modern Scale (Based on Qatalog/Linear style)

```css
:root {
  /* Display - Hero Headlines */
  --text-display: 4.5rem;      /* 72px - Main headlines */
  --text-display-2: 3.75rem;   /* 60px - Secondary headlines */
  
  /* Headings */
  --text-h1: 3rem;             /* 48px */
  --text-h2: 2.25rem;          /* 36px */
  --text-h3: 1.875rem;         /* 30px */
  --text-h4: 1.5rem;           /* 24px */
  --text-h5: 1.25rem;          /* 20px */
  --text-h6: 1.125rem;         /* 18px */
  
  /* Body */
  --text-base: 1rem;           /* 16px */
  --text-sm: 0.875rem;         /* 14px */
  --text-xs: 0.75rem;          /* 12px */
  
  /* Line Heights */
  --leading-tight: 1.1;        /* Headings */
  --leading-normal: 1.5;       /* Body text */
  --leading-relaxed: 1.75;     /* Long-form content */
  
  /* Font Weights */
  --font-light: 300;
  --font-normal: 400;
  --font-medium: 500;
  --font-semibold: 600;
  --font-bold: 700;
}
```

### Typography Usage

```css
/* Hero Section (Qatalog style) */
.hero-title {
  font-size: var(--text-display);
  font-weight: var(--font-semibold);
  line-height: var(--leading-tight);
  letter-spacing: -0.02em;
  color: var(--color-text-primary);
}

.hero-subtitle {
  font-size: var(--text-h4);
  font-weight: var(--font-normal);
  line-height: var(--leading-normal);
  color: var(--color-text-secondary);
  margin-top: var(--space-6);
}

/* Section Headers */
.section-title {
  font-size: var(--text-h2);
  font-weight: var(--font-semibold);
  line-height: var(--leading-tight);
  letter-spacing: -0.01em;
}

/* Card Titles */
.card-title {
  font-size: var(--text-h6);
  font-weight: var(--font-medium);
  line-height: var(--leading-tight);
}

/* Body Text */
.body-text {
  font-size: var(--text-base);
  font-weight: var(--font-normal);
  line-height: var(--leading-relaxed);
  color: var(--color-text-secondary);
}

/* Caption/Labels */
.caption {
  font-size: var(--text-sm);
  font-weight: var(--font-normal);
  line-height: var(--leading-normal);
  color: var(--color-text-tertiary);
}
```

---

## Color Systems (Modern Neutrals)

### Base Palette

```css
:root {
  /* Neutral Grays (Qatalog-inspired) */
  --gray-50: #fafafa;
  --gray-100: #f5f5f5;
  --gray-200: #e5e5e5;
  --gray-300: #d4d4d4;
  --gray-400: #a3a3a3;
  --gray-500: #737373;
  --gray-600: #525252;
  --gray-700: #404040;
  --gray-800: #262626;
  --gray-900: #171717;
  
  /* Semantic Colors */
  --color-primary: #000000;
  --color-primary-hover: #262626;
  
  --color-accent: #6366f1;      /* Indigo */
  --color-accent-hover: #4f46e5;
  
  --color-success: #10b981;
  --color-warning: #f59e0b;
  --color-error: #ef4444;
  --color-info: #3b82f6;
  
  /* Text Colors */
  --color-text-primary: var(--gray-900);
  --color-text-secondary: var(--gray-600);
  --color-text-tertiary: var(--gray-500);
  --color-text-inverse: var(--gray-50);
  
  /* Background Colors */
  --color-bg-primary: #ffffff;
  --color-bg-secondary: var(--gray-50);
  --color-bg-tertiary: var(--gray-100);
  --color-bg-overlay: rgba(0, 0, 0, 0.5);
  
  /* Border Colors */
  --color-border-light: var(--gray-200);
  --color-border-medium: var(--gray-300);
  --color-border-strong: var(--gray-400);
}

/* Dark Mode */
@media (prefers-color-scheme: dark) {
  :root {
    --color-text-primary: var(--gray-50);
    --color-text-secondary: var(--gray-400);
    --color-text-tertiary: var(--gray-500);
    
    --color-bg-primary: var(--gray-900);
    --color-bg-secondary: var(--gray-800);
    --color-bg-tertiary: var(--gray-700);
    
    --color-border-light: var(--gray-800);
    --color-border-medium: var(--gray-700);
    --color-border-strong: var(--gray-600);
  }
}
```

---

## Spacing System (8px Base)

```css
:root {
  /* Spacing Scale */
  --space-0: 0;
  --space-1: 0.125rem;    /* 2px */
  --space-2: 0.25rem;     /* 4px */
  --space-3: 0.5rem;      /* 8px */
  --space-4: 0.75rem;     /* 12px */
  --space-5: 1rem;        /* 16px */
  --space-6: 1.5rem;      /* 24px */
  --space-7: 2rem;        /* 32px */
  --space-8: 2.5rem;      /* 40px */
  --space-9: 3rem;        /* 48px */
  --space-10: 4rem;       /* 64px */
  --space-11: 5rem;       /* 80px */
  --space-12: 6rem;       /* 96px */
  --space-14: 8rem;       /* 128px */
  --space-16: 10rem;      /* 160px */
  
  /* Section Spacing */
  --section-padding-sm: var(--space-10);
  --section-padding-md: var(--space-12);
  --section-padding-lg: var(--space-16);
  
  /* Container Widths */
  --container-sm: 640px;
  --container-md: 768px;
  --container-lg: 1024px;
  --container-xl: 1280px;
  --container-2xl: 1536px;
}
```

---

## Card Components (Qatalog Style)

### Base Card

```css
.card {
  background: var(--color-bg-primary);
  border: 1px solid var(--color-border-light);
  border-radius: var(--radius-2xl);
  padding: var(--space-6);
  transition: all 0.2s ease;
}

.card:hover {
  border-color: var(--color-border-medium);
  box-shadow: var(--shadow-lg);
  transform: translateY(-2px);
}

/* Card with Icon */
.card-with-icon {
  display: flex;
  gap: var(--space-5);
  align-items: flex-start;
}

.card-icon {
  width: 48px;
  height: 48px;
  border-radius: var(--radius-lg);
  background: var(--color-bg-secondary);
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}

.card-content {
  flex: 1;
}

.card-title {
  font-size: var(--text-h6);
  font-weight: var(--font-medium);
  color: var(--color-text-primary);
  margin-bottom: var(--space-2);
}

.card-description {
  font-size: var(--text-sm);
  color: var(--color-text-secondary);
  line-height: var(--leading-relaxed);
}
```

### Feature Grid (3-Column Layout)

```css
.feature-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: var(--space-6);
  margin-top: var(--space-10);
}

.feature-card {
  background: var(--color-bg-primary);
  border: 1px solid var(--color-border-light);
  border-radius: var(--radius-2xl);
  padding: var(--space-7);
  transition: all 0.2s ease;
}

.feature-card:hover {
  border-color: var(--color-border-medium);
  transform: translateY(-4px);
  box-shadow: 0 12px 24px rgba(0, 0, 0, 0.08);
}
```

---

## Shadows (Subtle Elevation)

```css
:root {
  /* Soft Shadows */
  --shadow-xs: 0 1px 2px rgba(0, 0, 0, 0.05);
  --shadow-sm: 0 1px 3px rgba(0, 0, 0, 0.08);
  --shadow-md: 0 4px 6px rgba(0, 0, 0, 0.07),
               0 2px 4px rgba(0, 0, 0, 0.06);
  --shadow-lg: 0 10px 15px rgba(0, 0, 0, 0.08),
               0 4px 6px rgba(0, 0, 0, 0.05);
  --shadow-xl: 0 20px 25px rgba(0, 0, 0, 0.1),
               0 10px 10px rgba(0, 0, 0, 0.04);
  --shadow-2xl: 0 25px 50px rgba(0, 0, 0, 0.15);
  
  /* Inner Shadow */
  --shadow-inner: inset 0 2px 4px rgba(0, 0, 0, 0.06);
  
  /* Focus Ring */
  --shadow-focus: 0 0 0 3px rgba(99, 102, 241, 0.15);
}
```

---

## Border Radius

```css
:root {
  --radius-none: 0;
  --radius-sm: 0.25rem;     /* 4px */
  --radius-md: 0.5rem;      /* 8px */
  --radius-lg: 0.75rem;     /* 12px */
  --radius-xl: 1rem;        /* 16px */
  --radius-2xl: 1.5rem;     /* 24px - Qatalog style */
  --radius-3xl: 2rem;       /* 32px */
  --radius-full: 9999px;
}
```

---

## Search Bar Component (Qatalog Inspired)

```tsx
function SearchBar() {
  return (
    <div className="search-container">
      <div className="search-bar">
        <SearchIcon className="search-icon" />
        <input
          type="text"
          placeholder="One search bar for your business"
          className="search-input"
        />
        <button className="search-button">Try me</button>
      </div>
      <p className="search-caption">
        Access real-time data across your company
      </p>
    </div>
  );
}
```

```css
.search-container {
  max-width: 720px;
  margin: 0 auto;
  text-align: center;
}

.search-bar {
  display: flex;
  align-items: center;
  gap: var(--space-3);
  background: white;
  border: 1px solid var(--color-border-light);
  border-radius: var(--radius-2xl);
  padding: var(--space-4) var(--space-6);
  box-shadow: var(--shadow-lg);
  transition: all 0.2s ease;
}

.search-bar:focus-within {
  border-color: var(--color-border-medium);
  box-shadow: var(--shadow-xl);
}

.search-icon {
  width: 20px;
  height: 20px;
  color: var(--color-text-tertiary);
  flex-shrink: 0;
}

.search-input {
  flex: 1;
  border: none;
  outline: none;
  font-size: var(--text-base);
  color: var(--color-text-primary);
  background: transparent;
}

.search-input::placeholder {
  color: var(--color-text-tertiary);
}

.search-button {
  background: var(--color-primary);
  color: white;
  border: none;
  border-radius: var(--radius-lg);
  padding: var(--space-3) var(--space-5);
  font-size: var(--text-sm);
  font-weight: var(--font-medium);
  cursor: pointer;
  transition: all 0.2s ease;
  flex-shrink: 0;
}

.search-button:hover {
  background: var(--color-primary-hover);
  transform: translateY(-1px);
}

.search-caption {
  margin-top: var(--space-4);
  font-size: var(--text-sm);
  color: var(--color-text-tertiary);
}
```

---

## Button Styles (Modern)

```css
.button {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: var(--space-2);
  padding: var(--space-3) var(--space-5);
  border-radius: var(--radius-lg);
  font-size: var(--text-sm);
  font-weight: var(--font-medium);
  border: none;
  cursor: pointer;
  transition: all 0.2s ease;
  white-space: nowrap;
}

/* Primary Button */
.button-primary {
  background: var(--color-primary);
  color: white;
}

.button-primary:hover {
  background: var(--color-primary-hover);
  transform: translateY(-1px);
  box-shadow: var(--shadow-md);
}

/* Secondary Button */
.button-secondary {
  background: var(--color-bg-secondary);
  color: var(--color-text-primary);
  border: 1px solid var(--color-border-light);
}

.button-secondary:hover {
  background: var(--color-bg-tertiary);
  border-color: var(--color-border-medium);
}

/* Ghost Button */
.button-ghost {
  background: transparent;
  color: var(--color-text-secondary);
}

.button-ghost:hover {
  background: var(--color-bg-secondary);
  color: var(--color-text-primary);
}

/* Size Variants */
.button-sm {
  padding: var(--space-2) var(--space-4);
  font-size: var(--text-xs);
}

.button-lg {
  padding: var(--space-4) var(--space-7);
  font-size: var(--text-base);
}
```

---

## Layout Patterns

### Hero Section (Qatalog Style)

```tsx
function HeroSection() {
  return (
    <section className="hero">
      <div className="hero-content">
        <h1 className="hero-title">
          One search bar for
          <br />
          your business
        </h1>
        <SearchBar />
      </div>
      <div className="hero-visual">
        {/* Illustration or screenshot */}
      </div>
    </section>
  );
}
```

```css
.hero {
  min-height: 90vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: var(--section-padding-lg) var(--space-6);
  text-align: center;
  background: linear-gradient(
    180deg,
    var(--color-bg-primary) 0%,
    var(--color-bg-secondary) 100%
  );
}

.hero-content {
  max-width: var(--container-lg);
  margin: 0 auto;
}

.hero-title {
  font-size: var(--text-display);
  font-weight: var(--font-semibold);
  line-height: var(--leading-tight);
  letter-spacing: -0.02em;
  color: var(--color-text-primary);
  margin-bottom: var(--space-10);
}

@media (max-width: 768px) {
  .hero-title {
    font-size: var(--text-display-2);
  }
}
```

### Bento Grid (Modern Card Layout)

```css
.bento-grid {
  display: grid;
  grid-template-columns: repeat(12, 1fr);
  gap: var(--space-6);
  max-width: var(--container-xl);
  margin: 0 auto;
  padding: var(--section-padding-md);
}

.bento-item {
  background: var(--color-bg-primary);
  border: 1px solid var(--color-border-light);
  border-radius: var(--radius-2xl);
  padding: var(--space-7);
  overflow: hidden;
}

/* Span variations */
.bento-large {
  grid-column: span 8;
}

.bento-medium {
  grid-column: span 6;
}

.bento-small {
  grid-column: span 4;
}

@media (max-width: 1024px) {
  .bento-large,
  .bento-medium,
  .bento-small {
    grid-column: span 12;
  }
}
```

---

## Animations

```css
/* Smooth Transitions */
:root {
  --transition-fast: 150ms ease;
  --transition-base: 200ms ease;
  --transition-slow: 300ms ease;
}

/* Fade In */
@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.fade-in {
  animation: fadeIn 0.5s ease;
}

/* Slide Up */
@keyframes slideUp {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.slide-up {
  animation: slideUp 0.6s ease;
}

/* Scale In */
@keyframes scaleIn {
  from {
    opacity: 0;
    transform: scale(0.95);
  }
  to {
    opacity: 1;
    transform: scale(1);
  }
}

.scale-in {
  animation: scaleIn 0.3s ease;
}
```

---

## Accessibility Enhancements

```css
/* Focus Visible States */
*:focus-visible {
  outline: 2px solid var(--color-accent);
  outline-offset: 2px;
  border-radius: var(--radius-sm);
}

/* Reduced Motion */
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}

/* High Contrast Mode */
@media (prefers-contrast: high) {
  :root {
    --color-border-light: var(--gray-500);
    --color-border-medium: var(--gray-600);
  }
}
```

---

## Key Takeaways from Modern Designs

### Do's:
- Use generous whitespace (2-4x more than you think)
- Soft, subtle shadows instead of hard borders
- Large, readable typography with clear hierarchy
- Rounded corners (16-24px for cards)
- Muted, professional color palette
- Smooth, subtle animations
- Clear hover states on interactive elements

### Don'ts:
- Cramped spacing
- Harsh shadows or borders
- Too many colors
- Small, hard-to-read text
- Sharp corners everywhere
- Jarring animations
- Unclear clickable areas

---

## Related Guides
- [UI/UX Design](./ui-ux-design.md) - Core design principles
- [Code Aesthetics](./code-aesthetics.md) - Code style
- [Frontend Development](./frontend.md) - Implementation
