# AI Agent Instructions

Specific guidance for AI coding assistants (Claude, Copilot, etc.)

---

## Before Generating Code

### 1. Read Existing Code First
- Match the style and patterns already in the codebase
- Don't introduce new patterns unless there's a good reason
- Look for existing similar implementations to follow

### 2. Ask Clarifying Questions
Don't assume. Ask when:
- Requirements are unclear
- Multiple approaches are possible
- Trade-offs exist between solutions
- You need more context about the business logic

### 3. Load Relevant Context
Based on the task, read the appropriate guides:
- **Frontend work**: Read `frontend.md`, `ui-ux-design.md`
- **Backend work**: Read `backend.md`, `security.md`
- **New project**: Read `architecture.md`, `core-principles.md`
- **Refactoring**: Read `anti-patterns.md`, `code-aesthetics.md`
- **Testing**: Read `testing.md`

---

## When Generating Code

### 1. Follow the Type System
- Use TypeScript fully - don't use `any` unless absolutely necessary
- Define proper interfaces and types
- Leverage type inference when obvious
- Use discriminated unions for complex states

```typescript
// GOOD - Proper types
interface User {
  id: string;
  name: string;
  email: string;
  role: 'admin' | 'user' | 'moderator';
}

// BAD - Using any
function processUser(user: any) {
  // Type safety lost!
}
```

### 2. Provide Context
Explain non-obvious decisions in comments:

```typescript
// Use exponential backoff to prevent API rate limiting
// after multiple failed attempts (per API documentation)
await retryWithBackoff(apiCall, { 
  maxRetries: 3, 
  baseDelay: 1000 
});
```

### 3. Suggest Alternatives
When there are trade-offs, mention them:

```typescript
// Option 1: Client-side filtering (better UX, more bandwidth)
const filtered = items.filter(item => item.active);

// Option 2: Server-side filtering (less bandwidth, more latency)
const filtered = await api.getItems({ filter: 'active' });

// Recommendation: Use Option 2 if dataset is large (>1000 items)
```

### 4. Flag Potential Issues
Call out concerns:

```typescript
// ⚠️ Security: Ensure user is authorized before accessing this data
const sensitiveData = await fetchUserFinancials(userId);

// ⚠️ Performance: This query may be slow on large datasets - consider adding an index
const users = await db.user.findMany({ where: { createdAt: { gte: date } } });
```

### 5. Write Tests Alongside Code
Don't treat tests as an afterthought:

```typescript
// Feature implementation
export function calculateDiscount(total: number, userTier: string): number {
  if (userTier === 'premium') return total * 0.15;
  if (userTier === 'standard') return total * 0.05;
  return 0;
}

// Tests (write these together)
describe('calculateDiscount', () => {
  it('applies 15% discount for premium users', () => {
    expect(calculateDiscount(100, 'premium')).toBe(15);
  });
  
  it('applies 5% discount for standard users', () => {
    expect(calculateDiscount(100, 'standard')).toBe(5);
  });
  
  it('applies no discount for free users', () => {
    expect(calculateDiscount(100, 'free')).toBe(0);
  });
});
```

### 6. Consider Edge Cases
Think about:
- Empty states (empty arrays, null values)
- Error states (network failures, validation errors)
- Loading states (async operations)
- Boundary conditions (max/min values)

```typescript
function UserList({ users }: { users: User[] }) {
  // Edge case: Loading state
  if (users === undefined) {
    return <Loading />;
  }
  
  // Edge case: Empty state
  if (users.length === 0) {
    return <EmptyState message="No users found" />;
  }
  
  // Normal case
  return (
    <ul>
      {users.map(user => <UserCard key={user.id} user={user} />)}
    </ul>
  );
}
```

---

## Code Review Focus

When reviewing or generating code, check:

### Readability
- Can someone unfamiliar with the code understand it?
- Are variable and function names clear?
- Is the logic flow obvious?
- Are there comments where needed?

### Security
- Are user inputs validated?
- Are outputs sanitized?
- Are secrets kept out of the codebase?
- Is authentication/authorization checked?

### Error Handling
- Are errors caught and handled appropriately?
- Are error messages helpful?
- Are errors logged with context?
- Can the system recover gracefully?

### Testing
- Are there unit tests?
- Do tests cover edge cases?
- Are tests testing behavior, not implementation?
- Can tests be understood without reading implementation?

