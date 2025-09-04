# Coding Standards - Maicemita Site

> **AI Context**: This is the single source of truth for code conventions.
> For current status: see `../docs/status/progress.yaml`
> For project overview: see `../docs/index.md`

## Code Style & Conventions

### General Principles

- **Readability**: Code should be self-documenting
- **Consistency**: Follow established patterns
- **Maintainability**: Write code for future developers
- **Performance**: Optimize for user experience
- **Security**: Implement secure coding practices

### Naming Conventions

#### Files & Directories
- **Components**: PascalCase (`ProductGallery.tsx`)
- **Hooks**: camelCase (`useProductData.ts`)
- **Utilities**: camelCase (`formatPrice.ts`)
- **Constants**: UPPER_SNAKE_CASE (`PRODUCT_TYPES`)
- **Types**: PascalCase (`Product.ts`)
- **Pages**: kebab-case (`product-gallery.tsx`)
- **Styles**: kebab-case (`product-gallery.module.css`)

#### Variables & Functions
- **Variables**: camelCase (`productName`, `isLoading`)
- **Functions**: camelCase (`getProductData`, `handleSubmit`)
- **Boolean**: Prefix with `is`, `has`, `can` (`isVisible`, `hasStock`)
- **Constants**: UPPER_SNAKE_CASE (`MAX_QUANTITY`)
- **Classes**: PascalCase (`ProductService`)
- **Interfaces**: PascalCase with `I` prefix (`IProduct`)

## TypeScript Standards

### Type Definitions

```typescript
// Product types
interface Product {
  id: string;
  name: string;
  description: string;
  price: number;
  currency: string;
  images: string[];
  flavors: Flavor[];
  boxSizes: BoxSize[];
  inStock: boolean;
  createdAt: Date;
  updatedAt: Date;
}

interface Flavor {
  id: string;
  name: string;
  description: string;
  available: boolean;
}

interface BoxSize {
  id: string;
  name: string;
  quantity: number;
  price: number;
  available: boolean;
}

// Order types
interface Order {
  id: string;
  customerName: string;
  customerEmail: string;
  customerPhone: string;
  products: OrderItem[];
  totalAmount: number;
  status: OrderStatus;
  createdAt: Date;
  updatedAt: Date;
}

interface OrderItem {
  productId: string;
  flavorId: string;
  boxSizeId: string;
  quantity: number;
  price: number;
}

enum OrderStatus {
  PENDING = 'pending',
  CONFIRMED = 'confirmed',
  PREPARING = 'preparing',
  READY = 'ready',
  DELIVERED = 'delivered',
  CANCELLED = 'cancelled'
}

// API response types
type ApiResponse<T> = {
  data: T;
  error?: string;
  status: 'success' | 'error';
  timestamp: Date;
};
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

## React & Next.js Standards

### Component Structure

```typescript
// Product component example
interface ProductCardProps {
  product: Product;
  onSelect: (product: Product) => void;
  disabled?: boolean;
  className?: string;
}

export const ProductCard: React.FC<ProductCardProps> = ({
  product,
  onSelect,
  disabled = false,
  className
}) => {
  // Hooks at the top
  const [isLoading, setIsLoading] = useState(false);
  const [selectedFlavor, setSelectedFlavor] = useState<Flavor | null>(null);
  
  // Custom hooks
  const { data: productDetails } = useProductDetails(product.id);
  
  // Event handlers with useCallback
  const handleSelect = useCallback(() => {
    if (!disabled && !isLoading && selectedFlavor) {
      onSelect({ ...product, selectedFlavor });
    }
  }, [onSelect, disabled, isLoading, selectedFlavor, product]);
  
  // Memoized values
  const availableBoxSizes = useMemo(() => {
    return product.boxSizes.filter(size => size.available);
  }, [product.boxSizes]);
  
  // Early returns for loading/error states
  if (isLoading) {
    return <ProductCardSkeleton />;
  }
  
  if (!product.inStock) {
    return <ProductCardOutOfStock product={product} />;
  }
  
  // Main render
  return (
    <div className={cn("product-card", className)}>
      <ProductImage 
        src={product.images[0]} 
        alt={product.name}
        className="product-card__image"
      />
      <div className="product-card__content">
        <h3 className="product-card__title">{product.name}</h3>
        <p className="product-card__description">{product.description}</p>
        <FlavorSelector 
          flavors={product.flavors}
          selected={selectedFlavor}
          onSelect={setSelectedFlavor}
        />
        <BoxSizeSelector 
          sizes={availableBoxSizes}
          onSelect={handleSelect}
          disabled={!selectedFlavor}
        />
      </div>
    </div>
  );
};

