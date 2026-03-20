# Anti-Patterns to Avoid

Common mistakes and code smells to watch for.

---

## Code Smells

### 1. God Objects
**Problem**: One class/file doing everything

```typescript
// BAD - God class doing too much
class UserManager {
  createUser() { }
  updateUser() { }
  deleteUser() { }
  sendEmail() { }
  logActivity() { }
  generateReport() { }
  validatePermissions() { }
  processPayment() { }
  // ... 50 more methods
}

// GOOD - Separated concerns
class UserService {
  createUser() { }
  updateUser() { }
  deleteUser() { }
}

class EmailService {
  sendEmail() { }
}

class ActivityLogger {
  logActivity() { }
}
```

### 2. Premature Optimization
**Problem**: Optimizing before there's a problem

```typescript
// BAD - Premature optimization
const memoizedComplexCalculation = useMemo(() => {
  return 2 + 2; // This doesn't need memoization!
}, []);

// GOOD - Optimize when needed
const expensiveResult = useMemo(() => {
  return largeArray.map(item => complexTransform(item));
}, [largeArray]);
```

### 3. Cargo Cult Programming
**Problem**: Copying without understanding

```typescript
// BAD - Using patterns without understanding
class UserFactory {
  createUser() {
    return new User(); // Why is this a factory?
  }
}

// GOOD - Use patterns when they solve a problem
const user = new User(); // Simple is better
```

### 4. Not Invented Here (NIH)
**Problem**: Reinventing solved problems

```typescript
// BAD - Reinventing the wheel
function myDateFormatter(date: Date) {
  const month = date.getMonth() + 1;
  const day = date.getDate();
  const year = date.getFullYear();
  return `${month}/${day}/${year}`;
  // Missing edge cases, timezones, localization...
}

// GOOD - Use established libraries
import { format } from 'date-fns';
const formatted = format(date, 'MM/dd/yyyy');
```

### 5. Analysis Paralysis
**Problem**: Overthinking simple solutions

```typescript
// BAD - Overengineered
interface Strategy {
  execute(): string;
}

class UpperCaseStrategy implements Strategy {
  execute() { return "HELLO"; }
}

class LowerCaseStrategy implements Strategy {
  execute() { return "hello"; }
}

class StringFormatter {
  constructor(private strategy: Strategy) {}
  format() { return this.strategy.execute(); }
}

// GOOD - Simple solution
const text = "Hello";
const upper = text.toUpperCase();
const lower = text.toLowerCase();
```

### 6. Magic Numbers
**Problem**: Unexplained literal values

```typescript
// BAD - Magic numbers
if (user.age > 18 && user.accountBalance < 1000) {
  // What do these numbers mean?
}

// GOOD - Named constants
const LEGAL_AGE = 18;
const MINIMUM_BALANCE = 1000;

if (user.age > LEGAL_AGE && user.accountBalance < MINIMUM_BALANCE) {
  // Clear intent
}
```

### 7. Stringly Typed
**Problem**: Using strings instead of proper types

```typescript
// BAD - Stringly typed
function getUserRole(role: string) {
  if (role === 'admin') { }
  if (role === 'user') { }
  if (role === 'moderator') { } // Typos not caught!
}

// GOOD - Proper types
type UserRole = 'admin' | 'user' | 'moderator';

function getUserRole(role: UserRole) {
  // TypeScript catches typos
}
```

### 8. Pyramid of Doom
**Problem**: Deeply nested callbacks/conditionals

```typescript
// BAD - Pyramid of doom
function processUser(user: any) {
  if (user) {
    if (user.isActive) {
      if (user.hasPermission) {
        if (user.email) {
          // Do something
        }
      }
    }
  }
}

// GOOD - Early returns
function processUser(user: User | null) {
  if (!user) return;
  if (!user.isActive) return;
  if (!user.hasPermission) return;
  if (!user.email) return;
  
  // Do something
}
```

