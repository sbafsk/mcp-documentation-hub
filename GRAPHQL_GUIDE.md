# GraphQL Implementation Guide

> **AI Context**: This is the single source of truth for GraphQL implementation patterns and best practices.
> For MCP resources: see `MCP_RESOURCES.md`
> For project setup: see `SETUP.md`

## Overview

This guide provides comprehensive patterns for implementing GraphQL with Apollo Server and Supabase SDK, focusing on maintainable, performant, and type-safe solutions.

## Core Architecture Principles

### 1. Single GraphQL Endpoint
- One Apollo Server instance handling all GraphQL operations
- Supabase SDK for direct database operations
- Custom resolvers for business logic
- No GraphQL-to-GraphQL overhead

### 2. Type Safety First
- Use Supabase generated TypeScript types
- Strongly typed GraphQL schema
- Consistent error handling patterns
- Runtime type validation

### 3. Performance Optimization
- Direct database queries (no GraphQL-to-GraphQL overhead)
- Efficient query patterns with joins
- Proper pagination and filtering
- Connection pooling and caching

## Project Setup

### Dependencies
```json
{
  "dependencies": {
    "@apollo/server": "^4.9.5",
    "graphql": "^16.8.1",
    "@supabase/supabase-js": "^2.38.4",
    "graphql-scalars": "^1.22.4"
  },
  "devDependencies": {
    "supabase": "^1.123.4",
    "@types/node": "^20.8.0",
    "typescript": "^5.2.2"
  }
}
```

### Type Generation
```bash
# Generate TypeScript types from Supabase schema
npx supabase gen types typescript --project-id YOUR_PROJECT_ID > lib/database.types.ts
```

## Schema Definition

### Core Types
```typescript
import { gql } from 'apollo-server-express'

export const typeDefs = gql`
  scalar DateTime
  scalar JSON
  scalar UUID

  type Property {
    id: UUID!
    title: String!
    description: String
    price: Float!
    currency: String!
    city: String!
    neighborhood: String
    property_type: String!
    bedrooms: Int!
    bathrooms: Int!
    area_m2: Float!
    amenities: [String!]!
    images: [String!]!
    status: PropertyStatus!
    agency_id: UUID!
    created_at: DateTime!
    updated_at: DateTime!
    
    # Relations
    agency: Agency
    reservations: [TourReservation!]!
  }

  type Agency {
    id: UUID!
    name: String!
    email: String!
    phone: String
    address: String
    created_at: DateTime!
    updated_at: DateTime!
    
    # Relations
    properties: [Property!]!
  }

  type User {
    id: UUID!
    email: String!
    name: String
    role: UserRole!
    created_at: DateTime!
    updated_at: DateTime!
    
    # Relations
    reservations: [TourReservation!]!
  }

  type TourReservation {
    id: UUID!
    scheduled_date: DateTime!
    deposit_amount: Float!
    status: ReservationStatus!
    user_id: UUID!
    property_id: UUID!
    created_at: DateTime!
    updated_at: DateTime!
    
    # Relations
    user: User!
    property: Property!
  }

  enum PropertyStatus {
    AVAILABLE
    RESERVED
    SOLD
  }

  enum UserRole {
    USER
    AGENT
    ADMIN
  }

  enum ReservationStatus {
    PENDING
    CONFIRMED
    CANCELLED
  }
`
```

### Input Types
```typescript
export const inputTypes = gql`
  input PropertyFilters {
    city: String
    property_type: String
    min_price: Float
    max_price: Float
    bedrooms: Int
    status: PropertyStatus
  }

  input PaginationInput {
    limit: Int = 50
    offset: Int = 0
  }

  input CreateReservationInput {
    property_id: UUID!
    scheduled_date: DateTime!
    deposit_amount: Float!
  }

  input UpdateReservationInput {
    id: UUID!
    scheduled_date: DateTime
    deposit_amount: Float
    status: ReservationStatus
  }
`
```

## Context Setup

