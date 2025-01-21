## Microservices Architecture

### Core Services
1. **API Gateway**
   - Single entry point for all requests
   - Handles authentication and routing

2. **User Management**
   - Manages user accounts and profiles
   - Handles authentication

3. **Recommendation Engine**
   - Provides career predictions
   - Uses machine learning models

4. **Frontend**
   - User interface built with Flutter
   - Consumes data from backend services

### Communication
- All requests go through API Gateway
- Services communicate directly for internal operations
- Uses event-driven architecture for async operations
