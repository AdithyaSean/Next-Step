# Next Step 🎓

An AI-powered educational and career pathway recommendation system that helps students make informed decisions about their academic and career paths. Built with React frontend and planned Spring Boot microservices backend.

## 🌟 Current Features [Alpha]

- **Basic Authentication**:
  - Google authentication via Firebase
  - Basic login/logout functionality

- **Navigation**:
  - Basic routing setup
  - Responsive navigation bar

## 🚀 Planned Features

- **Career Path Recommendation**:
  - Personalized career path recommendations
  - Success probability calculations
  - Real-time AI-powered insights

- **Student Profile Management**:
  - Academic performance tracking
  - Skills and interests input
  - Goals and preferences settings

- **Educational Institution Data**:
  - University field exploration
  - Course compatibility matching
  - Institution information

## 🏗️ Project Structure

```
.
├── src/
│   ├── components/     # Reusable UI components
│   ├── pages/         # Page components
│   ├── services/      # API and external service integrations
│   ├── context/       # React context providers
│   └── utils/         # Helper functions and utilities
├── docs/             # Project documentation
│   ├── architecture.md
│   ├── data-model.md
│   ├── microservices.md
│   └── roadmap.md
└── public/           # Static assets
```

## 🛠️ Tech Stack

- **Frontend**:
  - React + Vite
  - Firebase Authentication
  - React Router
  - CSS Modules

- **Planned Backend**:
  - Spring Boot Microservices
  - PostgreSQL
  - Spring Security
  - Spring Cloud

## 🚀 Getting Started

### Prerequisites
- Node.js (v18+)
- Yarn
- Git
- Firebase Project (for authentication)
- Backend API running (for predictions)

### Local Development Setup

1. **Clone the repository**:
   ```bash
   git clone https://github.com/adithyasean/Next-Step-Frontend.git
   cd Next-Step-Frontend
   ```

2. **Install dependencies**:
   ```bash
   yarn install
   ```

3. **Environment Setup**:
   - Copy `.env.example` to `.env`
   - Fill in your environment variables:
     ```bash
     cp .env.example .env
     ```
   Required variables:
   - `VITE_API_URL`: Your backend API URL
   - Firebase configuration (from Firebase Console):
     - `VITE_FIREBASE_API_KEY`
     - `VITE_FIREBASE_AUTH_DOMAIN`
     - `VITE_FIREBASE_PROJECT_ID`
     - `VITE_FIREBASE_STORAGE_BUCKET`
     - `VITE_FIREBASE_MESSAGING_SENDER_ID`
     - `VITE_FIREBASE_APP_ID`

4. **Start development server**:
   ```bash
   yarn dev
   ```

### Firebase Setup

1. Create a new Firebase project at [Firebase Console](https://console.firebase.google.com)
2. Enable Authentication:
   - Go to Authentication > Sign-in method
   - Enable Google provider
3. Enable Firestore:
   - Go to Firestore Database
   - Create database in production mode
   - Set up Firestore rules:
     ```javascript
     rules_version = '2';
     service cloud.firestore {
       match /databases/{database}/documents {
         match /profiles/{userId} {
           allow read, write: if request.auth != null && request.auth.uid == userId;
         }
       }
     }
     ```
4. Get your Firebase configuration:
   - Go to Project Settings > General
   - Scroll down to "Your apps"
   - Click the web icon (</>)
   - Register your app
   - Copy the configuration to your `.env` file

## 🧪 Testing

```bash
# Run unit tests
yarn test

# Run e2e tests
yarn test:e2e
```

## 📚 Documentation

- [Architecture Overview](docs/architecture.md)
- [Development Roadmap](docs/roadmap.md)
- [Data Models](docs/data-model.md)
- [Microservices Design](docs/microservices.md)

## 🤝 Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📝 Development Status

- **Frontend**: Alpha stage - Basic authentication and navigation implemented
- **Backend**: Planning phase - Architecture and data models designed
- **AI Models**: Core models developed, integration pending

For detailed progress and upcoming features, see our [roadmap](docs/roadmap.md).

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