### Apollo Context
```typescript
import { SupabaseClient } from '@supabase/supabase-js'
import { createClient } from '@supabase/supabase-js'
import { Database } from './database.types'

export interface Context {
  supabase: SupabaseClient<Database>
  user?: {
    id: string
    role: string
    email: string
  }
}

export async function createContext({ req }: { req: any }): Promise<Context> {
  const authHeader = req.headers.authorization
  const token = authHeader?.replace('Bearer ', '')

  // Create Supabase client
  const supabase = createClient<Database>(
    process.env.SUPABASE_URL!,
    process.env.SUPABASE_ANON_KEY!,
    {
      auth: {
        autoRefreshToken: false,
        persistSession: false
      },
      global: {
        headers: token ? { Authorization: `Bearer ${token}` } : {}
      }
    }
  )

  // Get user from token if provided
  let user = undefined
  if (token) {
    const { data: { user: authUser }, error } = await supabase.auth.getUser(token)
    if (!error && authUser) {
      user = {
        id: authUser.id,
        role: authUser.user_metadata?.role || 'user',
        email: authUser.email!
      }
    }
  }

  return { supabase, user }
}
```

## Resolver Implementation

### Query Resolvers
```typescript
import { GraphQLError } from 'graphql'
import { Context } from '../lib/context'

export const queryResolvers = {
  Query: {
    properties: async (
      _: any,
      { filters = {}, pagination = {} }: {
        filters?: PropertyFilters
        pagination?: PaginationInput
      },
      { supabase }: Context
    ) => {
      let query = supabase
        .from('properties')
        .select(`
          *,
          agency:agencies(*)
        `)

      // Apply filters
      if (filters.city) query = query.eq('city', filters.city)
      if (filters.property_type) query = query.eq('property_type', filters.property_type)
      if (filters.min_price) query = query.gte('price', filters.min_price)
      if (filters.max_price) query = query.lte('price', filters.max_price)
      if (filters.bedrooms) query = query.eq('bedrooms', filters.bedrooms)
      if (filters.status) query = query.eq('status', filters.status)

      // Apply pagination
      const limit = Math.min(pagination.limit || 50, 100) // Cap at 100
      const offset = pagination.offset || 0
      query = query.range(offset, offset + limit - 1)

      const { data, error } = await query

      if (error) {
        throw new GraphQLError(`Failed to fetch properties: ${error.message}`)
      }

      return data || []
    },

    property: async (
      _: any,
      { id }: { id: string },
      { supabase }: Context
    ) => {
      const { data, error } = await supabase
        .from('properties')
        .select(`
          *,
          agency:agencies(*),
          reservations:tour_reservations(
            *,
            user:users(id, name, email)
          )
        `)
        .eq('id', id)
        .single()

      if (error && error.code !== 'PGRST116') { // Not found is ok
        throw new GraphQLError(`Failed to fetch property: ${error.message}`)
      }

      return data
    }
  }
}
```

### Mutation Resolvers
```typescript
export const mutationResolvers = {
  Mutation: {
    createReservation: async (
      _: any,
      { input }: { input: CreateReservationInput },
      { supabase, user }: Context
    ) => {
      if (!user) {
        throw new GraphQLError('Authentication required')
      }

      // Business logic validation
      const scheduledDate = new Date(input.scheduled_date)
      const now = new Date()
      
      if (scheduledDate <= now) {
        throw new GraphQLError('Scheduled date must be in the future')
      }

      // Check if property exists and is available
      const { data: property, error: propertyError } = await supabase
        .from('properties')
        .select('id, status, title')
        .eq('id', input.property_id)
        .single()

      if (propertyError) {
        throw new GraphQLError('Property not found')
      }

      if (property.status !== 'available') {
        throw new GraphQLError('Property is not available for reservation')
      }

      // Check for existing reservations on the same date
      const { data: existingReservation } = await supabase
        .from('tour_reservations')
        .select('id')
        .eq('property_id', input.property_id)
        .eq('scheduled_date', input.scheduled_date)
        .eq('status', 'confirmed')
        .single()

      if (existingReservation) {
        throw new GraphQLError('This time slot is already reserved')
      }

      // Create reservation
      const { data, error } = await supabase
        .from('tour_reservations')
        .insert({
          user_id: user.id,
          property_id: input.property_id,
          scheduled_date: input.scheduled_date,
          deposit_amount: input.deposit_amount,
          status: 'pending'
        })
        .select(`
          *,
          user:users(id, name, email),
          property:properties(id, title, city, price)
        `)
        .single()

      if (error) {
        throw new GraphQLError(`Failed to create reservation: ${error.message}`)
      }

      return data
    }
  }
}
```

## Server Configuration

