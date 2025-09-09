# Reginald v2 – Technical Documentation

Reginald is a Financial Crime and Compliance AI Chatbot designed to streamline AML, Fraud, and Compliance workflows. It provides a secure full-stack architecture with a modern frontend, scalable backend, and AI-powered chat interface.

## 📌 Table of Contents
- Overview
- Architecture
- Frontend
- Backend
- Shared Modules
- Deployment
- Product Vision
- Features
- Future Enhancements

## 1. Overview

**Project Name:** Reginald v2

**Category:** AI-powered Financial Crime & Compliance Chatbot

**Stack:**
- **Frontend:** React + Vite + Tailwind v4 + ShadCN UI
  - Modern component-based architecture with React 18's concurrent rendering
  - Vite build system offering optimized production builds
  - Latest Tailwind v4

- **Backend:** FastAPI (Python)
  - Asynchronous Python framework
  - Easy integration with Python data science and ML libraries

- **AI Model:** Gemini API (messages streamed via Server-Sent Events)
  - Enterprise-grade LLM with financial compliance domain knowledge
  - Secure, tokenized API access with rate limiting capabilities
  - Real-time streaming responses for natural conversation 
  - Context-aware responses with multi-turn conversation history

- **Shared:** Types, utilities, and configuration files
  - Environment configuration with validation
  - Shared constants and utility functions

- **Authentication:** User login with secure session handling
  - JWT token-based authentication with refresh token rotation
  - Role-based access control for different user types
  - Session timeout and security headers implementation

**Core Flow:**
1. User logs in through secure authentication portal
2. User lands on the dashboard with compliance case management overview
3. Chat interface connects with Gemini API through encrypted backend proxy

## 2. Architecture

```
fullstack-shadcn-app/
├── frontend/           # React + Vite + Tailwind v4 + ShadCN UI
│   ├── src/
│   │   ├── components/ # Reusable UI components
│   │   ├── pages/      # Route-based page components
│   │   ├── hooks/      # Custom React hooks
│   │   ├── lib/        # Utility functions and APIs
│   │   ├── store/      # State management
│   │   └── types/      # TypeScript definitions
│   ├── public/         # Static assets
│   ├── vite.config.ts  # Vite configuration
│   └── package.json    # Frontend dependencies
├── backend/            # FastAPI + Python
│   ├── app/
│   │   ├── api/        # API route handlers
│   │   ├── core/       # Core application logic
│   │   ├── models/     # Pydantic models
│   │   ├── services/   # Business logic services
│   │   └── utils/      # Utility functions
│   ├── tests/          # Backend test suite
│   └── requirements.txt # Python dependencies
├── shared/             # Shared types/configs
│   ├── types/          # TypeScript interfaces
│   └── config/         # Environment configurations
├── package.json        # Root scripts and dependencies
└── README.md           # Project overview
```

**Monorepo Structure Benefits:**
- Monorepo structure keeps frontend, backend, and shared resources aligned in a single repository
- Simplified dependency management and version control
- Atomic commits across related frontend and backend changes
- Streamlined CI/CD pipeline for the entire application
- Consistent coding standards and linting rules

**Security Architecture:**
- Gemini API integration is handled via backend proxy for security, preventing API key exposure
- All API requests authenticated through JWT verification middleware
- HTTPS-only communication with proper CORS configuration
- Rate limiting to prevent abuse and DoS attacks

**Performance Optimization:**
- Streaming Responses: Implemented with Server-Sent Events (SSE) for real-time AI replies
- Edge caching for static assets and API responses where appropriate
- Lazy loading of components and code splitting for optimal bundle sizes
- Containerized deployment for horizontal scaling

## 3. Frontend

**Tech Stack:**
- **React + Vite** – Fast, modern frontend build system
  - Component-based architecture with React 18 features
  - Suspense for data fetching and code splitting
  - Client-side routing with React Router v6
  - Vite for near-instantaneous hot module replacement

- **Tailwind v4** – Utility-first CSS framework
  - Zero-runtime CSS-in-JS solution
  - Responsive design system with breakpoints
  - Custom theming with extended color palettes
  - Dark mode support with persistent user preference

- **ShadCN UI** – Pre-styled UI components for consistency
  - Accessible components following WAI-ARIA practices
  - Themeable design system with consistent styling
  - Form elements, dialogs, tooltips, and notifications
  - Data tables and visualization components