// Export with display name for debugging
ProductCard.displayName = 'ProductCard';
```

### Hooks Usage

```typescript
// Custom hook for product data
export const useProductData = (productId: string) => {
  const [product, setProduct] = useState<Product | null>(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);
  
  useEffect(() => {
    const fetchProduct = async () => {
      try {
        setLoading(true);
        const response = await fetch(`/api/products/${productId}`);
        const data = await response.json();
        
        if (!response.ok) {
          throw new Error(data.error || 'Failed to fetch product');
        }
        
        setProduct(data.data);
      } catch (err) {
        setError(err instanceof Error ? err.message : 'Unknown error');
      } finally {
        setLoading(false);
      }
    };
    
    fetchProduct();
  }, [productId]);
  
  return { product, loading, error };
};

// Hook for form handling
export const useOrderForm = () => {
  const [formData, setFormData] = useState<OrderFormData>({
    customerName: '',
    customerEmail: '',
    customerPhone: '',
    products: []
  });
  
  const [errors, setErrors] = useState<Record<string, string>>({});
  const [isSubmitting, setIsSubmitting] = useState(false);
  
  const validateForm = useCallback(() => {
    const newErrors: Record<string, string> = {};
    
    if (!formData.customerName.trim()) {
      newErrors.customerName = 'Name is required';
    }
    
    if (!formData.customerEmail.trim()) {
      newErrors.customerEmail = 'Email is required';
    } else if (!isValidEmail(formData.customerEmail)) {
      newErrors.customerEmail = 'Invalid email format';
    }
    
    if (!formData.customerPhone.trim()) {
      newErrors.customerPhone = 'Phone is required';
    }
    
    if (formData.products.length === 0) {
      newErrors.products = 'Please select at least one product';
    }
    
    setErrors(newErrors);
    return Object.keys(newErrors).length === 0;
  }, [formData]);
  
  const submitOrder = useCallback(async () => {
    if (!validateForm()) return;
    
    setIsSubmitting(true);
    try {
      const response = await fetch('/api/orders', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(formData)
      });
      
      const result = await response.json();
      
      if (!response.ok) {
        throw new Error(result.error || 'Failed to submit order');
      }
      
      return result.data;
    } finally {
      setIsSubmitting(false);
    }
  }, [formData, validateForm]);
  
  return {
    formData,
    setFormData,
    errors,
    isSubmitting,
    submitOrder,
    validateForm
  };
};
```

## API & Data Fetching Standards

### API Routes

```typescript
// API route example: /api/products/[id].ts
import { NextApiRequest, NextApiResponse } from 'next';
import { z } from 'zod';

const ProductSchema = z.object({
  id: z.string(),
  name: z.string(),
  description: z.string(),
  price: z.number().positive(),
  currency: z.string(),
  images: z.array(z.string()),
  flavors: z.array(z.object({
    id: z.string(),
    name: z.string(),
    description: z.string(),
    available: z.boolean()
  })),
  boxSizes: z.array(z.object({
    id: z.string(),
    name: z.string(),
    quantity: z.number().positive(),
    price: z.number().positive(),
    available: z.boolean()
  })),
  inStock: z.boolean()
});

export default async function handler(
  req: NextApiRequest,
  res: NextApiResponse
) {
  if (req.method !== 'GET') {
    return res.status(405).json({ error: 'Method not allowed' });
  }
  
  try {
    const { id } = req.query;
    
    if (!id || typeof id !== 'string') {
      return res.status(400).json({ error: 'Product ID is required' });
    }
    
    // Fetch product from database
    const product = await getProductById(id);
    
    if (!product) {
      return res.status(404).json({ error: 'Product not found' });
    }
    
    // Validate product data
    const validatedProduct = ProductSchema.parse(product);
    
    res.status(200).json({
      data: validatedProduct,
      status: 'success',
      timestamp: new Date()
    });
    
  } catch (error) {
    console.error('Product API error:', error);
    
    if (error instanceof z.ZodError) {
      return res.status(400).json({
        error: 'Invalid product data',
        details: error.errors
      });
    }
    
    res.status(500).json({
      error: 'Internal server error',
      status: 'error',
      timestamp: new Date()
    });
  }
}
```

### Data Fetching

```typescript
// API client with error handling
class ApiClient {
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
      'Content-Type': 'application/json'
    };
  }
}
```

## Styling Standards

### CSS & TailwindCSS

```typescript
// Component with consistent class organization
const productCardClasses = [
  // Base styles
  'bg-white rounded-lg shadow-md overflow-hidden',
  // Layout
  'flex flex-col',
  // Hover states
  'hover:shadow-lg transition-shadow duration-200',
  // Focus states
  'focus:outline-none focus:ring-2 focus:ring-blue-500',
  // Responsive
  'w-full max-w-sm mx-auto'
].join(' ');