### Apollo Server Setup
```typescript
import { ApolloServer } from '@apollo/server'
import { startStandaloneServer } from '@apollo/server/standalone'
import { typeDefs } from './schema/typeDefs'
import { queryResolvers } from './resolvers/queries'
import { mutationResolvers } from './resolvers/mutations'
import { createContext } from './lib/context'

const resolvers = {
  ...queryResolvers,
  ...mutationResolvers
}

const server = new ApolloServer({
  typeDefs,
  resolvers,
  formatError: (error) => {
    console.error('GraphQL Error:', error)
    
    // Don't expose internal errors in production
    if (process.env.NODE_ENV === 'production') {
      if (error.message.startsWith('Failed to')) {
        return new Error('Internal server error')
      }
    }
    
    return error
  }
})

async function startServer() {
  const { url } = await startStandaloneServer(server, {
    context: createContext,
    listen: { port: parseInt(process.env.PORT || '4000') }
  })

  console.log(`🚀 Server ready at ${url}`)
}

startServer()
```

## Best Practices

### Error Handling
```typescript
// Always handle Supabase errors consistently
const { data, error } = await supabase.from('table').select('*')

if (error) {
  console.error('Database error:', error)
  throw new GraphQLError(`Operation failed: ${error.message}`)
}

return data || []
```

### Authentication Guards
```typescript
// Create reusable auth guards
export const requireAuth = (user?: Context['user']) => {
  if (!user) {
    throw new GraphQLError('Authentication required')
  }
  return user
}

export const requireRole = (user: Context['user'], allowedRoles: string[]) => {
  if (!user || !allowedRoles.includes(user.role)) {
    throw new GraphQLError('Insufficient permissions')
  }
}
```

### Query Optimization
```typescript
// Use specific selects to avoid over-fetching
const { data } = await supabase
  .from('properties')
  .select(`
    id, title, price,
    agency:agencies(name, phone)
  `)
  
// Use indexes for common filters
// Add this to your Supabase SQL editor:
// CREATE INDEX idx_properties_city ON properties(city);
// CREATE INDEX idx_properties_status ON properties(status);
```

### Input Validation
```typescript
// Validate inputs before database operations
const validateReservationInput = (input: CreateReservationInput) => {
  if (!input.property_id || !input.scheduled_date || !input.deposit_amount) {
    throw new GraphQLError('Missing required fields')
  }
  
  if (input.deposit_amount < 0) {
    throw new GraphQLError('Deposit amount must be positive')
  }
  
  const date = new Date(input.scheduled_date)
  if (isNaN(date.getTime())) {
    throw new GraphQLError('Invalid date format')
  }
}
```

## Testing Strategy

### Unit Tests
```typescript
// resolvers/queries.test.ts
import { queryResolvers } from './queries'
import { createMockContext } from '../test/helpers'

describe('Property Resolvers', () => {
  it('should fetch properties with filters', async () => {
    const mockContext = createMockContext()
    const result = await queryResolvers.Query.properties(
      {},
      { filters: { city: 'Miami' } },
      mockContext
    )
    
    expect(result).toBeDefined()
    expect(mockContext.supabase.from).toHaveBeenCalledWith('properties')
  })
})
```

## Performance Monitoring

### Key Metrics
- Query response times
- Database connection usage
- Memory consumption
- Error rates

### Optimization Tips
- Use connection pooling
- Implement query result caching
- Add database indexes for frequent filters
- Use pagination for large result sets

## Migration from Hybrid Resolver

### Step 1: Update Dependencies
```bash
npm uninstall [hybrid-resolver-packages]
npm install @supabase/supabase-js
```

### Step 2: Replace Resolvers
```typescript
// Before (hybrid approach)
const result = await hybridResolver.routeQuery('properties', args, context, customResolver)

// After (direct approach)
const { data, error } = await supabase
  .from('properties')
  .select('*')
  .eq('city', args.city)
```

### Step 3: Update Context
Replace GraphQL client initialization with Supabase client in context.

### Step 4: Test Thoroughly
- Run integration tests
- Verify all queries return expected data
- Check performance improvements

## Related Resources

- [MCP Resources](MCP_RESOURCES.md)
- [Documentation Architecture](DOCUMENTATION_ARCHITECTURE.md)
- [Project Setup Guide](SETUP.md)
- [Apollo Server Documentation](https://www.apollographql.com/docs/apollo-server/)
- [Supabase Documentation](https://supabase.com/docs)

---

**This guide provides a complete, production-ready approach to GraphQL implementation with Apollo Server and Supabase SDK.**
