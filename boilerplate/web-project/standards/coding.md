# Coding Standards - [Project Name]

> **AI Context**: This is the single source of truth for code conventions.
> For current status: see `../docs/status/progress.yaml`
> For project overview: see `../docs/index.md`

## Code Style & Conventions

> Note: Examples reference common React/TypeScript tooling but are adaptable. Replace libraries and patterns to match your chosen stack (e.g., Vue/Nuxt, SvelteKit, Angular) while keeping the same quality bar and structure.

### General Principles

- **Readability**: Code should be self-documenting
- **Consistency**: Follow established patterns
- **Maintainability**: Write code for future developers
- **Performance**: Optimize for user experience
- **Security**: Implement secure coding practices

### Naming Conventions

#### Files & Directories
- **Components**: PascalCase (`UserProfile.tsx`)
- **Hooks**: camelCase (`useUserData.ts`)
- **Utilities**: camelCase (`formatDate.ts`)
- **Constants**: UPPER_SNAKE_CASE (`API_ENDPOINTS`)
- **Types**: PascalCase (`UserProfile.ts`)
- **Pages**: kebab-case (`user-profile.tsx`)
- **Styles**: kebab-case (`user-profile.module.css`)

#### Variables & Functions
- **Variables**: camelCase (`userName`, `isLoading`)
- **Functions**: camelCase (`getUserData`, `handleSubmit`)
- **Boolean**: Prefix with `is`, `has`, `can` (`isVisible`, `hasPermission`)
- **Constants**: UPPER_SNAKE_CASE (`MAX_RETRY_ATTEMPTS`)
- **Classes**: PascalCase (`UserService`)
- **Interfaces**: PascalCase with `I` prefix (`IUserProfile`)

## TypeScript Standards

### Type Definitions

```typescript
// Prefer interfaces for object shapes
interface UserProfile {
  id: string;
  email: string;
  name: string;
  role: UserRole;
  createdAt: Date;
  updatedAt: Date;
}

// Use enums for fixed sets of values
enum UserRole {
  ADMIN = 'admin',
  CLIENT = 'client',
  AGENCY = 'agency'
}

// Use type aliases for unions and complex types
type ApiResponse<T> = {
  data: T;
  error?: string;
  status: 'success' | 'error';
  timestamp: Date;
};

// Use generics for reusable types
interface Repository<T> {
  findById(id: string): Promise<T | null>;
  create(data: Omit<T, 'id'>): Promise<T>;
  update(id: string, data: Partial<T>): Promise<T>;
  delete(id: string): Promise<void>;
}

// Use utility types effectively
type CreateUserRequest = Omit<UserProfile, 'id' | 'createdAt' | 'updatedAt'>;
type UpdateUserRequest = Partial<Pick<UserProfile, 'name' | 'email'>>;
type UserResponse = Pick<UserProfile, 'id' | 'name' | 'email'>;
```

### Strict Mode Configuration

```json
// tsconfig.json
{
  "compilerOptions": {
    "strict": true,
    "noImplicitAny": true,
    "strictNullChecks": true,
    "strictFunctionTypes": true,
    "noImplicitReturns": true,
    "noFallthroughCasesInSwitch": true,
    "noUncheckedIndexedAccess": true
  }
}
```

### Type Safety Best Practices

```typescript
// Use explicit return types for public functions
export const calculateTotal = (items: CartItem[]): number => {
  return items.reduce((total, item) => total + item.price * item.quantity, 0);
};

// Avoid 'any' type - use 'unknown' or proper types
const handleApiResponse = (response: unknown): ApiResponse<User> => {
  if (isValidApiResponse(response)) {
    return response;
  }
  throw new Error('Invalid API response format');
};

// Use type guards for runtime type checking
function isValidUser(obj: unknown): obj is User {
  return (
    typeof obj === 'object' &&
    obj !== null &&
    'id' in obj &&
    'email' in obj &&
    'name' in obj
  );
}

// Use discriminated unions for state management
type LoadingState = {
  status: 'loading';
};

type SuccessState = {
  status: 'success';
  data: User[];
};

type ErrorState = {
  status: 'error';
  error: string;
};

type UserState = LoadingState | SuccessState | ErrorState;
```