// Component with conditional styling
interface ProductCardProps {
  variant: 'default' | 'featured' | 'out-of-stock';
  size: 'sm' | 'md' | 'lg';
  disabled?: boolean;
}

const getProductCardClasses = ({ variant, size, disabled }: ProductCardProps) => {
  const baseClasses = 'bg-white rounded-lg shadow-md overflow-hidden transition-all duration-200';
  
  const variantClasses = {
    default: 'hover:shadow-lg',
    featured: 'ring-2 ring-blue-500 hover:ring-blue-600',
    'out-of-stock': 'opacity-60 grayscale'
  };
  
  const sizeClasses = {
    sm: 'w-64 h-80',
    md: 'w-80 h-96',
    lg: 'w-96 h-[28rem]'
  };
  
  const disabledClasses = disabled 
    ? 'opacity-50 cursor-not-allowed' 
    : 'cursor-pointer';
  
  return [
    baseClasses,
    variantClasses[variant],
    sizeClasses[size],
    disabledClasses
  ].join(' ');
};
```

### Responsive Design

```typescript
// Responsive component example
export const ProductGrid: React.FC<{ products: Product[] }> = ({ products }) => {
  return (
    <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6 p-4">
      {products.map((product) => (
        <ProductCard
          key={product.id}
          product={product}
          className="w-full"
        />
      ))}
    </div>
  );
};

// Mobile-first approach
export const MobileProductCard: React.FC<{ product: Product }> = ({ product }) => {
  return (
    <div className="
      flex flex-col sm:flex-row
      bg-white rounded-lg shadow-md
      p-4 sm:p-6
      space-y-4 sm:space-y-0 sm:space-x-4
      w-full max-w-sm sm:max-w-none
    ">
      <div className="flex-shrink-0">
        <ProductImage 
          src={product.images[0]} 
          alt={product.name}
          className="w-full sm:w-32 h-48 sm:h-32 object-cover rounded-lg"
        />
      </div>
      <div className="flex-1 min-w-0">
        <h3 className="text-lg font-semibold text-gray-900 truncate">
          {product.name}
        </h3>
        <p className="text-sm text-gray-600 mt-1 line-clamp-2">
          {product.description}
        </p>
        <div className="mt-2">
          <span className="text-lg font-bold text-blue-600">
            ${product.price}
          </span>
        </div>
      </div>
    </div>
  );
};
```

## Testing Standards

### Component Testing

```typescript
// ProductCard.test.tsx
import { render, screen, fireEvent, waitFor } from '@testing-library/react';
import { ProductCard } from './ProductCard';

const mockProduct: Product = {
  id: '1',
  name: 'Alfajores de Maicena',
  description: 'Delicious homemade alfajores',
  price: 15.99,
  currency: 'USD',
  images: ['/images/alfajores.jpg'],
  flavors: [
    { id: '1', name: 'Dulce de Leche', description: 'Classic flavor', available: true }
  ],
  boxSizes: [
    { id: '1', name: 'Small', quantity: 6, price: 15.99, available: true }
  ],
  inStock: true,
  createdAt: new Date(),
  updatedAt: new Date()
};

describe('ProductCard', () => {
  it('should render product information correctly', () => {
    const mockOnSelect = jest.fn();
    render(<ProductCard product={mockProduct} onSelect={mockOnSelect} />);
    
    expect(screen.getByText('Alfajores de Maicena')).toBeInTheDocument();
    expect(screen.getByText('Delicious homemade alfajores')).toBeInTheDocument();
    expect(screen.getByText('$15.99')).toBeInTheDocument();
  });
  
  it('should call onSelect when product is selected', async () => {
    const mockOnSelect = jest.fn();
    render(<ProductCard product={mockProduct} onSelect={mockOnSelect} />);
    
    const selectButton = screen.getByRole('button', { name: /select/i });
    fireEvent.click(selectButton);
    
    await waitFor(() => {
      expect(mockOnSelect).toHaveBeenCalledWith(mockProduct);
    });
  });
  
  it('should show out of stock state when product is not available', () => {
    const outOfStockProduct = { ...mockProduct, inStock: false };
    render(<ProductCard product={outOfStockProduct} onSelect={jest.fn()} />);
    
    expect(screen.getByText(/out of stock/i)).toBeInTheDocument();
  });
});
```

### API Testing

```typescript
// API route testing
import { createMocks } from 'node-mocks-http';
import handler from '../api/products/[id]';

