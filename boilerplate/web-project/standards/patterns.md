# Architectural Patterns

> **AI Context**: This is the single source of truth for architectural patterns.
> For current status: see `../docs/status/progress.yaml`
> For system overview: see `../docs/architecture/overview.md`

## Pattern Overview

[Project Name] follows established architectural patterns to ensure maintainability, scalability, and consistency across the codebase.

## Component Patterns

### Container/Presenter Pattern

Separate business logic from presentation:

```typescript
// Container Component (Business Logic)
const UserListContainer: React.FC = () => {
  const [users, setUsers] = useState<User[]>([]);
  const [loading, setLoading] = useState(false);
  
  const fetchUsers = useCallback(async () => {
    setLoading(true);
    try {
      const data = await api.getUsers();
      setUsers(data);
    } catch (error) {
      console.error('Failed to fetch users:', error);
    } finally {
      setLoading(false);
    }
  }, []);
  
  useEffect(() => {
    fetchUsers();
  }, [fetchUsers]);
  
  return <UserList users={users} loading={loading} onRefresh={fetchUsers} />;
};

// Presenter Component (Presentation)
interface UserListProps {
  users: User[];
  loading: boolean;
  onRefresh: () => void;
}

const UserList: React.FC<UserListProps> = ({ users, loading, onRefresh }) => {
  if (loading) return <LoadingSpinner />;
  
  return (
    <div className="user-list">
      <button onClick={onRefresh}>Refresh</button>
      {users.map(user => (
        <UserCard key={user.id} user={user} />
      ))}
    </div>
  );
};
```

### Compound Component Pattern

Create flexible component compositions:

```typescript
interface FormProps {
  children: React.ReactNode;
  onSubmit: (data: FormData) => void;
}

const Form: React.FC<FormProps> & {
  Field: typeof FormField;
  Submit: typeof FormSubmit;
} = ({ children, onSubmit }) => {
  // Form logic
  return <form onSubmit={handleSubmit}>{children}</form>;
};

const FormField: React.FC<FieldProps> = ({ name, label, ...props }) => {
  // Field logic
  return (
    <div className="form-field">
      <label htmlFor={name}>{label}</label>
      <input id={name} name={name} {...props} />
    </div>
  );
};

const FormSubmit: React.FC<SubmitProps> = ({ children }) => {
  return <button type="submit">{children}</button>;
};

Form.Field = FormField;
Form.Submit = FormSubmit;

// Usage
<Form onSubmit={handleSubmit}>
  <Form.Field name="email" label="Email" type="email" required />
  <Form.Field name="password" label="Password" type="password" required />
  <Form.Submit>Submit</Form.Submit>
</Form>
```

## State Management Patterns

### Context Pattern

Manage global state with React Context:

```typescript
// Context Definition
interface AppContextType {
  user: User | null;
  setUser: (user: User | null) => void;
  theme: Theme;
  toggleTheme: () => void;
}

const AppContext = createContext<AppContextType | undefined>(undefined);

// Context Provider
export const AppProvider: React.FC<{ children: React.ReactNode }> = ({ children }) => {
  const [user, setUser] = useState<User | null>(null);
  const [theme, setTheme] = useState<Theme>('light');
  
  const toggleTheme = useCallback(() => {
    setTheme(prev => prev === 'light' ? 'dark' : 'light');
  }, []);
  
  const value = {
    user,
    setUser,
    theme,
    toggleTheme
  };
  
  return <AppContext.Provider value={value}>{children}</AppContext.Provider>;
};

// Custom Hook
export const useApp = (): AppContextType => {
  const context = useContext(AppContext);
  if (context === undefined) {
    throw new Error('useApp must be used within an AppProvider');
  }
  return context;
};
```

### Reducer Pattern

Manage complex state logic:

```typescript
interface TodoState {
  todos: Todo[];
  filter: 'all' | 'active' | 'completed';
  loading: boolean;
}

type TodoAction =
  | { type: 'ADD_TODO'; payload: Todo }
  | { type: 'TOGGLE_TODO'; payload: string }
  | { type: 'SET_FILTER'; payload: 'all' | 'active' | 'completed' }
  | { type: 'SET_LOADING'; payload: boolean };

const todoReducer = (state: TodoState, action: TodoAction): TodoState => {
  switch (action.type) {
    case 'ADD_TODO':
      return { ...state, todos: [...state.todos, action.payload] };
    case 'TOGGLE_TODO':
      return {
        ...state,
        todos: state.todos.map(todo =>
          todo.id === action.payload
            ? { ...todo, completed: !todo.completed }
            : todo
        )
      };
    case 'SET_FILTER':
      return { ...state, filter: action.payload };
    case 'SET_LOADING':
      return { ...state, loading: action.payload };
    default:
      return state;
  }
};
```

## Data Fetching Patterns

### Custom Hook Pattern

Encapsulate data fetching logic:

