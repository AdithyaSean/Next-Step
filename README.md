# Next Step 🎓

An AI-powered educational and career pathway recommendation system that helps students make informed decisions about their academic and career paths. Built with Flutter frontend and Spring Boot microservices backend.

## 🌟 System Architecture

### Authentication & Security
- Firebase Authentication for secure user management
- Role-based access control
- API Gateway for centralized security

### Service Communication
- REST APIs for synchronous requests
- Event-driven architecture for asynchronous operations

## 🚀 Microservices

The project follows a microservices architecture with the following components:

### Core Services
1. **API Gateway** (`next-step-gateway`)
   - Route management
   - Authentication validation
   - Rate limiting
   - Load balancing

2. **User Management Service** (`next-step-users`)
   - Firebase authentication integration
   - Base user management with inheritance hierarchy
   - Student profiles (extends User)
   - Institution profiles (extends User)
   - Role-based authorization
   - Academic records tracking

3. **Education Service** (`next-step-education`)
   - Course management
   - Stream management
   - Career path data
   - Educational recommendations

4. **Recommendation Engine** (`next-step-recommendations`)
   - AI/ML models for career prediction
   - Skills analysis
   - Course compatibility
   - Success probability calculation

5. **Frontend** (`next-step-flutter`)
   - Flutter-based UI
   - Responsive design
   - Progressive Web App

### Service Dependencies
```mermaid
graph TD
    A[Frontend] --> B[API Gateway]
    B --> C[User Management]
    B --> D[Education Service]
    B --> E[Recommendation Engine]
    D --> C
    E --> C
    E --> D
```

## 🛠️ Tech Stack

### Frontend
- Flutter
- Firebase Authentication
- Progressive Web App

### Backend
- Spring Boot
- Spring Cloud
- PostgreSQL
- Firebase Admin SDK

### DevOps
- Docker

## 🚀 Getting Started

### Prerequisites
- Node.js (v18+)
- Java 17
- Docker & Docker Compose
- Firebase Project
- PostgreSQL

### Local Development Setup

1. **Clone with submodules**:
   ```bash
   git clone --recursive https://github.com/AdithyaSean/Next-Step.git
   cd Next-Step
   ```

   If you've already cloned the project without submodules:
   ```bash
   git submodule init
   git submodule update
   ```

   To update all submodules to their latest commits:
   ```bash
   git submodule update --remote --merge
   ```

2. **Environment Setup**:
   ```bash
   # Copy example env files
   cp .env.example .env
   cd next-step-frontend && cp .env.example .env
   ```

3. **Firebase Setup**:
   - Create a Firebase project
   - Enable Authentication
   - Download service account key
   - Place in appropriate service directories

4. **Start Services**:
   ```bash
   # Start infrastructure
   docker-compose up -d

   # Start backend services
   ./mvnw spring-boot:run -pl next-step-gateway
   ./mvnw spring-boot:run -pl next-step-users
   ./mvnw spring-boot:run -pl next-step-education
   ./mvnw spring-boot:run -pl next-step-recommendations

   # Start frontend
   cd next-step-flutter
   flutter pub get
   flutter run
   ```

## 📝 Documentation

Detailed documentation available in `/docs`:
- [Microservices](docs/microservices.md)
- [Development Roadmap](docs/roadmap.md)
- [API Details](docs/api.md)
- [Interfaces](interfaces/)

## 🤝 Contributing

1. Fork the repository
2. Create feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit changes (`git commit -m 'Add AmazingFeature'`)
4. Push to branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
