# API Specifications - Radios Blog

> **AI Context**: This is the single source of truth for API design.
> For current status: see `../status/progress.yaml`
> For system overview: see `overview.md`

## API Overview

Radios Blog exposes a REST API that provides secure access to vintage radio collections, blog content, and postcard galleries. The API is designed for content management and user interaction.

## API Architecture

### REST API Structure

```
/api/
├── /auth/                      # Authentication endpoints
├── /radios/                    # Vintage radio endpoints
├── /blog/                      # Blog post endpoints
├── /postcards/                 # Postcard endpoints
├── /collections/               # User collection endpoints
├── /users/                     # User management endpoints
├── /search/                    # Search and filtering endpoints
└── /health                     # Health check endpoint
```

## Core Endpoints

### Authentication

#### POST /api/auth/login
**Purpose**: User authentication via Supabase
**Request Body**:
```json
{
  "email": "user@example.com",
  "password": "secure_password"
}
```

**Response**:
```json
{
  "token": "jwt_token_here",
  "user": {
    "id": "user_id",
    "email": "user@example.com",
    "name": "User Name",
    "role": "USER"
  }
}
```

#### POST /api/auth/register
**Purpose**: User registration
**Request Body**:
```json
{
  "email": "user@example.com",
  "password": "secure_password",
  "name": "User Name"
}
```

**Response**:
```json
{
  "message": "User registered successfully",
  "user": {
    "id": "user_id",
    "email": "user@example.com",
    "name": "User Name"
  }
}
```

### Vintage Radios

#### GET /api/radios
**Purpose**: Retrieve radio listings with filtering
**Query Parameters**:
- `page`: Page number (default: 1)
- `limit`: Items per page (default: 20)
- `brand`: Filter by brand
- `year_min`: Minimum year
- `year_max`: Maximum year
- `condition`: Filter by condition
- `price_min`: Minimum price
- `price_max`: Maximum price
- `status`: Filter by status

**Response**:
```json
{
  "data": [
    {
      "id": "radio_id",
      "name": "Philips 22RB470",
      "brand": "Philips",
      "model": "22RB470",
      "year_manufactured": 1955,
      "condition": "EXCELLENT",
      "price": 450.00,
      "currency": "USD",
      "images": ["image1.jpg", "image2.jpg"],
      "seller": {
        "id": "seller_id",
        "name": "Seller Name"
      }
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 20,
    "total": 150,
    "pages": 8
  }
}
```

#### GET /api/radios/{id}
**Purpose**: Retrieve specific radio details
**Response**:
```json
{
  "id": "radio_id",
  "name": "Philips 22RB470",
  "brand": "Philips",
  "model": "22RB470",
  "year_manufactured": 1955,
  "condition": "EXCELLENT",
  "price": 450.00,
  "currency": "USD",
  "description": "Beautiful vintage Philips radio...",
  "specifications": {
    "power": "110V AC",
    "frequency": "AM/FM",
    "tubes": "5 tubes"
  },
  "dimensions": {
    "width": 45,
    "height": 25,
    "depth": 30
  },
  "images": ["image1.jpg", "image2.jpg"],
  "seller": {
    "id": "seller_id",
    "name": "Seller Name",
    "rating": 4.8
  }
}
```

### Blog Posts

#### GET /api/blog
**Purpose**: Retrieve blog posts with pagination
**Query Parameters**:
- `page`: Page number (default: 1)
- `limit`: Items per page (default: 10)
- `category`: Filter by category
- `tag`: Filter by tag
- `author`: Filter by author ID

**Response**:
```json
{
  "data": [
    {
      "id": "post_id",
      "title": "History of Radio Broadcasting",
      "slug": "history-radio-broadcasting",
      "excerpt": "The fascinating story of how radio...",
      "author": {
        "id": "author_id",
        "name": "Author Name"
      },
      "published_at": "2025-01-15T10:00:00Z",
      "read_time_minutes": 8,
      "tags": ["history", "broadcasting", "technology"],
      "featured_image": "featured.jpg"
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 10,
    "total": 45,
    "pages": 5
  }
}
```

#### GET /api/blog/{slug}
**Purpose**: Retrieve specific blog post
**Response**:
```json
{
  "id": "post_id",
  "title": "History of Radio Broadcasting",
  "slug": "history-radio-broadcasting",
  "content": "Full markdown content here...",
  "author": {
    "id": "author_id",
    "name": "Author Name",
    "avatar_url": "avatar.jpg"
  },
  "published_at": "2025-01-15T10:00:00Z",
  "read_time_minutes": 8,
  "tags": ["history", "broadcasting", "technology"],
  "category": "History",
  "featured_image": "featured.jpg"
}
```

### Vintage Postcards

#### GET /api/postcards
**Purpose**: Retrieve postcard gallery with filtering
**Query Parameters**:
- `page`: Page number (default: 1)
- `limit`: Items per page (default: 20)
- `category`: Filter by category
- `year_min`: Minimum year
- `year_max`: Maximum year
- `artist`: Filter by artist
- `location`: Filter by location

**Response**:
```json
{
  "data": [
    {
      "id": "postcard_id",
      "title": "Radio Station WABC",
      "year_created": 1930,
      "artist": "Unknown",
      "location": "New York, NY",
      "category": "RADIO_STATION",
      "condition": "GOOD",
      "rarity": "RARE",
      "estimated_value": 150.00,
      "images": ["front.jpg", "back.jpg"],
      "tags": ["radio", "station", "art deco"]
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 20,
    "total": 300,
    "pages": 15
  }
}
```

## Error Handling

### Standard Error Response
```json
{
  "error": {
    "code": "ERROR_CODE",
    "message": "Human readable error message",
    "details": "Additional error details if available"
  }
}
```

### Common Error Codes
- `400`: Bad Request - Invalid input data
- `401`: Unauthorized - Authentication required
- `403`: Forbidden - Insufficient permissions
- `404`: Not Found - Resource doesn't exist
- `422`: Validation Error - Data validation failed
- `500`: Internal Server Error - Server-side error

## Rate Limiting

- **Requests per minute**: 1000
- **Burst limit**: 100
- **Headers**: Rate limit information included in response headers
- **Exemptions**: Health check endpoints

## API Versioning

- **Current Version**: v1
- **Versioning Strategy**: URL path (/api/v1/radios)
- **Deprecation Policy**: 6-month notice for breaking changes
- **Backward Compatibility**: Maintained for 12 months

## Security

### Authentication
- **Method**: JWT tokens via Supabase Auth
- **Token Expiry**: 24 hours (regular), 7 days (remember me)
- **Refresh**: Automatic token refresh

### Authorization
- **Role-Based Access**: ADMIN, MODERATOR, USER, GUEST roles
- **Row Level Security**: Database-level access control
- **API Permissions**: Endpoint-level permission checks

### Data Protection
- **HTTPS**: All endpoints require HTTPS
- **Input Validation**: Zod schema validation
- **SQL Injection**: Parameterized queries only
- **XSS Prevention**: Input sanitization and output encoding

## Performance

### Query Optimization
- **N+1 Prevention**: Efficient joins and batching
- **Caching**: React Query client-side caching
- **Pagination**: Cursor-based pagination for large datasets

### Monitoring
- **Response Times**: Tracked via Vercel Analytics
- **Error Rates**: Monitored via Sentry
- **Usage Metrics**: API usage and performance tracking

## Related Documentation

- [System Architecture](overview.md)
- [Database Schema](database.md)
- [Current Status](../status/progress.yaml)
- [Setup Guide](../guides/setup.md)