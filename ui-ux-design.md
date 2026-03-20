# UI/UX Design Standards

Beautiful, accessible, and consistent user interfaces.

---

## Design Principles

1. **Whitespace is not wasted space** - Breathing room improves readability
2. **Consistency over creativity** - Same patterns throughout
3. **Progressive disclosure** - Don't overwhelm users
4. **Feedback on every action** - Users should never wonder "did that work?"

---

## Visual Hierarchy

Use intentional, systematic values:

```css
/* GOOD - Clear hierarchy, intentional spacing */
.card {
  padding: 1.5rem;
  border-radius: 0.75rem;
  background: white;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
  transition: box-shadow 0.2s ease;
}

.card:hover {
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
}

/* BAD - Random values, no system */
.card {
  padding: 13px;
  border-radius: 5px;
  background: #fff;
  box-shadow: 2px 2px 8px #ccc;
}
```

---

## Color Systems

### Principles
- Use a defined palette (8-12 colors max)
- Semantic colors: success (green), error (red), warning (yellow), info (blue)
- Accessibility: Minimum 4.5:1 contrast ratio for text
- Never use pure black (#000) or pure white (#fff) for text

### Example Palette

```css
:root {
  /* Primary */
  --color-primary-50: #eff6ff;
  --color-primary-500: #3b82f6;
  --color-primary-900: #1e3a8a;
  
  /* Neutral */
  --color-gray-50: #f9fafb;
  --color-gray-500: #6b7280;
  --color-gray-900: #111827;
  
  /* Semantic */
  --color-success: #10b981;
  --color-error: #ef4444;
  --color-warning: #f59e0b;
  --color-info: #3b82f6;
  
  /* Text */
  --color-text-primary: var(--color-gray-900);
  --color-text-secondary: var(--color-gray-500);
  --color-text-inverse: var(--color-gray-50);
}
```

---

## Typography

### Type Scale

Use a consistent scale (e.g., 1.250 Major Third):

```css
:root {
  --text-xs: 0.64rem;    /* 10.24px */
  --text-sm: 0.8rem;     /* 12.8px */
  --text-base: 1rem;     /* 16px */
  --text-lg: 1.25rem;    /* 20px */
  --text-xl: 1.563rem;   /* 25px */
  --text-2xl: 1.953rem;  /* 31.25px */
  --text-3xl: 2.441rem;  /* 39px */
}

/* NOT random values like 15px, 17px, 23px, 34px */
```

### Font Weights

```css
:root {
  --font-weight-normal: 400;
  --font-weight-medium: 500;
  --font-weight-semibold: 600;
  --font-weight-bold: 700;
}
```

### Font Families

```css
:root {
  --font-sans: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
  --font-mono: 'SF Mono', Monaco, 'Cascadia Code', monospace;
}
```

---

## Spacing System

Use consistent spacing scale (4px or 8px base):

```css
:root {
  --space-1: 0.25rem;  /* 4px */
  --space-2: 0.5rem;   /* 8px */
  --space-3: 0.75rem;  /* 12px */
  --space-4: 1rem;     /* 16px */
  --space-6: 1.5rem;   /* 24px */
  --space-8: 2rem;     /* 32px */
  --space-12: 3rem;    /* 48px */
  --space-16: 4rem;    /* 64px */
}
```

Apply consistently:
```css
.card {
  padding: var(--space-6);
  margin-bottom: var(--space-4);
  gap: var(--space-3);
}
```

---

## Border Radius

```css
:root {
  --radius-sm: 0.25rem;   /* 4px */
  --radius-md: 0.5rem;    /* 8px */
  --radius-lg: 0.75rem;   /* 12px */
  --radius-xl: 1rem;      /* 16px */
  --radius-full: 9999px;  /* Fully rounded */
}
```

---

## Shadows

Elevation system:

```css
:root {
  --shadow-sm: 0 1px 2px rgba(0, 0, 0, 0.05);
  --shadow-md: 0 4px 6px rgba(0, 0, 0, 0.1);
  --shadow-lg: 0 10px 15px rgba(0, 0, 0, 0.1);
  --shadow-xl: 0 20px 25px rgba(0, 0, 0, 0.15);
}
```

---

## Animations & Transitions

### Duration

```css
:root {
  --duration-fast: 150ms;
  --duration-normal: 300ms;
  --duration-slow: 500ms;
}
```

### Easing

```css
:root {
  --ease-in: cubic-bezier(0.4, 0, 1, 1);
  --ease-out: cubic-bezier(0, 0, 0.2, 1);
  --ease-in-out: cubic-bezier(0.4, 0, 0.2, 1);
}
```

### Usage

```css
.button {
  transition: all var(--duration-normal) var(--ease-out);
}

.modal {
  animation: fadeIn var(--duration-fast) var(--ease-out);
}
```

---

## Responsive Design

### Breakpoints

```css
:root {
  --screen-sm: 640px;
  --screen-md: 768px;
  --screen-lg: 1024px;
  --screen-xl: 1280px;
  --screen-2xl: 1536px;
}

/* Mobile first */
.container {
  padding: var(--space-4);
}

@media (min-width: 768px) {
  .container {
    padding: var(--space-8);
  }
}
```

---

## Accessibility

### Color Contrast
- **Normal text**: 4.5:1 minimum
- **Large text**: 3:1 minimum
- **UI components**: 3:1 minimum

### Focus States
Always visible, never `outline: none` without alternative:

```css
/* GOOD */
.button:focus-visible {
  outline: 2px solid var(--color-primary-500);
  outline-offset: 2px;
}

/* BAD */
.button:focus {
  outline: none; /* Never do this without alternative */
}
```

### Touch Targets
Minimum 44x44px for interactive elements:

```css
.button {
  min-height: 44px;
  min-width: 44px;
  padding: var(--space-3) var(--space-6);
}
```

---

## Component Patterns

### Loading States

```tsx
{isLoading ? (
  <Skeleton />
) : (
  <Content data={data} />
)}
```

### Empty States

```tsx
{items.length === 0 ? (
  <EmptyState
    icon={<InboxIcon />}
    title="No items yet"
    description="Create your first item to get started."
    action={<Button onClick={onCreate}>Create Item</Button>}
  />
) : (
  <ItemList items={items} />
)}
```

### Error States

```tsx
{error ? (
  <ErrorState
    title="Something went wrong"
    message={error.message}
    action={<Button onClick={retry}>Try Again</Button>}
  />
) : (
  <Content />
)}
```

---

## Design Tokens

Use CSS variables or design tokens for consistency:

```typescript
// tokens.ts
export const tokens = {
  colors: {
    primary: {
      50: '#eff6ff',
      500: '#3b82f6',
      900: '#1e3a8a',
    },
  },
  spacing: {
    1: '0.25rem',
    2: '0.5rem',
    4: '1rem',
  },
  typography: {
    fontSize: {
      sm: '0.875rem',
      base: '1rem',
      lg: '1.125rem',
    },
  },
};
```

---

## Related Guides
- [Frontend Development](./frontend.md) - Component implementation
- [Accessibility](./accessibility.md) - Detailed a11y guidelines
- [Code Aesthetics](./code-aesthetics.md) - Code style
