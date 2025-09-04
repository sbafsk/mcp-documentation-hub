# Architectural Patterns - Radios Blog

> **AI Context**: This is the single source of truth for architectural patterns.
> For current status: see `../docs/status/progress.yaml`
> For system overview: see `../docs/architecture/overview.md`

## Pattern Overview

Radios Blog follows established architectural patterns to ensure maintainability, scalability, and consistency across the codebase, with specific focus on content management and vintage radio data handling.

## Component Patterns

### Container/Presenter Pattern

Separate business logic from presentation:

```typescript
// Container Component (Business Logic)
const RadioListContainer: React.FC = () => {
  const [radios, setRadios] = useState<VintageRadio[]>([]);
  const [loading, setLoading] = useState(false);
  const [filters, setFilters] = useState<RadioFilters>({});
  
  const fetchRadios = useCallback(async () => {
    setLoading(true);
    try {
      const data = await api.getRadios(filters);
      setRadios(data);
    } catch (error) {
      console.error('Failed to fetch radios:', error);
    } finally {
      setLoading(false);
    }
  }, [filters]);
  
  useEffect(() => {
    fetchRadios();
  }, [fetchRadios]);
  
  return <RadioList radios={radios} loading={loading} onRefresh={fetchRadios} />;
};

// Presenter Component (Presentation)
interface RadioListProps {
  radios: VintageRadio[];
  loading: boolean;
  onRefresh: () => void;
}

const RadioList: React.FC<RadioListProps> = ({ radios, loading, onRefresh }) => {
  if (loading) return <LoadingSpinner />;
  
  return (
    <div className="radio-list">
      <button onClick={onRefresh}>Refresh</button>
      {radios.map(radio => (
        <RadioCard key={radio.id} radio={radio} />
      ))}
    </div>
  );
};
```

### Compound Component Pattern

Create flexible component compositions for complex UI elements:

```typescript
interface RadioGalleryProps {
  children: React.ReactNode;
  onRadioSelect: (radio: VintageRadio) => void;
}

const RadioGallery: React.FC<RadioGalleryProps> & {
  Header: typeof GalleryHeader;
  Grid: typeof GalleryGrid;
  Footer: typeof GalleryFooter;
} = ({ children, onRadioSelect }) => {
  // Gallery logic
  return <div className="radio-gallery">{children}</div>;
};

const GalleryHeader: React.FC<HeaderProps> = ({ title, filters }) => {
  return (
    <div className="gallery-header">
      <h2>{title}</h2>
      <FilterControls filters={filters} />
    </div>
  );
};

const GalleryGrid: React.FC<GridProps> = ({ radios }) => {
  return (
    <div className="gallery-grid">
      {radios.map(radio => (
        <RadioCard key={radio.id} radio={radio} />
      ))}
    </div>
  );
};

const GalleryFooter: React.FC<FooterProps> = ({ pagination }) => {
  return (
    <div className="gallery-footer">
      <Pagination {...pagination} />
    </div>
  );
};

RadioGallery.Header = GalleryHeader;
RadioGallery.Grid = GalleryGrid;
RadioGallery.Footer = GalleryFooter;

// Usage
<RadioGallery onRadioSelect={handleRadioSelect}>
  <RadioGallery.Header title="Vintage Radios" filters={filters} />
  <RadioGallery.Grid radios={radios} />
  <RadioGallery.Footer pagination={pagination} />
</RadioGallery>
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
  favorites: string[];
  addToFavorites: (radioId: string) => void;
  removeFromFavorites: (radioId: string) => void;
}

const AppContext = createContext<AppContextType | undefined>(undefined);

// Context Provider
export const AppProvider: React.FC<{ children: React.ReactNode }> = ({ children }) => {
  const [user, setUser] = useState<User | null>(null);
  const [theme, setTheme] = useState<Theme>('light');
  const [favorites, setFavorites] = useState<string[]>([]);
  
  const addToFavorites = useCallback((radioId: string) => {
    setFavorites(prev => [...prev, radioId]);
  }, []);
  
  const removeFromFavorites = useCallback((radioId: string) => {
    setFavorites(prev => prev.filter(id => id !== radioId));
  }, []);
  
  const toggleTheme = useCallback(() => {
    setTheme(prev => prev === 'light' ? 'dark' : 'light');
  }, []);
  
  const value = {
    user,
    setUser,
    theme,
    toggleTheme,
    favorites,
    addToFavorites,
    removeFromFavorites
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

Manage complex state logic for radio collections:

```typescript
interface RadioCollectionState {
  radios: VintageRadio[];
  filters: RadioFilters;
  loading: boolean;
  error: string | null;
}

type RadioCollectionAction =
  | { type: 'SET_RADIOS'; payload: VintageRadio[] }
  | { type: 'SET_FILTERS'; payload: RadioFilters }
  | { type: 'SET_LOADING'; payload: boolean }
  | { type: 'SET_ERROR'; payload: string | null }
  | { type: 'ADD_RADIO'; payload: VintageRadio }
  | { type: 'UPDATE_RADIO'; payload: { id: string; updates: Partial<VintageRadio> } };

