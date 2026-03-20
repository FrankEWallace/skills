# Code Aesthetics

Beautiful code is maintainable code. This guide covers naming, structure, and style.

---

## Naming Conventions

### Variables

```typescript
// GOOD - Clear, specific, intentional
const userAuthenticationToken = generateToken(userId);
const isEmailVerified = user.emailVerifiedAt !== null;
const activeSubscriptionPlans = plans.filter(plan => plan.status === 'active');

// BAD - Generic, vague, lazy
const data = getToken(id);
const flag = user.email !== null;
const result = plans.filter(x => x.status === 'active');
```

### Booleans
Prefix with `is`, `has`, `can`, `should`:
- `isLoading`, `hasPermission`, `canEdit`, `shouldRetry`

### Functions/Methods
Use verb phrases that describe actions:
- `calculateTotal()`, `fetchUserData()`, `validateEmail()`
- NOT: `total()`, `user()`, `email()`

### Classes
Use nouns, PascalCase:
- `UserAuthentication`, `PaymentProcessor`, `EmailValidator`

### Constants
Use SCREAMING_SNAKE_CASE for true constants:
```typescript
const MAX_RETRY_ATTEMPTS = 3;
const API_BASE_URL = 'https://api.example.com';
const DEFAULT_TIMEOUT_MS = 5000;
```

---

## Function Design

### Principles
- **Single Responsibility**: One function, one job
- **Max 3-4 parameters**: More? Use an options object
- **Early returns**: Avoid deep nesting
- **Meaningful names**: `calculateMonthlyRevenue()` not `calc()`

### Examples

```typescript
// GOOD - Clear, focused, readable
function calculateUserDiscount(user: User, orderTotal: number): number {
  if (!user.isPremium) return 0;
  if (orderTotal < 100) return 0;
  
  return orderTotal * 0.15;
}

// BAD - Nested, unclear, hard to follow
function processUser(user: any, data: any): any {
  if (user) {
    if (user.type === 'premium') {
      if (data.total >= 100) {
        return data.total * 0.15;
      } else {
        return 0;
      }
    }
  }
  return 0;
}
```

### Function Length
- Aim for **under 20 lines**
- Max **50 lines** absolute limit
- If longer, split into smaller functions

---

## Import Organization

Organize imports in clear blocks:

```typescript
// GOOD - Organized in blocks
// 1. External dependencies
import React, { useState, useEffect } from 'react';
import { useQuery } from '@tanstack/react-query';

// 2. Internal absolute imports
import { Button } from '@/shared/components';
import { formatCurrency } from '@/shared/utils';

// 3. Relative imports
import { UserCard } from './UserCard';
import type { UserProfile } from './types';

// 4. Styles
import styles from './Dashboard.module.css';

// BAD - Random order, mixed styles
import './Dashboard.css';
import { UserCard } from './UserCard';
import React from 'react';
import type { UserProfile } from './types';
import { Button } from '@/shared/components';
```

---

## Error Handling

### Custom Error Classes

```typescript
// GOOD - Specific, actionable errors
class AuthenticationError extends Error {
  constructor(
    message: string,
    public code: string,
    public statusCode: number
  ) {
    super(message);
    this.name = 'AuthenticationError';
  }
}

throw new AuthenticationError(
  'Invalid email or password. Please try again.',
  'AUTH_INVALID_CREDENTIALS',
  401
);

// BAD - Generic, unhelpful
throw new Error('Error');
throw new Error('Something went wrong');
```

### Error Messages
- Be specific about what failed
- Explain why it failed (if known)
- Suggest how to fix it (if possible)

```typescript
// GOOD
throw new Error('Failed to upload file "report.pdf": File size (15MB) exceeds maximum allowed size (10MB). Please compress the file and try again.');

// BAD
throw new Error('Upload failed');
```

---

## Comments - When and How

### When to Comment

**DO comment:**
- WHY something is done (business logic, workarounds)
- Complex algorithms that aren't obvious
- TODOs with ticket numbers

**DON'T comment:**
- WHAT the code does (code should be self-documenting)
- Obvious operations
- Commented-out code (delete it, use git)

### Examples

```typescript
// GOOD - Explains WHY, not WHAT
// Use exponential backoff to prevent API rate limiting
await retryWithBackoff(apiCall, { maxRetries: 3 });

// Complex business logic deserves explanation
// Per accounting rules, refunds older than 90 days require manual review
if (daysSincePurchase > 90) {
  return requireManualReview(refundRequest);
}

// BAD - Restates the obvious
// Increment counter by 1
counter++;

// Get the user
const user = getUser();
```

---

## Code Documentation

For public APIs, use JSDoc/TSDoc:

```typescript
/**
 * Calculates the prorated refund amount based on subscription cancellation date.
 * 
 * Business rule: Users are entitled to a refund for unused days in their billing cycle.
 * The refund is calculated from the cancellation date to the end of the current period.
 * 
 * @param subscriptionEndDate - The date when the current billing period ends
 * @param cancellationDate - The date when the user requested cancellation
 * @param amountPaid - The total amount paid for the current billing period
 * @returns The refund amount in cents (e.g., 1550 = $15.50)
 * 
 * @example
 * ```ts
 * const refund = calculateProratedRefund(
 *   new Date('2026-04-01'),
 *   new Date('2026-03-20'),
 *   2999
 * );
 * // Returns 1000 (roughly $10 for ~11 unused days)
 * ```
 */
function calculateProratedRefund(
  subscriptionEndDate: Date,
  cancellationDate: Date,
  amountPaid: number
): number {
  // Implementation...
}
```

---

## File Naming Conventions

- **Components**: PascalCase (`UserProfile.tsx`, `PaymentForm.tsx`)
- **Utilities**: camelCase (`formatCurrency.ts`, `validateEmail.ts`)
- **Types**: PascalCase with `.types.ts` suffix (`User.types.ts`)
- **Tests**: Same name as file with `.test.ts` or `.spec.ts`
- **Hooks**: camelCase starting with `use` (`useAuth.ts`, `useLocalStorage.ts`)
- **Constants**: camelCase or SCREAMING_SNAKE_CASE (`apiEndpoints.ts`, `CONSTANTS.ts`)

---

## Formatting Standards

### Use a Formatter
- **Prettier** for JavaScript/TypeScript
- **Black** for Python
- **rustfmt** for Rust

Never argue about formatting. Let tools handle it.

### Line Length
- Aim for **80-100 characters**
- Max **120 characters**
- Break long lines logically

### Indentation
- Use **2 spaces** for JS/TS/JSON/YAML
- Use **4 spaces** for Python
- Never mix tabs and spaces

---

## Related Guides
- [Core Principles](./core-principles.md) - Fundamental philosophy
- [Frontend Development](./frontend.md) - React/Vue patterns
- [Backend Development](./backend.md) - Server-side patterns
- [Anti-Patterns](./anti-patterns.md) - What to avoid