**Key Components:**
- **Login Page** – User authentication entry point
  - Form validation with error handling
  - Password strength requirements
  - "Remember me" functionality
  - Password reset flow

- **Dashboard** – Main workspace after login
  - Case overview metrics and visualizations
  - Recent activity timeline
  - Quick access to frequent compliance tasks
  - Notification center for alerts and updates

- **Chat Interface** – Core interaction point with Reginald
  - Context-aware conversation history
  - File attachment support for document analysis
  - Syntax highlighting for code and regulation references
  - Export conversation to PDF for documentation
  - Messages streamed with SSE for word-by-word rendering
  - Styled with Tailwind + ShadCN for consistent UI/UX

**State Management:**
- React Context API for global state
- React Query for server state management
- Local storage for persistent user preferences

**Development Commands:**
```bash
cd frontend
npm install
npm run dev
npm run build
npm run test
npm run lint
```

## 4. Backend

**Tech Stack:**
- **FastAPI (Python)** – High-performance backend API
  - OpenAPI documentation with Swagger UI
  - Automatic request validation with Pydantic
  - Dependency injection system
  - Async request handling for high throughput

- **Authentication** – Manages user sessions and tokens
  - JWT token generation and validation
  - Password hashing with bcrypt
  - Role-based middleware for access control
  - Session management with Redis (optional)

- **AI Integration** – Routes requests to Gemini API and streams responses
  - Prompt engineering and context management
  - Error handling and fallback responses
  - Rate limiting and usage tracking
  - Response filtering for compliance and security

**Endpoints (examples):**
- **POST /auth/login** → Authenticate user
  - Accepts username/password credentials
  - Returns JWT token with user information
  - Includes refresh token for session extension

- **POST /chat/send** → Send a message to Reginald
  - Accepts message content and conversation context
  - Validates input for security and compliance
  - Forwards sanitized request to Gemini API
  - Returns message ID for tracking

- **GET /chat/stream** → Stream AI response via SSE
  - Establishes SSE connection with client
  - Streams tokens from Gemini API in real-time
  - Handles disconnections gracefully
  - Includes message completion events

- **GET /users/me** → Get current user profile
  - Returns user details and permissions
  - Includes usage statistics and preferences

- **GET /compliance/regulations** → Fetch relevant regulations
  - Filters by jurisdiction and category
  - Returns structured regulation data

**Development Setup:**
```bash
cd backend
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install -r requirements.txt
uvicorn main:app --reload --port 8000
```

**Testing:**
```bash
pytest
pytest --cov=app tests/
```

## 5. Shared Modules

**Purpose:**
- Define types, constants, and configurations used by both frontend & backend
- Ensures type safety and single source of truth for common definitions
- Reduces duplication and maintains consistency across the stack
- Simplifies API contract maintenance between frontend and backend

**Structure:**
```
shared/
├── types/
│   ├── auth.ts         # Authentication related types
│   ├── chat.ts         # Chat and message interfaces
│   ├── user.ts         # User profile interfaces
│   └── compliance.ts   # Compliance domain models
├── constants/
│   ├── endpoints.ts    # API endpoint paths
│   ├── errors.ts       # Error codes and messages
│   └── config.ts       # Shared configuration values
└── utils/
    ├── validation.ts   # Shared validation logic
    ├── formatting.ts   # Date/time/currency formatting
    └── testing.ts      # Test utilities
```

**Examples:**
- **User session types**
  - User interface with roles and permissions
  - JWT payload structure
  - Authentication request/response interfaces

- **API response formats**
  - Standardized success/error response wrappers
  - Pagination structures for list endpoints
  - Streaming message formats for chat

- **Shared utility functions**
  - Date formatting for consistent timestamps
  - Input validation rules and regex patterns
  - Testing helpers for both frontend and backend

**Usage:**
- Frontend imports shared types for API requests/responses
- Backend imports shared constants for consistent error codes
- CI/CD uses shared validation for environment configuration

## 6. Deployment

**Options:**

**Local Dev:**
- Run backend (FastAPI + Uvicorn)
  ```bash
  cd backend
  uvicorn main:app --reload --host 0.0.0.0 --port 8000
  ```
