# Architecture & Project Structure

Clean, scalable project organization.

---

## Project Structure

### Feature-Based Organization

Organize by feature, not by type:

```
src/
├── features/          # Feature-based organization
│   ├── auth/
│   │   ├── components/
│   │   │   ├── LoginForm.tsx
│   │   │   └── RegisterForm.tsx
│   │   ├── hooks/
│   │   │   └── useAuth.ts
│   │   ├── services/
│   │   │   └── authService.ts
│   │   ├── types.ts
│   │   └── index.ts
│   ├── dashboard/
│   │   ├── components/
│   │   ├── hooks/
│   │   └── index.ts
│   └── profile/
├── shared/            # Shared utilities (not "common" or "utils")
│   ├── components/
│   │   ├── Button/
│   │   ├── Modal/
│   │   └── Input/
│   ├── hooks/
│   │   ├── useLocalStorage.ts
│   │   └── useDebounce.ts
│   └── types/
│       └── common.types.ts
├── lib/               # Third-party integrations/wrappers
│   ├── api/
│   ├── analytics/
│   └── storage/
└── config/            # Configuration files
    ├── env.ts
    └── constants.ts
```

### Why Feature-Based?
- All related code is together
- Easy to find what you need
- Features can be extracted/removed easily
- Scales better than type-based organization

---

## Layer Architecture

### Frontend Layers

```
┌─────────────────────────────────┐
│         UI Layer                │  Components, Pages
│  (React/Vue/Svelte Components)  │
├─────────────────────────────────┤
│       Business Logic            │  Hooks, State Management
│    (Custom Hooks, Stores)       │
├─────────────────────────────────┤
│       Data Access               │  API calls, Data fetching
│   (Services, API clients)       │
├─────────────────────────────────┤
│       External APIs              │  Backend, Third-party APIs
│                                 │
└─────────────────────────────────┘
```

### Backend Layers

```
┌─────────────────────────────────┐
│      Presentation Layer         │  Controllers, Routes
│     (HTTP/GraphQL/gRPC)         │
├─────────────────────────────────┤
│       Business Logic            │  Services, Use Cases
│    (Domain Logic, Rules)        │
├─────────────────────────────────┤
│       Data Access               │  Repositories, ORM
│   (Database Abstraction)        │
├─────────────────────────────────┤
│          Database               │  PostgreSQL, MongoDB, etc.
│                                 │
└─────────────────────────────────┘
```

---

## Dependency Rules

### The Dependency Rule

**Inner layers should NOT depend on outer layers.**

```
Entities (Core Business Logic)
    ↑
Use Cases (Application Logic)
    ↑
Interface Adapters (Controllers, Presenters)
    ↑
Frameworks & Drivers (UI, DB, External APIs)
```

### Example

```typescript
// GOOD - Service depends on abstraction
interface UserRepository {
  findById(id: string): Promise<User>;
}

class UserService {
  constructor(private userRepo: UserRepository) {}
  
  async getUser(id: string) {
    return this.userRepo.findById(id);
  }
}

// BAD - Service depends on concrete implementation
class UserService {
  async getUser(id: string) {
    return prisma.user.findUnique({ where: { id } }); // Tight coupling!
  }
}
```

---

## Module Organization

### Index Files

Use `index.ts` files to control exports:

```typescript
// features/auth/index.ts
export { LoginForm } from './components/LoginForm';
export { RegisterForm } from './components/RegisterForm';
export { useAuth } from './hooks/useAuth';
export type { AuthUser, LoginCredentials } from './types';

// DON'T export internal implementation details
```

### Barrel Exports

```typescript
// shared/components/index.ts
export { Button } from './Button';
export { Input } from './Input';
export { Modal } from './Modal';

// Usage
import { Button, Input, Modal } from '@/shared/components';
```

---

## Configuration Management

### Environment Variables

```typescript
// config/env.ts
import { z } from 'zod';

const envSchema = z.object({
  NODE_ENV: z.enum(['development', 'production', 'test']),
  API_URL: z.string().url(),
  API_KEY: z.string().min(1),
  DATABASE_URL: z.string().url(),
});

export const env = envSchema.parse(process.env);

// Type-safe access
console.log(env.API_URL); // TypeScript knows this is a string
```

### Constants

```typescript
// config/constants.ts
export const APP_CONFIG = {
  maxFileSize: 10 * 1024 * 1024, // 10MB
  supportedImageTypes: ['image/jpeg', 'image/png', 'image/webp'],
  paginationDefaults: {
    page: 1,
    pageSize: 20,
  },
} as const;

export const API_ENDPOINTS = {
  users: '/api/users',
  posts: '/api/posts',
  auth: {
    login: '/api/auth/login',
    logout: '/api/auth/logout',
  },
} as const;
```

---

## Monorepo Structure

For large projects:

```
my-project/
├── apps/
│   ├── web/              # Next.js app
│   ├── mobile/           # React Native app
│   └── admin/            # Admin dashboard
├── packages/
│   ├── ui/               # Shared UI components
│   ├── config/           # Shared configs
│   ├── types/            # Shared TypeScript types
│   └── utils/            # Shared utilities
├── services/
│   ├── api/              # Backend API
│   └── auth/             # Auth service
└── package.json
```

---

## API Structure

### REST API

```
api/
├── routes/
│   ├── users.ts
│   ├── posts.ts
│   └── auth.ts
├── controllers/
│   ├── UserController.ts
│   └── PostController.ts
├── services/
│   ├── UserService.ts
│   └── PostService.ts
├── repositories/
│   ├── UserRepository.ts
│   └── PostRepository.ts
├── middleware/
│   ├── auth.ts
│   ├── validation.ts
│   └── errorHandler.ts
└── types/
    └── api.types.ts
```

### GraphQL API

```
graphql/
├── schema/
│   ├── user.graphql
│   └── post.graphql
├── resolvers/
│   ├── user.ts
│   └── post.ts
├── dataSources/
│   ├── UserDataSource.ts
│   └── PostDataSource.ts
└── types/
    └── generated.ts
```

---

## Database Structure

### Migrations

```
prisma/
├── migrations/
│   ├── 20260101000000_init/
│   ├── 20260102000000_add_users/
│   └── 20260103000000_add_posts/
├── schema.prisma
└── seed.ts
```

### Naming Conventions
- Tables: plural, snake_case (`users`, `blog_posts`)
- Columns: snake_case (`created_at`, `user_id`)
- Indexes: descriptive (`idx_users_email`, `idx_posts_published_at`)

---

## Testing Structure

```
src/
├── features/
│   └── auth/
│       ├── LoginForm.tsx
│       ├── LoginForm.test.tsx      # Unit tests
│       └── LoginForm.spec.tsx      # Integration tests
└── __tests__/
    ├── e2e/                         # E2E tests
    │   └── login.e2e.ts
    └── integration/                 # Integration tests
        └── auth.integration.ts
```

---

## Documentation Structure

```
docs/
├── README.md                # Project overview
├── ARCHITECTURE.md          # Architecture decisions
├── API.md                   # API documentation
├── DEPLOYMENT.md            # Deployment guide
├── CONTRIBUTING.md          # Contribution guidelines
└── adr/                     # Architecture Decision Records
    ├── 001-use-typescript.md
    └── 002-choose-postgresql.md
```

---

## Related Guides
- [Frontend Development](./frontend.md) - Component structure
- [Backend Development](./backend.md) - API patterns
- [Code Aesthetics](./code-aesthetics.md) - File naming
