# Next-Step API Documentation

## Core Endpoints

### Authentication
- All endpoints require Firebase authentication
- Include token in Authorization header: `Bearer <firebase_token>`

### Users
1. **Register User**
   - `POST /register`
   - Request:
     ```json
     {"email": "user@example.com", "password": "securepassword123", "name": "John Doe"}
     ```
   - Response:
     ```json
     {"uid": "firebase-user-id", "email": "user@example.com", "name": "John Doe"}
     ```

2. **Login**
   - `POST /login`
   - Request:
     ```json
     {"email": "user@example.com", "password": "securepassword123"}
     ```
   - Response:
     ```json
     {"token": "firebase-auth-token", "refreshToken": "firebase-refresh-token"}
     ```

### Recommendations
1. **Get Career Predictions**
   - `POST /predict`
   - Request:
     ```json
     {
       "educationLevel": 1,
       "olResults": {"0": 85.0, "1": 78.0, "2": 72.0},
       "alStream": 0,
       "alResults": {"0": 88.0, "1": 82.0, "2": 90.0},
       "gpa": 3.75
     }
     ```
   - Response:
     ```json
     {"Engineering": 85.3, "Medicine": 72.1, "IT": 68.5}
     ```

### Errors
- All endpoints return:
  ```json
  {
    "status": 400,
    "error": "Bad Request",
    "message": "Invalid input data"
  }
  ```
- Common error codes:
  - 400: Bad Request
  - 401: Unauthorized
  - 404: Not Found
  - 500: Server Error