- Run frontend (Vite server)
  ```bash
  cd frontend
  npm run dev
  ```

**Production Deployment:**

- **Backend: HEROKU**
  - HEROKU: containerized deployment
  - Self-hosted: Localhost for development

  

- **Database/Storage**
  - MSSQL for relational data storage
  - PineCone for document-based storage

**CI/CD Recommendations:**
- **Use GitHub Actions for testing + deployment**
  - Automated testing on pull requests
  - Staging deployment for preview environments
  - Production deployment on main branch merges
  - Security scanning and vulnerability checks


**Environment Management:**
- Separate development, staging, and production environments
- Environment variables for configuration management
- Secrets management with AWS Secrets Manager/GCP Secret Manager
- Infrastructure as Code with Terraform/CloudFormation

## 7. Product Vision

Reginald aims to be the go-to compliance AI assistant for:

- **Financial Crime Risk**
  - Transaction monitoring alerts triage
  - Risk assessment and scoring
  - Unusual activity pattern detection
  - Due diligence research assistance

- **AML Investigations**
  - Case investigation acceleration
  - Regulatory requirement guidance
  - Documentation and evidence collection
  - Report generation assistance

- **Regulatory Compliance**
  - Policy interpretation and application
  - Regulatory change management
  - Compliance requirement explanations
  - Control framework assessment

- **Fraud Detection Support**
  - Fraud pattern recognition
  - Identity verification assistance
  - Scam and fraud scheme identification
  - Investigation procedure guidance

**Business Value Proposition:**
By combining AI-powered chat with compliance expertise, Reginald saves analysts hours of manual work through:
- 70% reduction in time spent researching regulations
- 50% faster case documentation and reporting
- 85% improved consistency in compliance decisions
- 40% decrease in false positive investigation time

**Target Users:**
- Compliance Officers and Managers
- AML and Fraud Investigators
- Risk Analysts and Specialists
- Regulatory Affairs Teams
- Financial Crime Units

## 8. Features

**Current Features:**

✅ **Secure Login & Authentication**
- Multi-factor authentication support
- Role-based access control
- Session management with timeout
- Secure password policies

✅ **Dashboard with AI Chat Interface**
- Intuitive conversation design
- Message history with search
- Quick prompt templates
- Rich text formatting support

✅ **Streaming AI Responses (SSE)**
- Real-time token streaming
- Typing indicators
- Cancelable responses
- Citation and source linking

✅ **Responsive UI with Tailwind & ShadCN**
- Mobile-optimized layouts
- Dark/light theme switching
- Accessibility compliance (WCAG 2.1)
- Consistent design language

✅ **FastAPI backend with Gemini API integration**
- Secure API key management
- Prompt engineering optimizations
- Context window management
- Error handling and fallbacks

**Upcoming Features:**

🔜 **Knowledge Base Integration (Docs, Policies, Case Studies)**
- Document uploading and indexing
- Semantic search across documents
- Citation of sources in responses
- Domain-specific knowledge graphs

🔜 **Multi-user role management (Admin, Analyst)**
- Team workspace organization
- Custom role definitions
- Permission management
- User activity monitoring

🔜 **Audit Log & Case Tracking**
- Comprehensive audit trails
- Case lifecycle management
- Decision documentation
- Regulatory reporting preparation

## 9. Future Enhancements

**Database integration (Postgres / MongoDB) for persistent chat history**
- Complete conversation history storage
- Message tagging and categorization
- Advanced search and filtering
- Analytics on conversation patterns

**Compliance workflow automation (Suspicious Activity Reports, Alerts)**
- Automated form filling
- Regulatory report generation
- Risk scoring calculations
- Alert routing and assignment

**Integration with transaction monitoring systems**
- Real-time data connectivity
- Transaction visualization
- Alert correlation
- Pattern analysis and recommendations

**Multilingual support for global compliance teams**
- Multi-language chat interface
- Cross-language knowledge base
- Regulatory requirements by jurisdiction
- Region-specific compliance guidance

**Custom AI fine-tuning on financial compliance datasets**
- Domain-specific model training
- Compliance terminology understanding
- Financial regulation corpus
- Company policy alignment

**Advanced Analytics Dashboard**
- Compliance efficiency metrics
- Team performance tracking
- Risk trend visualization
- Resource allocation optimization
