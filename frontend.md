# Frontend Development

React, Vue, and modern web development patterns.

---

## Component Principles

1. **Small and focused** - If it's over 200 lines, split it
2. **Declarative** - What, not how
3. **Composable** - Like LEGO blocks
4. **Self-documenting** - Props should explain themselves

---

## React Patterns

### Component Structure

```typescript
// GOOD - Clean, typed, obvious
interface UserCardProps {
  user: {
    id: string;
    name: string;
    email: string;
    avatarUrl?: string;
  };
  onEdit?: () => void;
  onDelete?: () => void;
  isLoading?: boolean;
}

export function UserCard({ 
  user, 
  onEdit, 
  onDelete, 
  isLoading = false 
}: UserCardProps) {
  if (isLoading) {
    return <UserCardSkeleton />;
  }

  return (
    <article className="user-card">
      <Avatar src={user.avatarUrl} alt={user.name} />
      <div className="user-info">
        <h3>{user.name}</h3>
        <p>{user.email}</p>
      </div>
      {(onEdit || onDelete) && (
        <div className="actions">
          {onEdit && <Button onClick={onEdit}>Edit</Button>}
          {onDelete && <Button onClick={onDelete} variant="danger">Delete</Button>}
        </div>
      )}
    </article>
  );
}

// BAD - Untyped, unclear, doing too much
export function Card(props: any) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(false);
  
  useEffect(() => {
    // Fetching data in a presentational component?
    fetchUser(props.id).then(setData);
  }, []);
  
  // ... 150 more lines of mixed concerns
}
```

### Separation of Concerns

**Container/Presentational Pattern:**

```typescript
// UserProfileContainer.tsx - Logic
export function UserProfileContainer({ userId }: { userId: string }) {
  const { data: user, isLoading, error } = useQuery({
    queryKey: ['user', userId],
    queryFn: () => fetchUser(userId),
  });

  if (isLoading) return <UserProfileSkeleton />;
  if (error) return <ErrorState error={error} />;
  if (!user) return <NotFound />;

  return <UserProfile user={user} />;
}

// UserProfile.tsx - Presentation
interface UserProfileProps {
  user: User;
}

export function UserProfile({ user }: UserProfileProps) {
  return (
    <div className="user-profile">
      <h1>{user.name}</h1>
      <p>{user.bio}</p>
    </div>
  );
}
```

---

## State Management

### Principles
1. **Colocate state** - Keep state as close to where it's used as possible
2. **Derive, don't duplicate** - Calculate values from existing state
3. **Immutable updates** - Never mutate state directly
4. **Single source of truth** - Each piece of data has one home

### Local State

```typescript
// GOOD - Derived state, clear structure
interface CartState {
  items: CartItem[];
}

function useCart() {
  const [state, setState] = useState<CartState>({ items: [] });
  
  // Derived values
  const itemCount = state.items.reduce((sum, item) => sum + item.quantity, 0);
  const subtotal = state.items.reduce((sum, item) => sum + item.price * item.quantity, 0);
  const tax = subtotal * 0.08;
  const total = subtotal + tax;
  
  return { ...state, itemCount, subtotal, tax, total };
}

// BAD - Duplicated data, mutation
function useCart() {
  const [items, setItems] = useState([]);
  const [count, setCount] = useState(0);        // Duplicate!
  const [total, setTotal] = useState(0);        // Duplicate!
  
  function addItem(item) {
    items.push(item);                           // Mutation!
    setCount(count + 1);                        // Manual sync
    setTotal(total + item.price);               // Manual sync
  }
}
```

### Global State

Use context for truly global state:

```typescript
// AuthContext.tsx
interface AuthContextValue {
  user: User | null;
  login: (credentials: Credentials) => Promise<void>;
  logout: () => void;
  isAuthenticated: boolean;
}

const AuthContext = createContext<AuthContextValue | null>(null);

export function AuthProvider({ children }: { children: ReactNode }) {
  const [user, setUser] = useState<User | null>(null);
  
  const login = async (credentials: Credentials) => {
    const user = await authService.login(credentials);
    setUser(user);
  };
  
  const logout = () => {
    authService.logout();
    setUser(null);
  };
  
  const value = {
    user,
    login,
    logout,
    isAuthenticated: user !== null,
  };
  
  return <AuthContext.Provider value={value}>{children}</AuthContext.Provider>;
}

export function useAuth() {
  const context = useContext(AuthContext);
  if (!context) {
    throw new Error('useAuth must be used within AuthProvider');
  }
  return context;
}
```

---

## Custom Hooks

### Naming
Always start with `use`:
- `useAuth`, `useLocalStorage`, `useDebounce`

### Examples

```typescript
// useLocalStorage.ts
export function useLocalStorage<T>(key: string, initialValue: T) {
  const [value, setValue] = useState<T>(() => {
    const stored = localStorage.getItem(key);
    return stored ? JSON.parse(stored) : initialValue;
  });

  useEffect(() => {
    localStorage.setItem(key, JSON.stringify(value));
  }, [key, value]);

  return [value, setValue] as const;
}

// useDebounce.ts
export function useDebounce<T>(value: T, delay: number): T {
  const [debouncedValue, setDebouncedValue] = useState(value);

  useEffect(() => {
    const timer = setTimeout(() => setDebouncedValue(value), delay);
    return () => clearTimeout(timer);
  }, [value, delay]);

  return debouncedValue;
}
```

