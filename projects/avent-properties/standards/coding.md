# Coding Standards - Avent Properties

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
- **Components**: PascalCase (`UserProfile.tsx`)
- **Hooks**: camelCase (`useUserData.ts`)
- **Utilities**: camelCase (`formatDate.ts`)
- **Constants**: UPPER_SNAKE_CASE (`API_ENDPOINTS`)
- **Types**: PascalCase (`UserProfile.ts`)

#### Variables & Functions
- **Variables**: camelCase (`userName`, `isLoading`)
- **Functions**: camelCase (`getUserData`, `handleSubmit`)
- **Boolean**: Prefix with `is`, `has`, `can` (`isVisible`, `hasPermission`)
- **Constants**: UPPER_SNAKE_CASE (`MAX_RETRY_ATTEMPTS`)

## TypeScript Standards

### Type Definitions

```typescript
// Prefer interfaces for object shapes
interface UserProfile {
  id: string;
  email: string;
  name: string;
  role: UserRole;
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
interface ComponentProps {
  title: string;
  onAction: () => void;
  disabled?: boolean;
}

export const Component: React.FC<ComponentProps> = ({
  title,
  onAction,
  disabled = false
}) => {
  // Hooks at the top
  const [state, setState] = useState<string>('');
  
  // Event handlers
  const handleClick = useCallback(() => {
    if (!disabled) {
      onAction();
    }
  }, [onAction, disabled]);
  
  // Render
  return (
    <div className="component">
      <h2>{title}</h2>
      <button onClick={handleClick} disabled={disabled}>
        Action
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

## GraphQL & API Standards

### GraphQL Operations

```typescript
// Use typed GraphQL functions
const GET_PROPERTIES = gql`
  query GetProperties($filter: propertiesFilter) {
    propertiesCollection(filter: $filter) {
      edges {
        node {
          id
          title
          price
          city
        }
      }
    }
  }
`;

// Handle errors gracefully
try {
  const { data } = await client.query({
    query: GET_PROPERTIES,
    variables: { filter: { city: { eq: 'Punta del Este' } } }
  });
  setProperties(data.propertiesCollection.edges);
} catch (error) {
  console.error('Failed to fetch properties:', error);
  setError(error.message);
}
```

### API Error Handling

- Validate API responses
- Use schema validation libraries
- Handle edge cases gracefully
- Implement proper error boundaries

## Styling Standards

### CSS & TailwindCSS

- Use TailwindCSS utility classes
- Create custom components for repeated patterns
- Follow mobile-first responsive design
- Maintain consistent spacing and typography

### Component Styling

```typescript
// Use consistent class organization
const buttonClasses = [
  'px-4 py-2 rounded-md',
  'bg-blue-600 text-white',
  'hover:bg-blue-700',
  'disabled:opacity-50 disabled:cursor-not-allowed',
  'transition-colors duration-200'
].join(' ');
```

## Testing Standards

### Test Structure

```typescript
describe('Component', () => {
  it('should render correctly', () => {
    render(<Component title="Test" onAction={jest.fn()} />);
    expect(screen.getByText('Test')).toBeInTheDocument();
  });
  
  it('should call onAction when clicked', () => {
    const mockAction = jest.fn();
    render(<Component title="Test" onAction={mockAction} />);
    
    fireEvent.click(screen.getByRole('button'));
    expect(mockAction).toHaveBeenCalledTimes(1);
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
 * Fetches user profile data from the API
 * @param userId - The unique identifier of the user
 * @returns Promise resolving to user profile data
 * @throws Error if the API request fails
 */
const fetchUserProfile = async (userId: string): Promise<UserProfile> => {
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