### Conventions
- Does it follow the project's coding style?
- Does it match existing patterns?
- Are file names and structure consistent?
- Are imports organized properly?

### Simplicity
- Is this the simplest solution that works?
- Can it be made simpler without losing functionality?
- Are there unnecessary abstractions?
- Is it over-engineered?

---

## Common Tasks & Approaches

### Adding a New Feature

1. **Understand requirements**
   - What is the user trying to accomplish?
   - What are the edge cases?
   - Are there existing similar features?

2. **Plan the structure**
   - Where does this feature live? (feature folder)
   - What components/functions are needed?
   - What state needs to be managed?

3. **Implement incrementally**
   - Start with the happy path
   - Add error handling
   - Add edge cases
   - Add tests

4. **Review and refine**
   - Can it be simplified?
   - Are names clear?
   - Is it well-tested?

### Fixing a Bug

1. **Reproduce the bug**
   - Can you consistently trigger it?
   - What are the exact steps?

2. **Find the root cause**
   - Don't just fix symptoms
   - Trace through the code
   - Check assumptions

3. **Write a failing test**
   - Test should fail before fix
   - Test should pass after fix

4. **Fix and verify**
   - Make minimal changes
   - Ensure fix doesn't break other things
   - Update documentation if needed

### Refactoring Code

1. **Ensure tests exist**
   - If not, write them first
   - They'll catch breaking changes

2. **Make small changes**
   - One refactor at a time
   - Run tests after each change

3. **Improve incrementally**
   - Better names
   - Extract functions
   - Reduce complexity
   - Remove duplication

4. **Verify behavior unchanged**
   - All tests still pass
   - Manual testing for UI changes

---

## Anti-Patterns for AI Agents

### Don't:
- Generate code without understanding the context
- Use generic names (`data`, `temp`, `result`, `item`)
- Create overly complex solutions for simple problems
- Copy-paste with slight variations instead of abstracting
- Ignore existing patterns in the codebase
- Skip error handling
- Forget edge cases
- Write code without tests
- Use `any` type unless absolutely necessary
- Generate TODO comments without context
- Leave commented-out code

### Do:
- Ask questions when unclear
- Match existing code style
- Use specific, meaningful names
- Keep solutions simple
- Follow established patterns
- Handle errors gracefully
- Consider edge cases
- Write tests with code
- Use proper types
- Explain complex decisions
- Clean up as you go

---

## Example: Good AI-Generated Code

```typescript
/**
 * Fetches paginated user data with error handling and loading states.
 * 
 * @param page - Page number (1-indexed)
 * @param pageSize - Number of items per page
 * @returns User data with pagination metadata
 * 
 * @example
 * ```ts
 * const { data, isLoading, error } = useUsers(1, 20);
 * ```
 */
export function useUsers(page: number = 1, pageSize: number = 20) {
  const { data, isLoading, error } = useQuery({
    queryKey: ['users', page, pageSize],
    queryFn: async () => {
      // Validate inputs
      if (page < 1) {
        throw new Error('Page must be >= 1');
      }
      if (pageSize < 1 || pageSize > 100) {
        throw new Error('Page size must be between 1 and 100');
      }

      const response = await fetch(
        `/api/users?page=${page}&pageSize=${pageSize}`
      );
      
      if (!response.ok) {
        throw new Error(`Failed to fetch users: ${response.statusText}`);
      }
      
      return response.json();
    },
    // Cache for 5 minutes
    staleTime: 5 * 60 * 1000,
  });

  return {
    users: data?.users ?? [],
    totalCount: data?.totalCount ?? 0,
    hasMore: data?.hasMore ?? false,
    isLoading,
    error,
  };
}

// Tests
describe('useUsers', () => {
  it('fetches users successfully', async () => {
    // Test implementation
  });
  
  it('handles empty results', async () => {
    // Test edge case
  });
  
  it('handles API errors', async () => {
    // Test error case
  });
  
  it('validates page number', async () => {
    // Test validation
  });
});
```

---

## Related Guides
- [Core Principles](./core-principles.md) - Philosophy to follow
- [Code Aesthetics](./code-aesthetics.md) - Style guidelines
- [Anti-Patterns](./anti-patterns.md) - What to avoid
