# Coding Standards - Radios Blog

> **AI Context**: This is the single source of truth for code conventions.
> For current status: see `../docs/status/progress.yaml`
> For project overview: see `../docs/index.md`

## Code Style & Conventions

### General Principles

- **Readability**: Code should be self-documenting
- **Consistency**: Follow established patterns
- **Maintainability**: Write code for future developers
- **Performance**: Optimize for user experience

### Naming Conventions

#### Files & Directories
- **Components**: PascalCase (`RadioCard.tsx`)
- **Hooks**: camelCase (`useRadioData.ts`)
- **Utilities**: camelCase (`formatPrice.ts`)
- **Constants**: UPPER_SNAKE_CASE (`API_ENDPOINTS`)
- **Types**: PascalCase (`RadioSpecs.ts`)

#### Variables & Functions
- **Variables**: camelCase (`radioName`, `isLoading`)
- **Functions**: camelCase (`getRadioData`, `handleSubmit`)
- **Boolean**: Prefix with `is`, `has`, `can` (`isAvailable`, `hasImages`)
- **Constants**: UPPER_SNAKE_CASE (`MAX_RADIO_PRICE`)

## TypeScript Standards

### Type Definitions

```typescript
// Prefer interfaces for object shapes
interface VintageRadio {
  id: string;
  name: string;
  brand: string;
  model: string;
  yearManufactured: number;
  condition: RadioCondition;
  price: number;
  currency: string;
  specifications: RadioSpecs;
}

// Use enums for fixed sets of values
enum RadioCondition {
  MINT = 'MINT',
  EXCELLENT = 'EXCELLENT',
  GOOD = 'GOOD',
  FAIR = 'FAIR',
  POOR = 'POOR'
}

// Use type aliases for unions and complex types
type ApiResponse<T> = {
  data: T;
  error?: string;
  status: 'success' | 'error';
};
```

### Strict Mode

- Enable strict TypeScript configuration
- Use explicit return types for public functions
- Avoid `any` type - use `unknown` or proper types
- Leverage type inference where appropriate

## React & Next.js Standards

### Component Structure

```typescript
// Functional components with proper typing
interface RadioCardProps {
  radio: VintageRadio;
  onViewDetails: (id: string) => void;
  onAddToFavorites: (id: string) => void;
  disabled?: boolean;
}

export const RadioCard: React.FC<RadioCardProps> = ({
  radio,
  onViewDetails,
  onAddToFavorites,
  disabled = false
}) => {
  // Hooks at the top
  const [isFavorite, setIsFavorite] = useState<boolean>(false);
  
  // Event handlers
  const handleViewDetails = useCallback(() => {
    onViewDetails(radio.id);
  }, [radio.id, onViewDetails]);
  
  // Render
  return (
    <div className="radio-card">
      <h3>{radio.name}</h3>
      <p>{radio.brand} {radio.model}</p>
      <button onClick={handleViewDetails} disabled={disabled}>
        View Details
      </button>
    </div>
  );
};
```

### Hooks Usage

- Use custom hooks for reusable logic
- Follow hooks naming convention (`use[Feature]`)
- Memoize expensive calculations with `useMemo`
- Optimize re-renders with `useCallback`

### State Management

- Use React Context for global state
- Prefer local state over global state
- Use appropriate state management patterns
- Implement proper error boundaries

## API & Data Handling

### API Calls

```typescript
// Use typed API functions
const fetchRadios = async (filters: RadioFilters): Promise<VintageRadio[]> => {
  const response = await fetch('/api/radios?' + new URLSearchParams(filters));
  
  if (!response.ok) {
    throw new Error(`Failed to fetch radios: ${response.statusText}`);
  }
  
  return response.json();
};

// Handle errors gracefully
try {
  const radios = await fetchRadios(filters);
  setRadios(radios);
} catch (error) {
  console.error('Failed to fetch radios:', error);
  setError(error.message);
}
```

### Data Validation

- Validate API responses
- Use schema validation libraries (Zod)
- Handle edge cases gracefully
- Implement proper error handling

## Styling Standards

### CSS & TailwindCSS

- Use TailwindCSS utility classes
- Create custom components for repeated patterns
- Follow mobile-first responsive design
- Maintain consistent spacing and typography

### Component Styling

```typescript
// Use consistent class organization
const cardClasses = [
  'p-6 rounded-lg border',
  'bg-white shadow-sm',
  'hover:shadow-md transition-shadow',
  'focus:ring-2 focus:ring-blue-500',
  'disabled:opacity-50 disabled:cursor-not-allowed'
].join(' ');
```

## Testing Standards

### Test Structure

```typescript
describe('RadioCard', () => {
  it('should render correctly', () => {
    render(<RadioCard radio={mockRadio} onViewDetails={jest.fn()} />);
    expect(screen.getByText(mockRadio.name)).toBeInTheDocument();
  });
  
  it('should call onViewDetails when clicked', () => {
    const mockOnViewDetails = jest.fn();
    render(<RadioCard radio={mockRadio} onViewDetails={mockOnViewDetails} />);
    
    fireEvent.click(screen.getByRole('button'));
    expect(mockOnViewDetails).toHaveBeenCalledWith(mockRadio.id);
  });
});
```

### Testing Principles

- Test behavior, not implementation
- Use meaningful test descriptions
- Mock external dependencies
- Maintain high test coverage

## Performance Standards

### Optimization Techniques

- Use React.memo for expensive components
- Implement proper loading states
- Optimize bundle size
- Use image optimization
- Implement proper caching strategies

### Monitoring

- Track Core Web Vitals
- Monitor bundle size
- Measure performance metrics
- Use performance profiling tools

## Security Standards

### Input Validation

- Validate all user inputs
- Sanitize data before rendering
- Use proper authentication
- Implement proper authorization

### Data Protection

- Use HTTPS in production
- Implement proper CORS policies
- Secure sensitive environment variables
- Follow OWASP security guidelines

## Documentation Standards

### Code Comments

```typescript
/**
 * Fetches vintage radio data from the API
 * @param filters - Filter criteria for radio search
 * @returns Promise resolving to array of vintage radios
 * @throws Error if the API request fails
 */
const fetchVintageRadios = async (filters: RadioFilters): Promise<VintageRadio[]> => {
  // Implementation
};
```

### README Files

- Document setup instructions
- Explain project structure
- Provide usage examples
- Include troubleshooting guides

## Related Documentation

- [Project Overview](../docs/index.md)
- [System Architecture](../docs/architecture/overview.md)
- [Current Status](../docs/status/progress.yaml)
- [Setup Guide](../docs/guides/setup.md)