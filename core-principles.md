# Core Principles

## Philosophy

**Code is read 10x more than it's written.** Prioritize clarity, elegance, and user experience over quick hacks.

---

## 1. No AI Slop

AI-generated code often has telltale signs of being generic and thoughtless. Avoid:

- Generic variable names (`data`, `temp`, `result`, `item`, `thing`)
- Overly verbose comments that restate the code
- Defensive programming without reason (excessive null checks everywhere)
- Kitchen sink imports (importing entire libraries for one function)
- Default/placeholder names (`MyComponent`, `Component1`, `test`, `foo`, `bar`)
- Walls of text in functions (no function should exceed 50 lines)
- Copy-paste patterns with slight variations instead of abstractions
- Generic error messages ("An error occurred", "Something went wrong")
- TODO comments without tickets/issues
- Commented-out code blocks

---

## 2. Key Principles

### Clarity Over Cleverness
Write code that is obvious to read and understand. Clever one-liners that save 2 lines but take 5 minutes to understand are not worth it.

### Consistency Over Creativity
Use the same patterns throughout your codebase. Don't get creative with different ways to solve the same problem.

### Intentional Design
Every choice should be deliberate:
- Variable names should be specific
- Spacing values should follow a system
- Colors should come from a palette
- Functions should have a single, clear purpose

### Progressive Disclosure
Don't show everything at once. Reveal complexity gradually:
- Start with simple, obvious code
- Add complexity only when needed
- Hide implementation details behind clean interfaces

### Fail Fast, Fail Clearly
When something goes wrong:
- Detect it as early as possible
- Provide clear, actionable error messages
- Include context (what failed, why, what to do)

---

## 3. Universal Standards

### DRY (Don't Repeat Yourself)
If you write the same code twice, abstract it. Three times? Definitely abstract it.

```typescript
// BAD - Repeated logic
function getActiveUsers() {
  return users.filter(u => u.status === 'active' && !u.deleted);
}

function getActivePremiumUsers() {
  return users.filter(u => u.status === 'active' && !u.deleted && u.isPremium);
}

// GOOD - Abstracted logic
function getActiveUsers(filters = {}) {
  return users.filter(u => {
    if (u.status !== 'active' || u.deleted) return false;
    if (filters.isPremium !== undefined && u.isPremium !== filters.isPremium) return false;
    return true;
  });
}
```

### SOLID Principles
- **Single Responsibility**: Each module/class/function does one thing
- **Open/Closed**: Open for extension, closed for modification
- **Liskov Substitution**: Subtypes must be substitutable for their base types
- **Interface Segregation**: Many specific interfaces > one general interface
- **Dependency Inversion**: Depend on abstractions, not concretions

### KISS (Keep It Simple, Stupid)
The simplest solution that works is usually the best solution.

### YAGNI (You Aren't Gonna Need It)
Don't build features you think you might need. Build what you need now.

---

## 4. Code Quality Indicators

### Good Code
- Reads like well-written prose
- Self-documenting with clear names
- Easy to change without breaking things
- Has tests that give confidence
- Handles errors gracefully
- Performs well without premature optimization

### Bad Code
- Requires comments to explain what it does
- Hard to modify without side effects
- No tests, or tests that test implementation details
- Silent failures or cryptic errors
- Either ignores performance or over-optimizes too early

---

## 5. The Boy Scout Rule

**Leave code better than you found it.**

Every time you touch a file:
- Fix a small issue if you see one
- Improve a confusing variable name
- Add a missing test
- Extract a long function

Small improvements compound over time.

---

## Related Guides
- [Code Aesthetics](./code-aesthetics.md) - How to write beautiful code
- [Anti-Patterns](./anti-patterns.md) - What to avoid
- [AI Agent Instructions](./ai-agent-instructions.md) - For AI assistants
