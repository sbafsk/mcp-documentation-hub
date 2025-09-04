# API Specifications - Avent Properties

> **AI Context**: This is the single source of truth for API design.
> For current status: see `../status/progress.yaml`
> For system overview: see `overview.md`

## API Overview

Avent Properties exposes a hybrid GraphQL API that provides secure access to application data and functionality through both Supabase auto-generated GraphQL and custom Apollo Server resolvers.

## API Architecture

### Hybrid GraphQL Approach

#### Supabase Auto-Generated GraphQL (72% of endpoints)
- **Purpose**: Simple CRUD operations with automatic schema generation
- **Benefits**: Zero maintenance, automatic relationships, efficient queries
- **Coverage**: Properties, agencies, users, reservations, transactions, contact requests

#### Custom Apollo Server (28% of endpoints)
- **Purpose**: Complex business logic and custom operations
- **Benefits**: Custom validation, business rules, complex workflows
- **Coverage**: Authentication, authorization, business operations

### Endpoint Structure

```
/api/
├── /graphql                    # GraphQL endpoint (Apollo Server)
├── /auth/                      # Authentication endpoints
├── /webhooks/                  # Stripe webhooks
└── /health                     # Health check endpoint
```

## Core Endpoints

### GraphQL Endpoint

#### POST /api/graphql
**Purpose**: Main GraphQL API endpoint
**Request Body**: GraphQL query/mutation
**Response**: GraphQL response with data or errors

**Example Query**:
```graphql
query GetProperties($filter: propertiesFilter, $first: Int) {
  propertiesCollection(filter: $filter, first: $first) {
    edges {
      node {
        id
        title
        price
        city
        agency {
          name
          email
        }
      }
    }
  }
}
```

**Example Mutation**:
```graphql
mutation CreateReservation($input: tour_reservationsInsertInput!) {
  insertIntoTourReservationsCollection(object: $input) {
    records {
      id
      scheduled_date
      deposit_amount
      status
    }
  }
}
```

### Authentication Endpoints

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
    "role": "CLIENT"
  }
}
```

#### POST /api/auth/logout
**Purpose**: User logout
**Headers**: `Authorization: Bearer {token}`
**Response**: `{ "message": "Logged out successfully" }`

### Webhook Endpoints

#### POST /api/webhooks/stripe
**Purpose**: Stripe webhook processing
**Headers**: `Stripe-Signature: {signature}`
**Body**: Stripe event payload
**Response**: `{ "status": "processed" }`

## GraphQL Schema

### Core Types

```graphql
type Property {
  id: ID!
  title: String!
  description: String
  price: Float!
  currency: String!
  city: String!
  neighborhood: String
  property_type: String!
  bedrooms: Int
  bathrooms: Int
  area_m2: Int
  amenities: [String!]!
  images: [String!]!
  status: PropertyStatus!
  agency: Agency!
  tour_reservations: [TourReservation!]!
  created_at: DateTime!
  updated_at: DateTime!
}

type TourReservation {
  id: ID!
  user: User!
  property: Property!
  scheduled_date: DateTime!
  deposit_amount: Float!
  status: ReservationStatus!
  transactions: [Transaction!]!
  created_at: DateTime!
  updated_at: DateTime!
}

type User {
  id: ID!
  email: String!
  name: String!
  role: UserRole!
  reservations: [TourReservation!]!
  transactions: [Transaction!]!
  created_at: DateTime!
  updated_at: DateTime!
}
```

### Enums

```graphql
enum PropertyStatus {
  AVAILABLE
  RESERVED
  SOLD
}

enum ReservationStatus {
  PENDING
  CONFIRMED
  CANCELLED
  COMPLETED
}

enum UserRole {
  ADMIN
  CLIENT
  AGENCY
}

enum TransactionType {
  DEPOSIT
  COMMISSION
  REFUND
}
```

## Error Handling

### Standard Error Response
```json
{
  "errors": [
    {
      "message": "Human readable error message",
      "extensions": {
        "code": "ERROR_CODE",
        "field": "field_name",
        "details": "Additional error details"
      }
    }
  ]
}
```

### Common Error Codes
- `AUTHENTICATION_ERROR`: User not authenticated
- `AUTHORIZATION_ERROR`: User lacks required permissions
- `VALIDATION_ERROR`: Input validation failed
- `NOT_FOUND`: Resource doesn't exist
- `INTERNAL_ERROR`: Server-side error

## Rate Limiting

- **Requests per minute**: 1000
- **Burst limit**: 100
- **Headers**: Rate limit information included in response headers
- **Exemptions**: Health check endpoints

## API Versioning

- **Current Version**: v1
- **Versioning Strategy**: URL path (/api/v1/graphql)
- **Deprecation Policy**: 6-month notice for breaking changes
- **Backward Compatibility**: Maintained for 12 months

## Security

### Authentication
- **Method**: JWT tokens via Supabase Auth
- **Token Expiry**: 24 hours (regular), 7 days (remember me)
- **Refresh**: Automatic token refresh

### Authorization
- **Role-Based Access**: ADMIN, CLIENT, AGENCY roles
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