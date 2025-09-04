# Database Architecture - Radios Blog

> **AI Context**: This is the single source of truth for database design.
> For current status: see `../status/progress.yaml`
> For system overview: see `overview.md`

## Database Overview

Radios Blog uses PostgreSQL via Supabase as the primary data store with a well-defined schema designed for vintage radio collections, blog content, and postcard galleries.

## Database Schema

### Core Tables

#### users
```sql
CREATE TABLE users (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  email TEXT UNIQUE NOT NULL,
  name TEXT NOT NULL,
  role user_role DEFAULT 'USER',
  avatar_url TEXT,
  bio TEXT,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

**Purpose**: User accounts with role-based access control
**Key Fields**: id, email, name, role, avatar_url, bio, created_at, updated_at
**Relationships**: One-to-many with favorites, blog_comments, user_collections

#### vintage_radios
```sql
CREATE TABLE vintage_radios (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  name TEXT NOT NULL,
  brand TEXT NOT NULL,
  model TEXT NOT NULL,
  year_manufactured INTEGER,
  condition condition_rating NOT NULL,
  price DECIMAL(10,2) NOT NULL,
  currency TEXT DEFAULT 'USD',
  description TEXT,
  specifications JSONB,
  dimensions JSONB,
  weight_kg DECIMAL(5,2),
  power_requirements TEXT,
  features TEXT[],
  images TEXT[] DEFAULT '{}',
  status radio_status DEFAULT 'AVAILABLE',
  seller_id UUID REFERENCES users(id) ON DELETE CASCADE,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

**Purpose**: Vintage radio listings with detailed specifications
**Key Fields**: id, name, brand, model, year_manufactured, condition, price, specifications
**Relationships**: Many-to-one with users (seller), one-to-many with images, favorites

#### blog_posts
```sql
CREATE TABLE blog_posts (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  title TEXT NOT NULL,
  slug TEXT UNIQUE NOT NULL,
  excerpt TEXT,
  content TEXT NOT NULL,
  author_id UUID REFERENCES users(id) ON DELETE CASCADE,
  status post_status DEFAULT 'DRAFT',
  published_at TIMESTAMP WITH TIME ZONE,
  featured_image TEXT,
  tags TEXT[] DEFAULT '{}',
  category TEXT,
  read_time_minutes INTEGER,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

**Purpose**: Blog posts about radio history and station information
**Key Fields**: id, title, slug, content, author_id, status, published_at, tags
**Relationships**: Many-to-one with users (author), one-to-many with comments

#### vintage_postcards
```sql
CREATE TABLE vintage_postcards (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  title TEXT NOT NULL,
  description TEXT,
  year_created INTEGER,
  artist TEXT,
  publisher TEXT,
  location TEXT,
  category postcard_category NOT NULL,
  condition condition_rating NOT NULL,
  rarity rarity_level DEFAULT 'COMMON',
  estimated_value DECIMAL(8,2),
  currency TEXT DEFAULT 'USD',
  images TEXT[] DEFAULT '{}',
  tags TEXT[] DEFAULT '{}',
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

**Purpose**: Vintage postcard collection with categorization and valuation
**Key Fields**: id, title, year_created, artist, location, category, condition, rarity
**Relationships**: Many-to-many with collections via junction table

#### collections
```sql
CREATE TABLE collections (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  name TEXT NOT NULL,
  description TEXT,
  owner_id UUID REFERENCES users(id) ON DELETE CASCADE,
  is_public BOOLEAN DEFAULT false,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

**Purpose**: User-created collections of radios and postcards
**Key Fields**: id, name, description, owner_id, is_public
**Relationships**: Many-to-one with users, many-to-many with radios and postcards

## Data Relationships

```
users ──► vintage_radios (One-to-Many, as seller)
users ──► blog_posts (One-to-Many, as author)
users ──► collections (One-to-Many, as owner)
vintage_radios ──► favorites (One-to-Many)
blog_posts ──► comments (One-to-Many)
collections ◄──► vintage_radios (Many-to-Many via collection_items)
collections ◄──► vintage_postcards (Many-to-Many via collection_items)
```

## Enums and Types

```sql
-- User roles
CREATE TYPE user_role AS ENUM ('ADMIN', 'MODERATOR', 'USER', 'GUEST');

-- Radio condition ratings
CREATE TYPE condition_rating AS ENUM ('MINT', 'EXCELLENT', 'GOOD', 'FAIR', 'POOR');

-- Radio status
CREATE TYPE radio_status AS ENUM ('AVAILABLE', 'RESERVED', 'SOLD', 'ARCHIVED');

-- Post status
CREATE TYPE post_status AS ENUM ('DRAFT', 'PUBLISHED', 'ARCHIVED');

-- Postcard categories
CREATE TYPE postcard_category AS ENUM ('RADIO_STATION', 'HISTORIC', 'ARTISTIC', 'COMMERCIAL', 'OTHER');

-- Rarity levels
CREATE TYPE rarity_level AS ENUM ('COMMON', 'UNCOMMON', 'RARE', 'VERY_RARE', 'EXTREMELY_RARE');
```

## Security & Access Control

### Row Level Security (RLS)
```sql
-- Enable RLS on all tables
ALTER TABLE users ENABLE ROW LEVEL SECURITY;
ALTER TABLE vintage_radios ENABLE ROW LEVEL SECURITY;
ALTER TABLE blog_posts ENABLE ROW LEVEL SECURITY;
ALTER TABLE vintage_postcards ENABLE ROW LEVEL SECURITY;
ALTER TABLE collections ENABLE ROW LEVEL SECURITY;
```

### RLS Policies
```sql
-- Users can view their own profile
CREATE POLICY "Users can view their own profile" ON users
  FOR SELECT USING (auth.uid() = id);

-- Anyone can view published blog posts
CREATE POLICY "Anyone can view published posts" ON blog_posts
  FOR SELECT USING (status = 'PUBLISHED');

-- Users can view available radios
CREATE POLICY "Anyone can view available radios" ON vintage_radios
  FOR SELECT USING (status = 'AVAILABLE');

-- Collection owners can manage their collections
CREATE POLICY "Collection owners can manage collections" ON collections
  FOR ALL USING (owner_id = auth.uid());
```

## Performance Considerations

### Indexing Strategy
```sql
-- Performance indexes
CREATE INDEX idx_radios_brand_model ON vintage_radios(brand, model);
CREATE INDEX idx_radios_year ON vintage_radios(year_manufactured);
CREATE INDEX idx_radios_status ON vintage_radios(status);
CREATE INDEX idx_radios_price ON vintage_radios(price);
CREATE INDEX idx_posts_slug ON blog_posts(slug);
CREATE INDEX idx_posts_status ON blog_posts(status);
CREATE INDEX idx_posts_published ON blog_posts(published_at);
CREATE INDEX idx_postcards_category ON vintage_postcards(category);
CREATE INDEX idx_postcards_year ON vintage_postcards(year_created);
```

### Query Optimization
- **N+1 Prevention**: Use joins instead of multiple queries
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