---

## Architecture Smells

### 1. Tight Coupling
**Problem**: Components that can't work independently

```typescript
// BAD - Tight coupling
class OrderService {
  processOrder(order: Order) {
    // Directly using concrete implementation
    const payment = new PayPalPaymentProcessor();
    payment.process(order.total);
  }
}

// GOOD - Loose coupling
interface PaymentProcessor {
  process(amount: number): Promise<void>;
}

class OrderService {
  constructor(private paymentProcessor: PaymentProcessor) {}
  
  async processOrder(order: Order) {
    await this.paymentProcessor.process(order.total);
  }
}
```

### 2. Hidden Dependencies
**Problem**: Unclear what depends on what

```typescript
// BAD - Hidden dependency
function sendEmail(to: string) {
  const config = getGlobalConfig(); // Where does this come from?
  const mailer = new Mailer(config); // Hidden dependency!
  mailer.send(to);
}

// GOOD - Explicit dependencies
function sendEmail(to: string, mailer: Mailer) {
  mailer.send(to);
}
```

### 3. Circular Dependencies
**Problem**: A imports B, B imports A

```typescript
// BAD - Circular dependency
// userService.ts
import { postService } from './postService';
export const userService = { /* uses postService */ };

// postService.ts
import { userService } from './userService';
export const postService = { /* uses userService */ };

// GOOD - Break the cycle
// userService.ts
export const userService = { /* ... */ };

// postService.ts
export const postService = { /* ... */ };

// compositeService.ts
import { userService } from './userService';
import { postService } from './postService';
export const compositeService = { /* uses both */ };
```

### 4. Fat Interfaces
**Problem**: Interfaces with too many methods

```typescript
// BAD - Fat interface
interface User {
  getId(): string;
  getName(): string;
  getEmail(): string;
  updateProfile(): void;
  deleteAccount(): void;
  sendEmail(): void;
  logActivity(): void;
  generateReport(): void;
  // ... 20 more methods
}

// GOOD - Segregated interfaces
interface UserIdentity {
  getId(): string;
  getName(): string;
  getEmail(): string;
}

interface UserProfile {
  updateProfile(): void;
}

interface UserAccount {
  deleteAccount(): void;
}
```

### 5. Shotgun Surgery
**Problem**: One change requires touching many files

```typescript
// BAD - Scattered logic
// file1.ts
const TAX_RATE = 0.08;

// file2.ts
const TAX_RATE = 0.08;

// file3.ts
const TAX_RATE = 0.08;

// GOOD - Centralized
// config.ts
export const TAX_RATE = 0.08;

// Import from single source everywhere
import { TAX_RATE } from './config';
```

---

## React Anti-Patterns

### 1. Prop Drilling
**Problem**: Passing props through many layers

```typescript
// BAD - Prop drilling
<GrandParent user={user}>
  <Parent user={user}>
    <Child user={user}>
      <GrandChild user={user} />
    </Child>
  </Parent>
</GrandParent>

// GOOD - Context or composition
const UserContext = createContext<User | null>(null);

<UserContext.Provider value={user}>
  <GrandParent>
    <Parent>
      <Child>
        <GrandChild />
      </Child>
    </Parent>
  </GrandParent>
</UserContext.Provider>
```

### 2. Mutating State
**Problem**: Modifying state directly

```typescript
// BAD - Mutation
const [items, setItems] = useState([1, 2, 3]);
items.push(4); // Mutating!
setItems(items);

// GOOD - Immutable update
setItems([...items, 4]);
```

### 3. Huge Components
**Problem**: Components over 200 lines

```typescript
// BAD - 500 line component
function Dashboard() {
  // ... 500 lines of JSX and logic
}

// GOOD - Split into smaller components
function Dashboard() {
  return (
    <>
      <DashboardHeader />
      <DashboardStats />
      <DashboardCharts />
      <DashboardActivity />
    </>
  );
}
```