describe('/api/products/[id]', () => {
  it('should return product data for valid ID', async () => {
    const { req, res } = createMocks({
      method: 'GET',
      query: { id: '1' }
    });
    
    await handler(req, res);
    
    expect(res._getStatusCode()).toBe(200);
    const data = JSON.parse(res._getData());
    expect(data.status).toBe('success');
    expect(data.data).toBeDefined();
  });
  
  it('should return 404 for non-existent product', async () => {
    const { req, res } = createMocks({
      method: 'GET',
      query: { id: 'non-existent' }
    });
    
    await handler(req, res);
    
    expect(res._getStatusCode()).toBe(404);
    const data = JSON.parse(res._getData());
    expect(data.error).toBe('Product not found');
  });
});
```

## Performance Standards

### Optimization Techniques

```typescript
// React.memo for expensive components
export const ProductGallery = React.memo<{ products: Product[] }>(({ products }) => {
  const processedProducts = useMemo(() => {
    return products.map(product => ({
      ...product,
      displayPrice: formatPrice(product.price),
      availableFlavors: product.flavors.filter(flavor => flavor.available)
    }));
  }, [products]);
  
  return (
    <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
      {processedProducts.map(product => (
        <ProductCard key={product.id} product={product} />
      ))}
    </div>
  );
});

// Image optimization
export const ProductImage: React.FC<{
  src: string;
  alt: string;
  className?: string;
}> = ({ src, alt, className }) => {
  return (
    <Image
      src={src}
      alt={alt}
      width={400}
      height={300}
      className={className}
      placeholder="blur"
      blurDataURL="data:image/jpeg;base64,/9j/4AAQSkZJRgABAQAAAQABAAD/2wBDAAYEBQYFBAYGBQYHBwYIChAKCgkJChQODwwQFxQYGBcUFhYaHSUfGhsjHBYWICwgIyYnKSopGR8tMC0oMCUoKSj/2wBDAQcHBwoIChMKChMoGhYaKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCj/wAARCAAIAAoDASIAAhEBAxEB/8QAFQABAQAAAAAAAAAAAAAAAAAAAAv/xAAhEAACAQMDBQAAAAAAAAAAAAABAgMABAUGIWGRkqGx0f/EABUBAQEAAAAAAAAAAAAAAAAAAAMF/8QAGhEAAgIDAAAAAAAAAAAAAAAAAAECEgMRkf/aAAwDAQACEQMRAD8AltJagyeH0AthI5xdrLcNM91BF5pX2HaH9bcfaSXWGaRmknyJckliyjqTzSlT54b6bk+h0R//2Q=="
      priority={false}
    />
  );
};
```

## Security Standards

### Input Validation

```typescript
// Form validation with Zod
import { z } from 'zod';

const OrderFormSchema = z.object({
  customerName: z.string().min(1, 'Name is required').max(100, 'Name too long'),
  customerEmail: z.string().email('Invalid email format'),
  customerPhone: z.string().min(10, 'Phone number too short').max(15, 'Phone number too long'),
  products: z.array(z.object({
    productId: z.string(),
    flavorId: z.string(),
    boxSizeId: z.string(),
    quantity: z.number().min(1, 'Quantity must be at least 1').max(10, 'Maximum 10 items')
  })).min(1, 'Please select at least one product')
});

type OrderFormData = z.infer<typeof OrderFormSchema>;

const validateOrderForm = (data: unknown): OrderFormData => {
  try {
    return OrderFormSchema.parse(data);
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
// Safe content rendering
import DOMPurify from 'dompurify';

const sanitizeHtml = (html: string): string => {
  return DOMPurify.sanitize(html, {
    ALLOWED_TAGS: ['b', 'i', 'em', 'strong', 'p', 'br'],
    ALLOWED_ATTR: []
  });
};

// Safe HTML rendering component
const SafeHtml: React.FC<{ content: string }> = ({ content }) => {
  const sanitizedContent = useMemo(() => sanitizeHtml(content), [content]);
  
  return (
    <div 
      dangerouslySetInnerHTML={{ __html: sanitizedContent }}
    />
  );
};
```

## Related Documentation

- [Project Overview](../docs/index.md)
- [System Architecture](../docs/architecture/overview.md)
- [Current Status](../docs/status/progress.yaml)
- [Setup Guide](../docs/guides/setup.md)
