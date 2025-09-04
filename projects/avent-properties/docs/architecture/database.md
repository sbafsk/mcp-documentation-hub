# Database Architecture - Avent Properties

> **AI Context**: This is the single source of truth for database design.
> For current status: see `../status/progress.yaml`
> For system overview: see `overview.md`

## Database Overview

Avent Properties uses PostgreSQL via Supabase as the primary data store with a well-defined schema designed for luxury real estate management.

## Database Schema

### Core Tables

#### users
```sql
CREATE TABLE users (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  email TEXT UNIQUE NOT NULL,
  name TEXT NOT NULL,
  role user_role DEFAULT 'CLIENT',
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

**Purpose**: User accounts with role-based access control
**Key Fields**: id, email, name, role, created_at, updated_at
**Relationships**: One-to-many with tour_reservations, transactions, contact_requests

#### agencies
```sql
CREATE TABLE agencies (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  name TEXT NOT NULL,
  email TEXT UNIQUE NOT NULL,
  phone TEXT,
  address TEXT,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

**Purpose**: Real estate agency information
**Key Fields**: id, name, email, phone, address
**Relationships**: One-to-many with properties

#### properties
```sql
CREATE TABLE properties (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  title TEXT NOT NULL,
  description TEXT,
  price DECIMAL(12,2) NOT NULL,
  currency TEXT DEFAULT 'USD',
  city TEXT NOT NULL,
  neighborhood TEXT,
  property_type TEXT NOT NULL,
  bedrooms INTEGER,
  bathrooms INTEGER,
  area_m2 INTEGER,
  amenities TEXT[] DEFAULT '{}',
  images TEXT[] DEFAULT '{}',
  status property_status DEFAULT 'AVAILABLE',
  agency_id UUID REFERENCES agencies(id) ON DELETE CASCADE,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

**Purpose**: Property listings with details and media
**Key Fields**: id, title, price, city, property_type, agency_id
**Relationships**: Many-to-one with agencies, one-to-many with tour_reservations

#### tour_reservations
```sql
CREATE TABLE tour_reservations (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID REFERENCES users(id) ON DELETE CASCADE,
  property_id UUID REFERENCES properties(id) ON DELETE CASCADE,
  scheduled_date TIMESTAMP WITH TIME ZONE NOT NULL,
  deposit_amount DECIMAL(12,2) NOT NULL,
  status reservation_status DEFAULT 'PENDING',
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

**Purpose**: Tour bookings with deposit tracking
**Key Fields**: id, user_id, property_id, scheduled_date, deposit_amount, status
**Relationships**: Many-to-one with users and properties, one-to-many with transactions

#### transactions
```sql
CREATE TABLE transactions (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  amount DECIMAL(12,2) NOT NULL,
  type transaction_type NOT NULL,
  status transaction_status DEFAULT 'PENDING',
  reservation_id UUID REFERENCES tour_reservations(id) ON DELETE SET NULL,
  user_id UUID REFERENCES users(id) ON DELETE CASCADE,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

**Purpose**: Financial transactions and commission tracking
**Key Fields**: id, amount, type, status, reservation_id, user_id
**Relationships**: Many-to-one with users and tour_reservations

## Data Relationships

```
users ──► tour_reservations (One-to-Many)
agencies ──► properties (One-to-Many)
properties ──► tour_reservations (One-to-Many)
tour_reservations ──► transactions (One-to-Many)
users ──► transactions (One-to-Many)
```

## Security & Access Control

### Row Level Security (RLS)
```sql
-- Enable RLS on all tables
ALTER TABLE users ENABLE ROW LEVEL SECURITY;
ALTER TABLE agencies ENABLE ROW LEVEL SECURITY;
ALTER TABLE properties ENABLE ROW LEVEL SECURITY;
ALTER TABLE tour_reservations ENABLE ROW LEVEL SECURITY;
ALTER TABLE transactions ENABLE ROW LEVEL SECURITY;
```

### RLS Policies
```sql
-- Users can view their own profile
CREATE POLICY "Users can view their own profile" ON users
  FOR SELECT USING (auth.uid() = id);

-- Anyone can view available properties
CREATE POLICY "Anyone can view available properties" ON properties
  FOR SELECT USING (status = 'AVAILABLE');

-- Users can view their own reservations
CREATE POLICY "Users can view their own reservations" ON tour_reservations
  FOR SELECT USING (user_id = auth.uid());
```

## Performance Considerations

### Indexing Strategy
```sql
-- Performance indexes
CREATE INDEX idx_properties_city ON properties(city);
CREATE INDEX idx_properties_price ON properties(price);
CREATE INDEX idx_properties_status ON properties(status);
CREATE INDEX idx_reservations_user ON tour_reservations(user_id);
CREATE INDEX idx_reservations_property ON tour_reservations(property_id);
CREATE INDEX idx_transactions_user ON transactions(user_id);
```

### Query Optimization
- **N+1 Query Elimination**: Use joins instead of multiple queries
- **Connection Pooling**: Supabase connection pooling for production
- **Read Replicas**: Future AWS RDS read replicas for scaling

## Migration Strategy

### Schema Changes
- **Development**: Prisma migrations with `prisma migrate dev`
- **Production**: Prisma migrations with `prisma migrate deploy`
- **Version Control**: All schema changes tracked in Git

### Data Migration
- **Development**: Prisma seeding with sample data
- **Production**: Automated backup and restore procedures
- **Testing**: Comprehensive migration testing before production

## Related Documentation

- [System Architecture](overview.md)
- [API Specifications](api.md)
- [Current Status](../status/progress.yaml)
- [Setup Guide](../guides/setup.md)