---

## Performance Optimization

### Memoization

```typescript
// useMemo for expensive calculations
const sortedUsers = useMemo(
  () => users.sort((a, b) => a.name.localeCompare(b.name)),
  [users]
);

// useCallback for stable function references
const handleSubmit = useCallback((data: FormData) => {
  submitForm(data);
}, []);

// React.memo for component memoization
export const ExpensiveComponent = memo(function ExpensiveComponent({ data }: Props) {
  // Only re-renders when data changes
  return <div>{/* ... */}</div>;
});
```

### Code Splitting

```typescript
// Lazy loading routes
const Dashboard = lazy(() => import('./features/dashboard/Dashboard'));
const Settings = lazy(() => import('./features/settings/Settings'));

function App() {
  return (
    <Suspense fallback={<Loading />}>
      <Routes>
        <Route path="/dashboard" element={<Dashboard />} />
        <Route path="/settings" element={<Settings />} />
      </Routes>
    </Suspense>
  );
}
```

---

## Forms

### Controlled Components

```typescript
function LoginForm() {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  const [errors, setErrors] = useState<{ email?: string; password?: string }>({});

  const handleSubmit = async (e: FormEvent) => {
    e.preventDefault();
    
    // Validation
    const newErrors: typeof errors = {};
    if (!email) newErrors.email = 'Email is required';
    if (!password) newErrors.password = 'Password is required';
    
    if (Object.keys(newErrors).length > 0) {
      setErrors(newErrors);
      return;
    }

    // Submit
    try {
      await login({ email, password });
    } catch (error) {
      setErrors({ email: 'Invalid credentials' });
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <Input
        type="email"
        value={email}
        onChange={(e) => setEmail(e.target.value)}
        error={errors.email}
        label="Email"
      />
      <Input
        type="password"
        value={password}
        onChange={(e) => setPassword(e.target.value)}
        error={errors.password}
        label="Password"
      />
      <Button type="submit">Log In</Button>
    </form>
  );
}
```

### Form Libraries

For complex forms, use libraries:
- **React Hook Form** - Performance-focused
- **Formik** - Full-featured
- **Zod** - Schema validation

```typescript
import { useForm } from 'react-hook-form';
import { zodResolver } from '@hookform/resolvers/zod';
import { z } from 'zod';

const loginSchema = z.object({
  email: z.string().email('Invalid email'),
  password: z.string().min(8, 'Password must be at least 8 characters'),
});

type LoginFormData = z.infer<typeof loginSchema>;

function LoginForm() {
  const { register, handleSubmit, formState: { errors } } = useForm<LoginFormData>({
    resolver: zodResolver(loginSchema),
  });

  const onSubmit = async (data: LoginFormData) => {
    await login(data);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <Input {...register('email')} error={errors.email?.message} />
      <Input {...register('password')} type="password" error={errors.password?.message} />
      <Button type="submit">Log In</Button>
    </form>
  );
}
```

---

## Data Fetching

### React Query (Recommended)

```typescript
import { useQuery, useMutation, useQueryClient } from '@tanstack/react-query';

function UserProfile({ userId }: { userId: string }) {
  const queryClient = useQueryClient();
  
  // Fetch data
  const { data: user, isLoading, error } = useQuery({
    queryKey: ['user', userId],
    queryFn: () => fetchUser(userId),
    staleTime: 5 * 60 * 1000, // 5 minutes
  });

  // Mutation
  const updateMutation = useMutation({
    mutationFn: (data: Partial<User>) => updateUser(userId, data),
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['user', userId] });
    },
  });

  if (isLoading) return <Loading />;
  if (error) return <Error error={error} />;
  if (!user) return <NotFound />;

  return (
    <div>
      <h1>{user.name}</h1>
      <Button onClick={() => updateMutation.mutate({ name: 'New Name' })}>
        Update
      </Button>
    </div>
  );
}
```

---

## Accessibility

### Semantic HTML

```tsx
// GOOD - Semantic
<button onClick={handleClick}>Click me</button>
<nav><a href="/about">About</a></nav>

// BAD - Non-semantic
<div onClick={handleClick}>Click me</div>
<div><span onClick={() => navigate('/about')}>About</span></div>
```

### ARIA Attributes

```tsx
// GOOD - Accessible
<button 
  onClick={handleDelete}
  aria-label="Delete item from cart"
>
  <TrashIcon aria-hidden="true" />
</button>

// BAD - Not accessible
<div onClick={handleDelete}>
  <TrashIcon />
</div>
```

### Keyboard Navigation

```tsx
function Modal({ isOpen, onClose, children }: ModalProps) {
  useEffect(() => {
    const handleEscape = (e: KeyboardEvent) => {
      if (e.key === 'Escape') onClose();
    };
    
    if (isOpen) {
      document.addEventListener('keydown', handleEscape);
      return () => document.removeEventListener('keydown', handleEscape);
    }
  }, [isOpen, onClose]);

  if (!isOpen) return null;

  return (
    <div role="dialog" aria-modal="true">
      {children}
    </div>
  );
}
```

---

## Related Guides
- [UI/UX Design](./ui-ux-design.md) - Design systems
- [Testing](./testing.md) - Component testing
- [Performance](./performance.md) - Optimization
- [Code Aesthetics](./code-aesthetics.md) - Code style