## React & Next.js Standards

### Component Structure

```typescript
// Functional components with proper typing
interface ComponentProps {
  title: string;
  onAction: () => void;
  disabled?: boolean;
  children?: React.ReactNode;
}

export const Component: React.FC<ComponentProps> = ({
  title,
  onAction,
  disabled = false,
  children
}) => {
  // Hooks at the top
  const [state, setState] = useState<string>('');
  const [isLoading, setIsLoading] = useState<boolean>(false);
  
  // Custom hooks
  const { data, error } = useUserData();
  
  // Event handlers with useCallback
  const handleClick = useCallback(() => {
    if (!disabled && !isLoading) {
      onAction();
    }
  }, [onAction, disabled, isLoading]);
  
  // Memoized values
  const memoizedValue = useMemo(() => {
    return expensiveCalculation(data);
  }, [data]);
  
  // Early returns for loading/error states
  if (isLoading) {
    return <LoadingSpinner />;
  }
  
  if (error) {
    return <ErrorMessage error={error} />;
  }
  
  // Main render
  return (
    <div className="component">
      <h2>{title}</h2>
      <button 
        onClick={handleClick} 
        disabled={disabled}
        className="btn btn-primary"
      >
        Action
      </button>
      {children}
    </div>
  );
};

// Export with display name for debugging
Component.displayName = 'Component';
```

### Hooks Usage

```typescript
// Custom hooks for reusable logic
export const useUserData = (userId: string) => {
  const [user, setUser] = useState<User | null>(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);
  
  useEffect(() => {
    const fetchUser = async () => {
      try {
        setLoading(true);
        const userData = await userService.getUser(userId);
        setUser(userData);
      } catch (err) {
        setError(err instanceof Error ? err.message : 'Unknown error');
      } finally {
        setLoading(false);
      }
    };
    
    fetchUser();
  }, [userId]);
  
  return { user, loading, error };
};

// Hook with cleanup
export const useEventListener = (
  eventName: string,
  handler: (event: Event) => void,
  element: Element | Window = window
) => {
  useEffect(() => {
    element.addEventListener(eventName, handler);
    
    return () => {
      element.removeEventListener(eventName, handler);
    };
  }, [eventName, handler, element]);
};
```

### State Management

```typescript
// Redux Toolkit slice example
import { createSlice, createAsyncThunk, PayloadAction } from '@reduxjs/toolkit';

interface UserState {
  users: User[];
  loading: boolean;
  error: string | null;
}

const initialState: UserState = {
  users: [],
  loading: false,
  error: null
};

// Async thunk for API calls
export const fetchUsers = createAsyncThunk(
  'users/fetchUsers',
  async (_, { rejectWithValue }) => {
    try {
      const response = await userService.getUsers();
      return response.data;
    } catch (error) {
      return rejectWithValue(error.message);
    }
  }
);

const userSlice = createSlice({
  name: 'users',
  initialState,
  reducers: {
    clearError: (state) => {
      state.error = null;
    }
  },
  extraReducers: (builder) => {
    builder
      .addCase(fetchUsers.pending, (state) => {
        state.loading = true;
        state.error = null;
      })
      .addCase(fetchUsers.fulfilled, (state, action) => {
        state.loading = false;
        state.users = action.payload;
      })
      .addCase(fetchUsers.rejected, (state, action) => {
        state.loading = false;
        state.error = action.payload as string;
      });
  }
});

export const { clearError } = userSlice.actions;
export default userSlice.reducer;
```

## API & Data Fetching Standards

### GraphQL Operations

```typescript
// Typed GraphQL operations
import { gql } from '@apollo/client';

const GET_USERS = gql`
  query GetUsers($filter: UserFilter) {
    users(filter: $filter) {
      id
      name
      email
      role
      createdAt
    }
  }
`;

const CREATE_USER = gql`
  mutation CreateUser($input: CreateUserInput!) {
    createUser(input: $input) {
      id
      name
      email
      role
    }
  }
