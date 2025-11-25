# Task Manager API - Replit Configuration

## Overview

This is a REST API for task management built with Node.js and Express. The application provides CRUD operations for managing tasks with properties including name, description, and completion status. Tasks are persisted using MongoDB with Mongoose as the ODM layer.

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Backend Architecture

**Framework**: Express.js (v5.1.0)
- Chosen for its simplicity and robust middleware ecosystem
- RESTful API design pattern with JSON request/response format
- Centralized routing through dedicated route files in `/routes` directory
- Middleware stack includes JSON body parsing for request handling

**API Design**:
- Resource-based endpoints under `/api` prefix
- Standard HTTP methods (GET, POST, PATCH, DELETE) for CRUD operations
- Informational root endpoint documenting available API routes
- Structured error responses with appropriate HTTP status codes

**Server Configuration**:
- Configurable port via environment variable (defaults to 5000)
- Binds to `0.0.0.0` for compatibility with containerized environments
- Graceful error handling with process termination on database connection failure

### Data Layer

**Database**: MongoDB
- Document-oriented NoSQL database chosen for flexibility with task schema
- Connection managed through Mongoose ODM (v9.0.0)
- Connection string configurable via `MONGODB_URI` environment variable
- Supports both MongoDB Atlas (cloud) and local MongoDB instances

**Data Model** (Task Schema):
- `taskName`: Required string field for task title
- `description`: Required string field for task details  
- `completed`: Boolean flag with default value of false
- Automatic timestamp tracking (`createdAt`, `updatedAt`) via Mongoose schema options
- Mongoose validation ensures required fields are present

**ODM Pattern**:
- Model-based architecture with schemas defined in `/models` directory
- Built-in validation at the schema level
- Async/await pattern for database operations
- ObjectId validation for route parameters

### Error Handling

- Try-catch blocks around all async database operations
- HTTP status codes: 200 (success), 201 (created), 400 (validation error), 500 (server error)
- Structured error messages with error details in response body
- Manual validation for required fields before database operations
- MongoDB ObjectId validation for route parameters

### Frontend Architecture

**User Interface**: Modern Single-Page Application
- Built with vanilla HTML, CSS, and JavaScript
- Served as static files from `/public` directory via Express
- Real-time communication with backend API using Fetch API
- Responsive design with mobile-first approach

**Features**:
- Task creation form with validation
- Dynamic task list rendering
- Smooth animations for task interactions (add, complete, delete)
- Success/error notifications for user feedback
- Task completion statistics display

**Design Elements**:
- Gradient background for visual appeal
- Card-based layout for clean organization
- Animation system: slide-in (add), celebrate (complete), slide-out (delete)
- Color-coded task states (pending: yellow, completed: green)

### Project Structure

```
├── models/          # Mongoose schemas and models
├── routes/          # Express route handlers
├── public/          # Frontend static files
│   └── index.html   # Main UI with embedded CSS and JavaScript
├── server.js        # Application entry point and server configuration
├── package.json     # Dependencies and metadata
├── README.md        # Complete documentation
├── API_DOCUMENTATION.md  # API quick reference
└── .env             # Environment configuration (not in repo)
```

**Design Rationale**:
- Separation of concerns with models and routes in dedicated directories
- Single entry point (`server.js`) for server initialization
- Environment-based configuration for portability across environments
- Frontend served as static files for simplicity and performance

## External Dependencies

### Database Service
- **MongoDB**: Primary data store
  - Can be MongoDB Atlas (cloud-hosted) or local MongoDB instance
  - Connection string must be provided via `MONGODB_URI` environment variable
  - Default fallback: `mongodb://localhost:27017/taskmanager`

### NPM Packages
- **express** (^5.1.0): Web framework for API routing and middleware
- **mongoose** (^9.0.0): MongoDB ODM for schema definition and query building

### Environment Variables
- `MONGODB_URI`: MongoDB connection string (required for production)
- `PORT`: Server port number (optional, defaults to 5000)