const radioCollectionReducer = (state: RadioCollectionState, action: RadioCollectionAction): RadioCollectionState => {
  switch (action.type) {
    case 'SET_RADIOS':
      return { ...state, radios: action.payload, loading: false, error: null };
    case 'SET_FILTERS':
      return { ...state, filters: action.payload, loading: true };
    case 'SET_LOADING':
      return { ...state, loading: action.payload };
    case 'SET_ERROR':
      return { ...state, error: action.payload, loading: false };
    case 'ADD_RADIO':
      return { ...state, radios: [...state.radios, action.payload] };
    case 'UPDATE_RADIO':
      return {
        ...state,
        radios: state.radios.map(radio =>
          radio.id === action.payload.id
            ? { ...radio, ...action.payload.updates }
            : radio
        )
      };
    default:
      return state;
  }
};
```

## Data Fetching Patterns

### Custom Hook Pattern

Encapsulate data fetching logic for radios and blog posts:

```typescript
interface UseRadiosOptions {
  filters?: RadioFilters;
  onSuccess?: (data: VintageRadio[]) => void;
  onError?: (error: Error) => void;
  enabled?: boolean;
}

const useRadios = (options: UseRadiosOptions = {}) => {
  const [radios, setRadios] = useState<VintageRadio[]>([]);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState<Error | null>(null);
  
  const fetchRadios = useCallback(async () => {
    setLoading(true);
    setError(null);
    
    try {
      const result = await api.getRadios(options.filters || {});
      setRadios(result);
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
      fetchRadios();
    }
  }, [fetchRadios, options.enabled]);
  
  return { radios, loading, error, refetch: fetchRadios };
};

// Usage
const RadioList = () => {
  const { radios, loading, error, refetch } = useRadios({
    filters: { brand: 'Philips', condition: 'EXCELLENT' },
    onSuccess: (data) => console.log(`Loaded ${data.length} radios`),
    onError: (error) => console.error('Failed to load radios:', error)
  });
  
  if (loading) return <LoadingSpinner />;
  if (error) return <ErrorMessage error={error} />;
  
  return (
    <div>
      {radios.map(radio => (
        <RadioCard key={radio.id} radio={radio} />
      ))}
    </div>
  );
};
```

### React Query Pattern

Organize API queries for better caching and state management:

```typescript
// API Query Functions
export const radioQueries = {
  getRadios: (filters: RadioFilters) => ({
    queryKey: ['radios', filters],
    queryFn: () => api.getRadios(filters)
  }),
  
  getRadio: (id: string) => ({
    queryKey: ['radios', id],
    queryFn: () => api.getRadio(id)
  }),
  
  createRadio: (radioData: CreateRadioData) => ({
    mutationFn: (data: CreateRadioData) => api.createRadio(data),
    onSuccess: () => {
      // Invalidate and refetch radios
      queryClient.invalidateQueries({ queryKey: ['radios'] });
    }
  })
};

// Usage in Components
const RadioList = () => {
  const { data: radios, isLoading } = useQuery(radioQueries.getRadios(filters));
  const createRadioMutation = useMutation(radioQueries.createRadio());
  
  const handleCreateRadio = (radioData: CreateRadioData) => {
    createRadioMutation.mutate(radioData);
  };
  
  // Component logic
};
```

## Error Handling Patterns

### Error Boundary Pattern

Catch and handle React errors gracefully:

```typescript
interface ErrorBoundaryState {
  hasError: boolean;
  error?: Error;
}

class RadioErrorBoundary extends React.Component<
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
          <h2>Something went wrong with the radio display</h2>
          <p>We're having trouble loading the vintage radio collection.</p>
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
const result = await safeApiCall(() => api.getRadios(filters));
if (result.success) {
  setRadios(result.data);
} else {
  setError(result.error.message);
}
```

## Performance Patterns

### Memoization Pattern

Optimize expensive calculations for radio filtering and sorting:

```typescript
const RadioGallery: React.FC<{ radios: VintageRadio[]; filters: RadioFilters }> = ({ radios, filters }) => {
  const filteredRadios = useMemo(() => {
    return radios.filter(radio => {
      if (filters.brand && radio.brand !== filters.brand) return false;
      if (filters.yearMin && radio.yearManufactured < filters.yearMin) return false;
      if (filters.yearMax && radio.yearManufactured > filters.yearMax) return false;
      if (filters.condition && radio.condition !== filters.condition) return false;
      if (filters.priceMin && radio.price < filters.priceMin) return false;
      if (filters.priceMax && radio.price > filters.priceMax) return false;
      return true;
    });
  }, [radios, filters]);
  
  const sortedRadios = useMemo(() => {
    return [...filteredRadios].sort((a, b) => {
      switch (filters.sortBy) {
        case 'price_asc': return a.price - b.price;
        case 'price_desc': return b.price - a.price;
        case 'year_asc': return a.yearManufactured - b.yearManufactured;
        case 'year_desc': return b.yearManufactured - a.yearManufactured;
        case 'name': return a.name.localeCompare(b.name);
        default: return 0;
      }
    });
  }, [filteredRadios, filters.sortBy]);
  
  return (
    <div className="radio-gallery">
      {sortedRadios.map(radio => (
        <RadioCard key={radio.id} radio={radio} />
      ))}
    </div>
  );
};
```

### Lazy Loading Pattern

Load components on demand for better performance:

```typescript
const LazyRadioDetails = lazy(() => import('./RadioDetails'));
const LazyBlogPost = lazy(() => import('./BlogPost'));

const App = () => {
  const [showRadioDetails, setShowRadioDetails] = useState(false);
  const [showBlogPost, setShowBlogPost] = useState(false);
  
  return (
    <div>
      <button onClick={() => setShowRadioDetails(true)}>
        View Radio Details
      </button>
      
      {showRadioDetails && (
        <Suspense fallback={<LoadingSpinner />}>
          <LazyRadioDetails />
        </Suspense>
      )}
      
      <button onClick={() => setShowBlogPost(true)}>
        Read Blog Post
      </button>
      
      {showBlogPost && (
        <Suspense fallback={<LoadingSpinner />}>
          <LazyBlogPost />
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