`;

// Typed hooks for GraphQL
export const useUsers = (filter?: UserFilter) => {
  const { data, loading, error, refetch } = useQuery(GET_USERS, {
    variables: { filter },
    errorPolicy: 'all'
  });
  
  return {
    users: data?.users || [],
    loading,
    error,
    refetch
  };
};

export const useCreateUser = () => {
  const [createUser, { loading, error }] = useMutation(CREATE_USER, {
    refetchQueries: [GET_USERS],
    onError: (error) => {
      console.error('Failed to create user:', error);
    }
  });
  
  return { createUser, loading, error };
};
```

### REST API Standards

```typescript
// API service with proper error handling
class ApiService {
  private baseURL: string;
  
  constructor(baseURL: string) {
    this.baseURL = baseURL;
  }
  
  async get<T>(endpoint: string): Promise<ApiResponse<T>> {
    try {
      const response = await fetch(`${this.baseURL}${endpoint}`, {
        method: 'GET',
        headers: this.getHeaders()
      });
      
      if (!response.ok) {
        throw new Error(`HTTP error! status: ${response.status}`);
      }
      
      const data = await response.json();
      return { data, status: 'success', timestamp: new Date() };
    } catch (error) {
      return {
        data: null as T,
        error: error instanceof Error ? error.message : 'Unknown error',
        status: 'error',
        timestamp: new Date()
      };
    }
  }
  
  async post<T>(endpoint: string, data: unknown): Promise<ApiResponse<T>> {
    try {
      const response = await fetch(`${this.baseURL}${endpoint}`, {
        method: 'POST',
        headers: this.getHeaders(),
        body: JSON.stringify(data)
      });
      
      if (!response.ok) {
        throw new Error(`HTTP error! status: ${response.status}`);
      }
      
      const responseData = await response.json();
      return { data: responseData, status: 'success', timestamp: new Date() };
    } catch (error) {
      return {
        data: null as T,
        error: error instanceof Error ? error.message : 'Unknown error',
        status: 'error',
        timestamp: new Date()
      };
    }
  }
  
  private getHeaders(): HeadersInit {
    return {
      'Content-Type': 'application/json',
      'Authorization': `Bearer ${this.getToken()}`
    };
  }
  
  private getToken(): string {
    return localStorage.getItem('authToken') || '';
  }
}
```

## Styling Standards

### CSS & TailwindCSS

```typescript
// Use consistent class organization
const buttonClasses = [
  // Base styles
  'px-4 py-2 rounded-md font-medium',
  // Color and background
  'bg-blue-600 text-white',
  // Hover states
  'hover:bg-blue-700 hover:shadow-md',
  // Disabled states
  'disabled:opacity-50 disabled:cursor-not-allowed',
  // Transitions
  'transition-colors duration-200',
  // Focus states
  'focus:outline-none focus:ring-2 focus:ring-blue-500'
].join(' ');

// Component with conditional styling
interface ButtonProps {
  variant: 'primary' | 'secondary' | 'danger';
  size: 'sm' | 'md' | 'lg';
  disabled?: boolean;
}

const getButtonClasses = ({ variant, size, disabled }: ButtonProps) => {
  const baseClasses = 'font-medium rounded-md transition-colors duration-200';
  
  const variantClasses = {
    primary: 'bg-blue-600 text-white hover:bg-blue-700',
    secondary: 'bg-gray-200 text-gray-900 hover:bg-gray-300',
    danger: 'bg-red-600 text-white hover:bg-red-700'
  };
  
  const sizeClasses = {
    sm: 'px-3 py-1.5 text-sm',
    md: 'px-4 py-2 text-base',
    lg: 'px-6 py-3 text-lg'
  };
  
  const disabledClasses = disabled 
    ? 'opacity-50 cursor-not-allowed' 
    : 'focus:outline-none focus:ring-2';
  
  return [
    baseClasses,
    variantClasses[variant],
    sizeClasses[size],
    disabledClasses
  ].join(' ');
};
```

### CSS Modules (Alternative)

