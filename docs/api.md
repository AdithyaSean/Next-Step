# Next Step API Documentation

## Base URLs
- Local: `http://localhost:8080/api/v1`
- Development: `https://api-dev.nextstep.edu/v1`
- Production: `https://api.nextstep.edu/v1`

## Authentication
All endpoints require JWT authentication unless specified otherwise.
```http
Authorization: Bearer <token>
```

## Core Endpoints

### Auth Service
```http
POST /auth/register
POST /auth/login
POST /auth/refresh
GET  /auth/me
```

### Students
```http
GET    /students/{id}
PUT    /students/{id}
GET    /students/{id}/predictions
POST   /students/{id}/skills
DELETE /students/{id}
```

### Institutions
```http
GET    /institutions
GET    /institutions/{id}
POST   /institutions
PUT    /institutions/{id}
DELETE /institutions/{id}
```

### Education
```http
GET /streams
GET /streams/{id}
GET /courses
GET /courses/{id}
GET /careers
GET /careers/{id}
```

### Recommendations
```http
GET  /recommendations/careers/{studentId}
POST /recommendations/analyze
GET  /recommendations/courses/{studentId}
```

## Request/Response Examples

### Register User
```http
POST /auth/register
Content-Type: application/json

{
  "name": "John Doe",
  "email": "john@example.com",
  "password": "secure123",
  "role": "student"
}
```

### Update Student Profile
```http
PUT /students/{id}
Content-Type: application/json

{
  "school": "High School Name",
  "stream": "Physical Science",
  "olResults": {
    "Mathematics": "A",
    "Science": "A"
  }
}
```

### Get Career Recommendations
```http
GET /recommendations/careers/{studentId}

Response:
{
  "predictions": [
    {
      "career": "Software Engineer",
      "probability": 0.85,
      "factors": ["skills", "interests", "academic_performance"]
    }
  ]
}
```

## Status Codes
- `200` - Success
- `201` - Created
- `400` - Bad Request
- `401` - Unauthorized
- `403` - Forbidden
- `404` - Not Found
- `500` - Server Error

## Rate Limiting
- 100 requests per minute per IP
- Status `429` when exceeded

## Response Format
All responses follow the format:
```json
{
  "status": "success|error",
  "data": {},
  "message": "string"
}
```

## Error Format
```json
{
  "status": "error",
  "error": {
    "code": "ERROR_CODE",
    "message": "Error description"
  }
}
```

## Versioning
API versioning is managed through URL path: `/api/v1/`
Breaking changes will result in a new version number.
