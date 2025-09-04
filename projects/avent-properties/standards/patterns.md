# Architectural Patterns - Avent Properties

> **AI Context**: This is the single source of truth for architectural patterns.
> For current status: see `../docs/status/progress.yaml`
> For system overview: see `../docs/architecture/overview.md`

## Pattern Overview

Avent Properties follows established architectural patterns to ensure maintainability, scalability, and consistency across the codebase.

## Component Patterns

### Container/Presenter Pattern

Separate business logic from presentation:

```typescript
// Container Component (Business Logic)
const PropertyListContainer: React.FC = () => {
  const [properties, setProperties] = useState<Property[]>([]);
  const [loading, setLoading] = useState(false);
  
  const fetchProperties = useCallback(async () => {
    setLoading(true);
    try {
      const data = await api.getProperties();
      setProperties(data);
    } catch (error) {
      console.error('Failed to fetch properties:', error);
    } finally {
      setLoading(false);
    }
  }, []);
  
  useEffect(() => {
    fetchProperties();
  }, [fetchProperties]);
  
  return <PropertyList properties={properties} loading={loading} onRefresh={fetchProperties} />;
};

// Presenter Component (Presentation)
interface PropertyListProps {
  properties: Property[];
  loading: boolean;
  onRefresh: () => void;
}

const PropertyList: React.FC<PropertyListProps> = ({ properties, loading, onRefresh }) => {
  if (loading) return <LoadingSpinner />;
  
  return (
    <div className="property-list">
      <button onClick={onRefresh}>Refresh</button>
      {properties.map(property => (
        <PropertyCard key={property.id} property={property} />
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
interface PropertyState {
  properties: Property[];
  filter: 'all' | 'available' | 'reserved';
  loading: boolean;
}

type PropertyAction =
  | { type: 'ADD_PROPERTY'; payload: Property }
  | { type: 'SET_FILTER'; payload: 'all' | 'available' | 'reserved' }
  | { type: 'SET_LOADING'; payload: boolean };

const propertyReducer = (state: PropertyState, action: PropertyAction): PropertyState => {
  switch (action.type) {
    case 'ADD_PROPERTY':
      return { ...state, properties: [...state.properties, action.payload] };
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

const useProperties = (options: UseApiOptions<Property[]> = {}) => {
  const [properties, setProperties] = useState<Property[]>([]);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState<Error | null>(null);
  
  const fetchProperties = useCallback(async () => {
    setLoading(true);
    setError(null);
    
    try {
      const result = await api.getProperties();
      setProperties(result);
      options.onSuccess?.(result);
    } catch (err) {
      const error = err instanceof Error ? err : new Error('Unknown error');
      setError(error);
      options.onError?.(error);
    } finally {
      setLoading(false);
    }
  }, [options]);
  
  useEffect(() => {
    if (options.enabled !== false) {
      fetchProperties();
    }
  }, [fetchProperties, options.enabled]);
  
  return { properties, loading, error, refetch: fetchProperties };
};
```

### React Query Pattern

Organize API queries:

```typescript
// API Query Functions
export const propertyQueries = {
  getProperties: () => ({
    queryKey: ['properties'],
    queryFn: () => api.getProperties()
  }),
  
  getProperty: (id: string) => ({
    queryKey: ['properties', id],
    queryFn: () => api.getProperty(id)
  }),
  
  createProperty: (propertyData: CreatePropertyData) => ({
    mutationFn: (data: CreatePropertyData) => api.createProperty(data),
    onSuccess: () => {
      // Invalidate and refetch properties
      queryClient.invalidateQueries({ queryKey: ['properties'] });
    }
  })
};

// Usage in Components
const PropertyList = () => {
  const { data: properties, isLoading } = useQuery(propertyQueries.getProperties());
  const createPropertyMutation = useMutation(propertyQueries.createProperty());
  
  const handleCreateProperty = (propertyData: CreatePropertyData) => {
    createPropertyMutation.mutate(propertyData);
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
const result = await safeApiCall(() => api.getProperties());
if (result.success) {
  setProperties(result.data);
} else {
  setError(result.error.message);
}
```

## Performance Patterns

### Memoization Pattern

Optimize expensive calculations:

```typescript
const ExpensiveComponent: React.FC<{ properties: Property[]; filter: string }> = ({ properties, filter }) => {
  const filteredProperties = useMemo(() => {
    return properties.filter(property => 
      property.title.toLowerCase().includes(filter.toLowerCase())
    );
  }, [properties, filter]);
  
  const sortedProperties = useMemo(() => {
    return [...filteredProperties].sort((a, b) => a.title.localeCompare(b.title));
  }, [filteredProperties]);
  
  return (
    <div>
      {sortedProperties.map(property => (
        <PropertyCard key={property.id} property={property} />
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