```css
/* Button.module.css */
.button {
  @apply px-4 py-2 rounded-md font-medium transition-colors duration-200;
}

.primary {
  @apply bg-blue-600 text-white hover:bg-blue-700;
}

.secondary {
  @apply bg-gray-200 text-gray-900 hover:bg-gray-300;
}

.disabled {
  @apply opacity-50 cursor-not-allowed;
}
```

## Testing Standards

### Unit Testing

```typescript
// Component testing with React Testing Library
import { render, screen, fireEvent, waitFor } from '@testing-library/react';
import { UserProfile } from './UserProfile';

describe('UserProfile', () => {
  const mockUser: User = {
    id: '1',
    name: 'John Doe',
    email: 'john@example.com',
    role: UserRole.CLIENT,
    createdAt: new Date('2023-01-01'),
    updatedAt: new Date('2023-01-01')
  };
  
  it('should render user information correctly', () => {
    render(<UserProfile user={mockUser} />);
    
    expect(screen.getByText('John Doe')).toBeInTheDocument();
    expect(screen.getByText('john@example.com')).toBeInTheDocument();
    expect(screen.getByText('CLIENT')).toBeInTheDocument();
  });
  
  it('should call onEdit when edit button is clicked', async () => {
    const mockOnEdit = jest.fn();
    render(<UserProfile user={mockUser} onEdit={mockOnEdit} />);
    
    const editButton = screen.getByRole('button', { name: /edit/i });
    fireEvent.click(editButton);
    
    await waitFor(() => {
      expect(mockOnEdit).toHaveBeenCalledWith(mockUser.id);
    });
  });
  
  it('should handle loading state', () => {
    render(<UserProfile user={mockUser} loading={true} />);
    
    expect(screen.getByTestId('loading-spinner')).toBeInTheDocument();
  });
});
```

### Integration Testing

```typescript
// API integration testing
import { ApiService } from './ApiService';

describe('ApiService', () => {
  let apiService: ApiService;
  
  beforeEach(() => {
    apiService = new ApiService('http://localhost:3000/api');
  });
  
  it('should fetch users successfully', async () => {
    const mockUsers = [
      { id: '1', name: 'John Doe', email: 'john@example.com' }
    ];
    
    global.fetch = jest.fn().mockResolvedValue({
      ok: true,
      json: () => Promise.resolve(mockUsers)
    });
    
    const result = await apiService.get<User[]>('/users');
    
    expect(result.status).toBe('success');
    expect(result.data).toEqual(mockUsers);
    expect(result.error).toBeUndefined();
  });
  
  it('should handle API errors gracefully', async () => {
    global.fetch = jest.fn().mockResolvedValue({
      ok: false,
      status: 500
    });
    
    const result = await apiService.get<User[]>('/users');
    
    expect(result.status).toBe('error');
    expect(result.data).toBeNull();
    expect(result.error).toBeDefined();
  });
});
```

### E2E Testing

```typescript
// Playwright E2E test
import { test, expect } from '@playwright/test';

test('user can login and view profile', async ({ page }) => {
  // Navigate to login page
  await page.goto('/login');
  
  // Fill in login form
  await page.fill('[data-testid="email-input"]', 'test@example.com');
  await page.fill('[data-testid="password-input"]', 'password123');
  
  // Submit form
  await page.click('[data-testid="login-button"]');
  
  // Wait for redirect to dashboard
  await page.waitForURL('/dashboard');
  
  // Verify user is logged in
  await expect(page.locator('[data-testid="user-menu"]')).toBeVisible();
  
  // Navigate to profile
  await page.click('[data-testid="user-menu"]');
  await page.click('[data-testid="profile-link"]');
  
  // Verify profile page loads
  await expect(page.locator('h1')).toContainText('Profile');
});
```

## Performance Standards

### Optimization Techniques

