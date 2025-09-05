# API Specifications

> **AI Context**: This is the single source of truth for API design.
> For current status: see `../status/progress.yaml`
> For system overview: see `overview.md`

## API Overview

[Project Name] exposes a [Your API Choice] API that provides secure access to application data and functionality.

## API Architecture

### Endpoint Structure

```
/api/
├── /auth/          # Authentication endpoints
├── /[resource1]/   # [Resource 1] management
├── /[resource2]/   # [Resource 2] management
└── /[resource3]/   # [Resource 3] management
```

### Authentication

- **Method**: [Your auth method]
- **Token Type**: [JWT, Session, etc.]
- **Security**: [HTTPS, CORS, etc.]

## Core Endpoints

### Authentication

#### POST /api/auth/login
**Purpose**: User authentication
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
    "role": "user"
  }
}
```

#### POST /api/auth/logout
**Purpose**: User logout
**Headers**: `Authorization: Bearer {token}`
**Response**: `{ "message": "Logged out successfully" }`

### [Resource 1] Management

#### GET /api/[resource1]
**Purpose**: Retrieve [resource1] list
**Headers**: `Authorization: Bearer {token}`
**Query Parameters**:
- `page`: Page number (default: 1)
- `limit`: Items per page (default: 20)
- `[filter]`: [Filter description]

**Response**:
```json
{
  "data": [
    {
      "id": "[id]",
      "[field1]": "[value1]",
      "[field2]": "[value2]"
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 20,
    "total": 100,
    "pages": 5
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
- `500`: Internal Server Error - Server-side error

## Rate Limiting

- **Requests per minute**: [Number]
- **Burst limit**: [Number]
- **Headers**: Rate limit information included in response headers

## API Versioning

- **Current Version**: v1
- **Versioning Strategy**: [URL path, header, etc.]
- **Deprecation Policy**: [How you handle API changes]

## Related Documentation

- [System Architecture](overview.md)
- [Database Schema](database.md)
- [Authentication Flow](auth.md)