### 4. useEffect Abuse
**Problem**: Using useEffect for everything

```typescript
// BAD - Unnecessary useEffect
const [count, setCount] = useState(0);
const [doubleCount, setDoubleCount] = useState(0);

useEffect(() => {
  setDoubleCount(count * 2); // Don't do this!
}, [count]);

// GOOD - Derived state
const [count, setCount] = useState(0);
const doubleCount = count * 2; // Just calculate it!
```

---

## Database Anti-Patterns

### 1. N+1 Queries
**Problem**: Making separate queries in a loop

```typescript
// BAD - N+1 problem
const users = await db.user.findMany();
for (const user of users) {
  user.posts = await db.post.findMany({ where: { userId: user.id } });
}

// GOOD - Single query with join
const users = await db.user.findMany({
  include: { posts: true }
});
```

### 2. SELECT *
**Problem**: Fetching all columns when you need few

```sql
-- BAD - Fetching everything
SELECT * FROM users WHERE id = 1;

-- GOOD - Select only what you need
SELECT id, name, email FROM users WHERE id = 1;
```

### 3. Missing Indexes
**Problem**: Slow queries without indexes

```sql
-- BAD - No index on frequently queried column
SELECT * FROM users WHERE email = 'user@example.com'; -- Slow!

-- GOOD - Add index
CREATE INDEX idx_users_email ON users(email);
SELECT * FROM users WHERE email = 'user@example.com'; -- Fast!
```

---

## Security Anti-Patterns

### 1. SQL Injection
**Problem**: Building queries with string concatenation

```typescript
// BAD - SQL injection vulnerability
const query = `SELECT * FROM users WHERE email = '${email}'`;

// GOOD - Parameterized query
const query = 'SELECT * FROM users WHERE email = $1';
const result = await db.query(query, [email]);
```

### 2. XSS Vulnerabilities
**Problem**: Rendering unsanitized user input

```tsx
// BAD - XSS vulnerability
<div dangerouslySetInnerHTML={{ __html: userInput }} />

// GOOD - Let framework handle escaping
<div>{userInput}</div>
```

### 3. Storing Passwords as Plain Text
**Problem**: Not hashing passwords

```typescript
// BAD - Plain text password
const user = { email, password }; // Never store plain text!

// GOOD - Hashed password
import bcrypt from 'bcrypt';
const hashedPassword = await bcrypt.hash(password, 10);
const user = { email, password: hashedPassword };
```

---

## Performance Anti-Patterns

### 1. Blocking the Event Loop (Node.js)
**Problem**: Synchronous operations blocking everything

```typescript
// BAD - Blocking
const data = fs.readFileSync('large-file.txt'); // Blocks everything!

// GOOD - Non-blocking
const data = await fs.promises.readFile('large-file.txt');
```

### 2. Memory Leaks
**Problem**: Not cleaning up subscriptions/listeners

```typescript
// BAD - Memory leak
useEffect(() => {
  const subscription = observable.subscribe();
  // Forgot to unsubscribe!
}, []);

// GOOD - Cleanup
useEffect(() => {
  const subscription = observable.subscribe();
  return () => subscription.unsubscribe();
}, []);
```

### 3. Unnecessary Re-renders
**Problem**: Components re-rendering when they don't need to

```typescript
// BAD - Re-renders on every parent render
function Child({ onClick }: { onClick: () => void }) {
  return <button onClick={onClick}>Click</button>;
}

function Parent() {
  return <Child onClick={() => console.log('clicked')} />; // New function every render!
}

// GOOD - Stable reference
function Parent() {
  const handleClick = useCallback(() => console.log('clicked'), []);
  return <Child onClick={handleClick} />;
}
```

---

## Related Guides
- [Core Principles](./core-principles.md) - What to do instead
- [Code Aesthetics](./code-aesthetics.md) - Clean code practices
- [Security](./security.md) - Security best practices