```typescript
interface UseApiOptions<T> {
  onSuccess?: (data: T) => void;
  onError?: (error: Error) => void;
  enabled?: boolean;
}

const useApi = <T>(
  url: string,
  options: UseApiOptions<T> = {}
): {
  data: T | null;
  loading: boolean;
  error: Error | null;
  refetch: () => void;
} => {
  const [data, setData] = useState<T | null>(null);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState<Error | null>(null);
  
  const fetchData = useCallback(async () => {
    setLoading(true);
    setError(null);
    
    try {
      const response = await fetch(url);
      if (!response.ok) {
        throw new Error(`HTTP error! status: ${response.status}`);
      }
      const result = await response.json();
      setData(result);
      options.onSuccess?.(result);
    } catch (err) {
      const error = err instanceof Error ? err : new Error('Unknown error');
      setError(error);
      options.onError?.(error);
    } finally {
      setLoading(false);
    }
  }, [url, options]);
  
  useEffect(() => {
    if (options.enabled !== false) {
      fetchData();
    }
  }, [fetchData, options.enabled]);
  
  return { data, loading, error, refetch: fetchData };
};
```

### Query Pattern

Organize API queries:

```typescript
// API Query Functions
export const userQueries = {
  getUsers: () => ({
    queryKey: ['users'],
    queryFn: () => api.getUsers()
  }),
  
  getUser: (id: string) => ({
    queryKey: ['users', id],
    queryFn: () => api.getUser(id)
  }),
  
  createUser: (userData: CreateUserData) => ({
    mutationFn: (data: CreateUserData) => api.createUser(data),
    onSuccess: () => {
      // Invalidate and refetch users
      queryClient.invalidateQueries({ queryKey: ['users'] });
    }
  })
};

// Usage in Components
const UserList = () => {
  const { data: users, isLoading } = useQuery(userQueries.getUsers());
  const createUserMutation = useMutation(userQueries.createUser());
  
  const handleCreateUser = (userData: CreateUserData) => {
    createUserMutation.mutate(userData);
  };
  
  // Component logic
};
```

## Error Handling Patterns

### Error Boundary Pattern

Catch and handle React errors:

```typescript
interface ErrorBoundaryState {
  hasError: boolean;
  error?: Error;
}

class ErrorBoundary extends React.Component<
  React.PropsWithChildren<{}>,
  ErrorBoundaryState
> {
  constructor(props: React.PropsWithChildren<{}>) {
    super(props);
    this.state = { hasError: false };
  }
  
  static getDerivedStateFromError(error: Error): ErrorBoundaryState {
    return { hasError: true, error };
  }
  
  componentDidCatch(error: Error, errorInfo: React.ErrorInfo) {
    console.error('Error caught by boundary:', error, errorInfo);
    // Log to error reporting service
  }
  
  render() {
    if (this.state.hasError) {
      return (
        <div className="error-boundary">
          <h2>Something went wrong</h2>
          <button onClick={() => this.setState({ hasError: false })}>
            Try again
          </button>
        </div>
      );
    }
    
    return this.props.children;
  }
}
```

### Result Pattern

Handle success and error states consistently:

```typescript
type Result<T, E = Error> = 
  | { success: true; data: T }
  | { success: false; error: E };

const safeApiCall = async <T>(fn: () => Promise<T>): Promise<Result<T>> => {
  try {
    const data = await fn();
    return { success: true, data };
  } catch (error) {
    return { success: false, error: error instanceof Error ? error : new Error('Unknown error') };
  }
};

// Usage
const result = await safeApiCall(() => api.getUsers());
if (result.success) {
  setUsers(result.data);
} else {
  setError(result.error.message);
}
```

## Performance Patterns

### Memoization Pattern

Optimize expensive calculations:

```typescript
const ExpensiveComponent: React.FC<{ items: Item[]; filter: string }> = ({ items, filter }) => {
  const filteredItems = useMemo(() => {
    return items.filter(item => 
      item.name.toLowerCase().includes(filter.toLowerCase())
    );
  }, [items, filter]);
  
  const sortedItems = useMemo(() => {
    return [...filteredItems].sort((a, b) => a.name.localeCompare(b.name));
  }, [filteredItems]);
  
  return (
    <div>
      {sortedItems.map(item => (
        <ItemCard key={item.id} item={item} />
      ))}
    </div>
  );
};
```

### Lazy Loading Pattern

Load components on demand:

```typescript
const LazyComponent = lazy(() => import('./HeavyComponent'));

const App = () => {
  const [showHeavy, setShowHeavy] = useState(false);
  
  return (
    <div>
      <button onClick={() => setShowHeavy(true)}>
        Load Heavy Component
      </button>
      
      {showHeavy && (
        <Suspense fallback={<LoadingSpinner />}>
          <LazyComponent />
        </Suspense>
      )}
    </div>
  );
};
```

## Related Documentation

- [Project Overview](../docs/index.md)
- [System Architecture](../docs/architecture/overview.md)
- [Current Status](../docs/status/progress.yaml)
- [Coding Standards](coding.md)