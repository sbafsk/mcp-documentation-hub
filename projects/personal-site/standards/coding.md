# Coding Standards

> **AI Context**: This is the single source of truth for code conventions.
> For current status: see `../docs/status/progress.yaml`
> For project overview: see `../docs/index.md`

## Code Style & Conventions

### General Principles

- **Readability**: Code should be self-documenting and easy to understand
- **Consistency**: Follow established patterns throughout the codebase
- **Maintainability**: Write code for future developers and easy updates
- **Performance**: Optimize for user experience and Core Web Vitals
- **Accessibility**: Ensure all components meet WCAG guidelines

### Naming Conventions

#### Files & Directories
- **Components**: PascalCase (`HeroSection.tsx`)
- **Hooks**: camelCase (`useContactForm.ts`)
- **Utilities**: camelCase (`formatDate.ts`)
- **Constants**: UPPER_SNAKE_CASE (`API_ENDPOINTS`)
- **Types**: PascalCase (`ContactFormData.ts`)
- **Sections**: PascalCase (`AboutSection.tsx`, `ExperienceSection.tsx`)

#### Variables & Functions
- **Variables**: camelCase (`userName`, `isLoading`, `contactData`)
- **Functions**: camelCase (`handleSubmit`, `sendEmail`, `validateForm`)
- **Boolean**: Prefix with `is`, `has`, `can` (`isVisible`, `hasError`, `canSubmit`)
- **Constants**: UPPER_SNAKE_CASE (`MAX_MESSAGE_LENGTH`)

## TypeScript Standards

### Type Definitions

```typescript
// Prefer interfaces for object shapes
interface ContactFormData {
  name: string
  email: string
  subject: string
  message: string
}

// Use enums for fixed sets of values
enum SkillLevel {
  BEGINNER = 'beginner',
  INTERMEDIATE = 'intermediate',
  ADVANCED = 'advanced',
  EXPERT = 'expert'
}

// Use type aliases for unions and complex types
type ApiResponse<T> = {
  data: T
  error?: string
  status: 'success' | 'error'
}

// Use discriminated unions for different states
type ContactFormState = 
  | { status: 'idle' }
  | { status: 'submitting' }
  | { status: 'success'; message: string }
  | { status: 'error'; error: string }
```

### Strict Mode

- Enable strict TypeScript configuration
- Use explicit return types for public functions
- Avoid `any` type - use `unknown` or proper types
- Leverage type inference where appropriate
- Use proper typing for form data and API responses

## React & Next.js Standards

### Component Structure

```typescript
// Functional components with proper typing
interface HeroSectionProps {
  title: string
  subtitle: string
  onDownloadCV: () => void
  onContact: () => void
}

export const HeroSection: React.FC<HeroSectionProps> = ({
  title,
  subtitle,
  onDownloadCV,
  onContact
}) => {
  // Hooks at the top
  const [isVisible, setIsVisible] = useState(false)
  
  // Event handlers
  const handleDownloadCV = useCallback(() => {
    onDownloadCV()
  }, [onDownloadCV])
  
  const handleContact = useCallback(() => {
    onContact()
  }, [onContact])
  
  // Effects
  useEffect(() => {
    setIsVisible(true)
  }, [])
  
  // Render
  return (
    <section className="hero-section">
      <Card className="hero-card bg-gradient-to-br from-blue-50 to-indigo-100">
        <CardContent className="text-center py-20">
          <h1 className="text-5xl font-bold text-gray-900 mb-4">
            {title}
          </h1>
          <p className="text-xl text-gray-600 mb-8">{subtitle}</p>
          <div className="flex gap-4 justify-center">
            <Button size="lg" onClick={handleDownloadCV}>
              Download CV
            </Button>
            <Button variant="outline" size="lg" onClick={handleContact}>
              Contact Me
            </Button>
          </div>
        </CardContent>
      </Card>
    </section>
  )
}
```

### Hooks Usage

- Use custom hooks for reusable logic
- Follow hooks naming convention (`use[Feature]`)
- Memoize expensive calculations with `useMemo`
- Optimize re-renders with `useCallback`
- Use proper dependency arrays

```typescript
// Custom hook example
const useContactForm = () => {
  const [formData, setFormData] = useState<ContactFormData>({
    name: '',
    email: '',
    subject: '',
    message: ''
  })
  
  const [formState, setFormState] = useState<ContactFormState>({ status: 'idle' })
  
  const handleSubmit = useCallback(async (data: ContactFormData) => {
    setFormState({ status: 'submitting' })
    
    try {
      const response = await fetch('/api/contact', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(data)
      })
      
      if (!response.ok) {
        throw new Error('Failed to send message')
      }
      
      setFormState({ status: 'success', message: 'Message sent successfully!' })
      setFormData({ name: '', email: '', subject: '', message: '' })
    } catch (error) {
      setFormState({ 
        status: 'error', 
        error: error instanceof Error ? error.message : 'Unknown error' 
      })
    }
  }, [])
  
  return {
    formData,
    formState,
    handleSubmit,
    setFormData
  }
}
```