```typescript
// React.memo for expensive components
export const ExpensiveComponent = React.memo<Props>(({ data, onAction }) => {
  const processedData = useMemo(() => {
    return data.map(item => expensiveProcessing(item));
  }, [data]);
  
  const handleAction = useCallback((id: string) => {
    onAction(id);
  }, [onAction]);
  
  return (
    <div>
      {processedData.map(item => (
        <Item key={item.id} data={item} onAction={handleAction} />
      ))}
    </div>
  );
});

// Lazy loading for routes
const LazyDashboard = lazy(() => import('./Dashboard'));
const LazyProfile = lazy(() => import('./Profile'));

// Virtual scrolling for large lists
import { FixedSizeList as List } from 'react-window';

const VirtualizedList = ({ items }: { items: Item[] }) => (
  <List
    height={600}
    itemCount={items.length}
    itemSize={50}
    itemData={items}
  >
    {({ index, style, data }) => (
      <div style={style}>
        <ItemComponent item={data[index]} />
      </div>
    )}
  </List>
);
```

### Bundle Optimization

```typescript
// Dynamic imports for code splitting
const loadFeature = async () => {
  const { FeatureComponent } = await import('./Feature');
  return FeatureComponent;
};

// Tree shaking friendly exports
export { Button } from './Button';
export { Input } from './Input';
export type { ButtonProps } from './Button';

// Avoid default exports for better tree shaking
// ❌ Bad
export default MyComponent;

// ✅ Good
export const MyComponent = () => { /* ... */ };
```

## Security Standards

### Input Validation

```typescript
// Input validation with Zod
import { z } from 'zod';

const UserSchema = z.object({
  name: z.string().min(1, 'Name is required').max(100, 'Name too long'),
  email: z.string().email('Invalid email format'),
  age: z.number().min(18, 'Must be 18 or older').max(120, 'Invalid age'),
  role: z.enum(['admin', 'user', 'guest'])
});

type UserInput = z.infer<typeof UserSchema>;

const validateUser = (input: unknown): UserInput => {
  try {
    return UserSchema.parse(input);
  } catch (error) {
    if (error instanceof z.ZodError) {
      throw new Error(`Validation failed: ${error.errors.map(e => e.message).join(', ')}`);
    }
    throw error;
  }
};
```

### XSS Prevention

```typescript
// Sanitize user input
import DOMPurify from 'dompurify';

const sanitizeHtml = (html: string): string => {
  return DOMPurify.sanitize(html, {
    ALLOWED_TAGS: ['b', 'i', 'em', 'strong', 'p', 'br'],
    ALLOWED_ATTR: []
  });
};

// Safe HTML rendering
const SafeHtml = ({ content }: { content: string }) => {
  const sanitizedContent = useMemo(() => sanitizeHtml(content), [content]);
  
  return (
    <div 
      dangerouslySetInnerHTML={{ __html: sanitizedContent }}
    />
  );
};
```

## Documentation Standards

### Code Comments

```typescript
/**
 * Calculates the total price including tax and discounts
 * @param items - Array of cart items
 * @param taxRate - Tax rate as decimal (e.g., 0.08 for 8%)
 * @param discountCode - Optional discount code
 * @returns Promise resolving to total price with breakdown
 * @throws Error if tax rate is invalid or discount code is expired
 * @example
 * ```typescript
 * const total = await calculateTotal(
 *   [{ price: 100, quantity: 2 }],
 *   0.08,
 *   'SAVE10'
 * );
 * console.log(total.final); // 194.4
 * ```
 */
export const calculateTotal = async (
  items: CartItem[],
  taxRate: number,
  discountCode?: string
): Promise<PriceBreakdown> => {
  // Implementation
};
```

### README Files

```markdown
# Component Name

Brief description of what this component does.

## Usage

```typescript
import { ComponentName } from './ComponentName';

<ComponentName 
  prop1="value"
  prop2={42}
  onAction={handleAction}
/>
```

## Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| prop1 | string | - | Description of prop1 |
| prop2 | number | 0 | Description of prop2 |
| onAction | function | - | Callback function |

## Examples

### Basic Usage
[Example code]

### Advanced Usage
[Example code]

## Related Components

- [RelatedComponent1](./RelatedComponent1)
- [RelatedComponent2](./RelatedComponent2)
```

## Related Documentation

- [Project Overview](../docs/index.md)
- [System Architecture](../docs/architecture/overview.md)
- [Current Status](../docs/status/progress.yaml)
- [Setup Guide](../docs/guides/setup.md)