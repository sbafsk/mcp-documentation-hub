# Database Architecture

> **AI Context**: This is the single source of truth for database design.
> For current status: see `../status/progress.yaml`
> For system overview: see `overview.md`

## Database Overview

[Project Name] uses [Your Database Choice] as the primary data store with a well-defined schema designed for [your use case].

## Database Schema

### Core Tables

#### [Table 1]
```sql
CREATE TABLE [table_name] (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  [field1] [type] [constraints],
  [field2] [type] [constraints],
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

**Purpose**: [What this table stores]
**Key Fields**: [Important fields and their purpose]
**Relationships**: [How it connects to other tables]

#### [Table 2]
```sql
CREATE TABLE [table_name] (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  [field1] [type] [constraints],
  [field2] [type] [constraints],
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

**Purpose**: [What this table stores]
**Key Fields**: [Important fields and their purpose]
**Relationships**: [How it connects to other tables]

## Data Relationships

```
[Table 1] ──► [Table 2] (One-to-Many)
[Table 2] ──► [Table 3] (Many-to-One)
[Table 1] ◄──► [Table 3] (Many-to-Many via junction table)
```

## Security & Access Control

- **Row Level Security (RLS)**: [Explain your RLS strategy]
- **User Permissions**: [Explain your permission model]
- **Data Validation**: [Explain your validation approach]

## Performance Considerations

- **Indexing Strategy**: [Explain your indexing approach]
- **Query Optimization**: [Explain your query optimization]
- **Caching Strategy**: [Explain your caching approach]

## Migration Strategy

- **Schema Changes**: [How you handle schema updates]
- **Data Migration**: [How you handle data migrations]
- **Version Control**: [How you version your database schema]

## Related Documentation

- [System Architecture](overview.md)
- [API Specifications](api.md)
- [Security Model](security.md)