### State Management

- Use React Context for global state (theme, user preferences)
- Prefer local state over global state
- Use appropriate state management patterns
- Implement proper error boundaries
- Use form libraries (React Hook Form) for complex forms

## shadcn/ui Component Standards

### Component Usage

- Always import components from `@/components/ui`
- Use consistent prop patterns
- Leverage built-in variants and sizes
- Customize through className props when needed
- Follow accessibility guidelines

```typescript
// Good: Using shadcn/ui components consistently
import { Card, CardContent, CardHeader, CardTitle } from "@/components/ui/card"
import { Button } from "@/components/ui/button"
import { Badge } from "@/components/ui/badge"

// Use consistent sizing and variants
<Card className="overflow-hidden">
  <CardHeader>
    <CardTitle className="text-lg">Project Title</CardTitle>
  </CardHeader>
  <CardContent>
    <div className="flex gap-2 mb-4">
      <Badge variant="outline">React</Badge>
      <Badge variant="outline">TypeScript</Badge>
    </div>
    <Button size="sm" variant="outline">View Demo</Button>
  </CardContent>
</Card>
```

### Customization

- Extend shadcn/ui components through composition
- Use CSS variables for consistent theming
- Maintain design system consistency
- Avoid overriding core component behavior

## Form Handling Standards

### React Hook Form + Zod

```typescript
import { useForm } from "react-hook-form"
import { zodResolver } from "@hookform/resolvers/zod"
import { z } from "zod"

const contactSchema = z.object({
  name: z.string().min(2, 'Name must be at least 2 characters'),
  email: z.string().email('Invalid email address'),
  subject: z.string().min(5, 'Subject must be at least 5 characters'),
  message: z.string().min(10, 'Message must be at least 10 characters'),
})

type ContactFormData = z.infer<typeof contactSchema>

const ContactForm = () => {
  const {
    register,
    handleSubmit,
    formState: { errors, isSubmitting },
    reset
  } = useForm<ContactFormData>({
    resolver: zodResolver(contactSchema)
  })
  
  const onSubmit = async (data: ContactFormData) => {
    try {
      const response = await fetch('/api/contact', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(data)
      })
      
      if (response.ok) {
        reset()
        // Show success message
      }
    } catch (error) {
      // Handle error
    }
  }
  
  return (
    <form onSubmit={handleSubmit(onSubmit)} className="space-y-4">
      <div>
        <Label htmlFor="name">Name</Label>
        <Input
          id="name"
          {...register('name')}
          className={errors.name ? 'border-red-500' : ''}
        />
        {errors.name && (
          <p className="text-sm text-red-500 mt-1">{errors.name.message}</p>
        )}
      </div>
      
      {/* Repeat for other fields */}
      
      <Button type="submit" disabled={isSubmitting} className="w-full">
        {isSubmitting ? 'Sending...' : 'Send Message'}
      </Button>
    </form>
  )
}
```

## Styling Standards

### TailwindCSS Usage

- Use utility classes consistently
- Create custom components for repeated patterns
- Follow mobile-first responsive design
- Maintain consistent spacing and typography
- Use CSS variables for custom values

```typescript
// Consistent spacing and typography
const sectionClasses = "py-16 px-4 md:px-8 lg:px-16"
const containerClasses = "max-w-7xl mx-auto"
const headingClasses = "text-3xl md:text-4xl lg:text-5xl font-bold text-gray-900 mb-8"

// Component-specific classes
const heroCardClasses = [
  "bg-gradient-to-br from-blue-50 to-indigo-100",
  "rounded-xl shadow-lg",
  "border border-blue-100"
].join(" ")

const buttonClasses = [
  "px-6 py-3 rounded-lg",
  "font-semibold transition-all duration-200",
  "hover:scale-105 active:scale-95"
].join(" ")
```

### Responsive Design

- Start with mobile layout
- Use `md:` and `lg:` prefixes for larger screens
- Ensure touch-friendly button sizes (min 44px)
- Optimize spacing for mobile viewing
- Test on various screen sizes

## Performance Standards

### Optimization Techniques

- Use React.memo for expensive components
- Implement proper loading states
- Optimize bundle size
- Use Next.js Image component for images
- Implement proper caching strategies
- Lazy load non-critical components

```typescript
// Lazy loading example
const LazyProjectsSection = lazy(() => import('./ProjectsSection'))

const HomePage = () => {
  return (
    <div>
      <HeroSection />
      <AboutSection />
      <Suspense fallback={<ProjectsSkeleton />}>
        <LazyProjectsSection />
      </Suspense>
    </div>
  )
}

// Memoization example
const SkillCard = React.memo<SkillCardProps>(({ skill }) => {
  return (
    <Card>
      <CardContent>
        <div className="flex justify-between items-center mb-2">
          <span className="text-sm font-medium">{skill.name}</span>
          <Badge variant="secondary">{skill.level}</Badge>
        </div>
        <Progress value={skill.proficiency} className="h-2" />
      </CardContent>
    </Card>
  )
})
```

## Accessibility Standards

### ARIA and Semantic HTML

- Use semantic HTML elements (`<section>`, `<article>`, `<nav>`)
- Add descriptive labels for interactive elements
- Ensure proper heading hierarchy
- Provide alt text for images
- Use proper form labels and descriptions

```typescript
// Accessible form example
<div>
  <Label htmlFor="email" className="block text-sm font-medium mb-2">
    Email Address
  </Label>
  <Input
    id="email"
    type="email"
    aria-describedby="email-help"
    placeholder="your@email.com"
    required
  />
  <p id="email-help" className="text-sm text-gray-500 mt-1">
    We'll use this to get back to you.
  </p>
</div>

// Accessible navigation
<nav aria-label="Main navigation">
  <ul className="flex space-x-6">
    <li><a href="#about" className="hover:text-blue-600">About</a></li>
    <li><a href="#experience" className="hover:text-blue-600">Experience</a></li>
    <li><a href="#projects" className="hover:text-blue-600">Projects</a></li>
    <li><a href="#contact" className="hover:text-blue-600">Contact</a></li>
  </ul>
</nav>
```

### Keyboard Navigation

- All interactive elements must be keyboard accessible
- Visible focus indicators
- Logical tab order
- Skip links for main content
- Proper focus management in modals

## Testing Standards

### Test Structure

```typescript
import { render, screen, fireEvent } from '@testing-library/react'
import { ContactForm } from './ContactForm'

describe('ContactForm', () => {
  it('should render all form fields', () => {
    render(<ContactForm />)
    
    expect(screen.getByLabelText(/name/i)).toBeInTheDocument()
    expect(screen.getByLabelText(/email/i)).toBeInTheDocument()
    expect(screen.getByLabelText(/subject/i)).toBeInTheDocument()
    expect(screen.getByLabelText(/message/i)).toBeInTheDocument()
  })
  
  it('should show validation errors for invalid input', async () => {
    render(<ContactForm />)
    
    const submitButton = screen.getByRole('button', { name: /send message/i })
    fireEvent.click(submitButton)
    
    expect(await screen.findByText(/name must be at least 2 characters/i)).toBeInTheDocument()
  })
  
  it('should submit form with valid data', async () => {
    const mockSubmit = jest.fn()
    render(<ContactForm onSubmit={mockSubmit} />)
    
    // Fill form with valid data
    fireEvent.change(screen.getByLabelText(/name/i), { target: { value: 'John Doe' } })
    fireEvent.change(screen.getByLabelText(/email/i), { target: { value: 'john@example.com' } })
    fireEvent.change(screen.getByLabelText(/subject/i), { target: { value: 'Job Opportunity' } })
    fireEvent.change(screen.getByLabelText(/message/i), { target: { value: 'I would like to discuss...' } })
    
    fireEvent.click(screen.getByRole('button', { name: /send message/i }))
    
    expect(mockSubmit).toHaveBeenCalledWith({
      name: 'John Doe',
      email: 'john@example.com',
      subject: 'Job Opportunity',
      message: 'I would like to discuss...'
    })
  })
})
```

### Testing Principles

- Test behavior, not implementation
- Use meaningful test descriptions
- Mock external dependencies (API calls)
- Maintain high test coverage
- Test accessibility features
- Test responsive behavior

## Security Standards

### Input Validation

- Validate all user inputs with Zod schemas
- Sanitize data before rendering
- Use proper authentication if needed
- Implement proper authorization
- Prevent XSS attacks

### Data Protection

- Use HTTPS in production
- Implement proper CORS policies
- Secure sensitive environment variables
- Follow OWASP security guidelines
- Validate email inputs and sanitize content

## Documentation Standards

### Code Comments

```typescript
/**
 * Contact form component with validation and submission handling
 * 
 * @param onSubmit - Callback function called when form is successfully submitted
 * @param initialData - Optional initial form data for editing existing contacts
 * @returns Rendered contact form component
 */
export const ContactForm: React.FC<ContactFormProps> = ({
  onSubmit,
  initialData
}) => {
  // Implementation
}
```

### README Files

- Document setup instructions
- Explain project structure
- Provide usage examples
- Include troubleshooting guides
- Document environment variables

## Related Documentation

- [Project Overview](../docs/index.md)
- [System Architecture](../docs/architecture/overview.md)
- [Design System](../docs/design-system/components.md)
- [Current Status](../docs/status/progress.yaml)
- [Setup Guide](../docs/guides/setup.md)

