# Task Manager API - Complete Guide

## Table of Contents
1. [Project Overview](#project-overview)
2. [Project Structure](#project-structure)
3. [Technology Stack](#technology-stack)
4. [Setup Instructions](#setup-instructions)
5. [Database Schema](#database-schema)
6. [API Endpoints](#api-endpoints)
7. [Code Explanation](#code-explanation)
8. [Error Handling](#error-handling)
9. [Testing the API](#testing-the-api)
10. [Frontend Interface](#frontend-interface)

---

## Project Overview

The **Task Manager API** is a full-stack web application that allows users to create, read, update, and delete tasks. It's built with Node.js and Express on the backend, MongoDB for data storage, and includes a beautiful, animated frontend interface.

### What Does It Do?
- ✅ Create new tasks with a name and description
- ✅ View all tasks in a list
- ✅ Mark tasks as complete or incomplete
- ✅ Delete tasks you no longer need
- ✅ Store all data permanently in a MongoDB database
- ✅ Beautiful animated user interface

### How Does It Work?
The application follows a **client-server architecture**:
1. **Frontend** (HTML/CSS/JavaScript) - User interface in the browser
2. **Backend** (Node.js/Express) - Handles requests and business logic
3. **Database** (MongoDB) - Stores task data permanently

When you interact with the frontend (like clicking "Add Task"), it sends an HTTP request to the backend API. The backend processes the request, talks to the database, and sends back a response. The frontend then updates to show the new data.

---

## Project Structure

```
task-manager-api/
│
├── models/                      # Database models (schemas)
│   └── task.model.js           # Task schema definition
│
├── routes/                      # API route handlers
│   └── task.routes.js          # Task-related routes
│
├── public/                      # Frontend files
│   └── index.html              # Main UI page
│
├── server.js                    # Main server file (entry point)
├── package.json                 # Project dependencies
├── .env.example                 # Environment variables template
└── README.md                    # This file
```

### Why This Structure?
- **Separation of Concerns**: Each folder has a specific purpose
- **Scalability**: Easy to add new models or routes
- **Maintainability**: Clear organization makes code easy to find and update

---

## Technology Stack

| Technology | Purpose | Why We Use It |
|------------|---------|---------------|
| **Node.js** | Runtime environment | Allows JavaScript to run on the server |
| **Express.js** | Web framework | Simplifies building APIs and handling HTTP requests |
| **MongoDB** | Database | NoSQL database perfect for flexible data structures |
| **Mongoose** | ODM (Object Data Modeling) | Makes working with MongoDB easier and adds validation |

---

## Setup Instructions

### Prerequisites
Before starting, make sure you have:
- Node.js installed (version 14 or higher)
- A MongoDB database (MongoDB Atlas free tier or local MongoDB)

### Step 1: Install Dependencies
```bash
npm install
```

This installs:
- `express` - Web framework
- `mongoose` - MongoDB ODM

### Step 2: Set Up MongoDB

#### Option A: MongoDB Atlas (Recommended - Free Cloud Database)
1. Go to [MongoDB Atlas](https://www.mongodb.com/cloud/atlas)
2. Sign up for a free account
3. Create a new cluster (choose the free tier)
4. Click "Connect" → "Connect your application"
5. Copy the connection string (it looks like: `mongodb+srv://username:password@cluster.mongodb.net/...`)
6. Replace `<password>` with your actual database password

#### Option B: Local MongoDB
1. Install MongoDB locally
2. Use connection string: `mongodb://localhost:27017/taskmanager`

### Step 3: Configure Environment Variables
Create a `.env` file in the root directory (or set environment variables in Replit):
```
MONGODB_URI=your_connection_string_here
PORT=5000
```

### Step 4: Whitelist Your IP (MongoDB Atlas Only)
1. In MongoDB Atlas, go to "Network Access"
2. Click "Add IP Address"
3. Add `0.0.0.0/0` to allow connections from anywhere (or add your specific IP)

### Step 5: Run the Server
```bash
node server.js
```

You should see:
```
Connected to MongoDB
Server is running on port 5000
```

### Step 6: Open the Application
- Open your browser and go to `http://localhost:5000`
- You should see the Task Manager interface!

---

## Database Schema

### Task Model (`models/task.model.js`)

```javascript
{
  taskName: {
    type: String,
    required: true
  },
  description: {
    type: String,
    required: true
  },
  completed: {
    type: Boolean,
    default: false
  },
  createdAt: {
    type: Date,
    auto-generated: true
  },
  updatedAt: {
    type: Date,
    auto-generated: true
  }
}
```

### Field Explanations
| Field | Type | Required | Default | Description |
|-------|------|----------|---------|-------------|
| `taskName` | String | Yes | - | The name/title of the task |
| `description` | String | Yes | - | Detailed description of what needs to be done |
| `completed` | Boolean | No | `false` | Whether the task is complete or not |
| `createdAt` | Date | Auto | Current time | When the task was created |
| `updatedAt` | Date | Auto | Current time | Last time the task was modified |

### Validation Rules
- `taskName` must be provided and cannot be empty
- `description` must be provided and cannot be empty
- `completed` automatically defaults to `false` if not specified
- MongoDB automatically generates a unique `_id` for each task

---

## API Endpoints

### Base URL
```
http://localhost:5000/api
```

### 1. GET /tasks - Get All Tasks

**Description**: Retrieves all tasks from the database

**Request**:
```http
GET /api/tasks
```

**Response** (200 OK):
```json
[
  {
    "_id": "507f1f77bcf86cd799439011",
    "taskName": "Complete project documentation",
    "description": "Write comprehensive README for the Task Manager API",
    "completed": false,
    "createdAt": "2025-11-24T10:00:00.000Z",
    "updatedAt": "2025-11-24T10:00:00.000Z",
    "__v": 0
  },
  {
    "_id": "507f1f77bcf86cd799439012",
    "taskName": "Review code",
    "description": "Review and test all API endpoints",
    "completed": true,
    "createdAt": "2025-11-24T11:00:00.000Z",
    "updatedAt": "2025-11-24T11:30:00.000Z",
    "__v": 0
  }
]
```

**How It Works**:
1. Client sends GET request to `/api/tasks`
2. Server queries MongoDB for all tasks using `Task.find({})`
3. MongoDB returns all task documents
4. Server sends tasks as JSON array to client

---

### 2. POST /tasks - Create New Task

**Description**: Creates a new task with the provided data

**Request**:
```http
POST /api/tasks
Content-Type: application/json

{
  "taskName": "Buy groceries",
  "description": "Get milk, eggs, bread, and vegetables"
}
```

**Response** (201 Created):
```json
{
  "_id": "507f1f77bcf86cd799439013",
  "taskName": "Buy groceries",
  "description": "Get milk, eggs, bread, and vegetables",
  "completed": false,
  "createdAt": "2025-11-24T12:00:00.000Z",
  "updatedAt": "2025-11-24T12:00:00.000Z",
  "__v": 0
}
```

**Validation Error Response** (400 Bad Request):
```json
{
  "message": "Validation failed",
  "errors": {
    "taskName": null,
    "description": "Description is required"
  }
}
```

**How It Works**:
1. Client sends POST request with task data in request body
2. Server validates that `taskName` and `description` are provided
3. If validation fails, server returns 400 error
4. If valid, server creates new Task document
5. MongoDB saves the task and auto-generates `_id`, timestamps
6. Server returns the created task with 201 status

---

### 3. PATCH /tasks/:id - Toggle Task Completion

**Description**: Toggles the completion status of a task (complete ↔ incomplete)

**Request**:
```http
PATCH /api/tasks/507f1f77bcf86cd799439013
```

**Response** (200 OK):
```json
{
  "_id": "507f1f77bcf86cd799439013",
  "taskName": "Buy groceries",
  "description": "Get milk, eggs, bread, and vegetables",
  "completed": true,
  "createdAt": "2025-11-24T12:00:00.000Z",
  "updatedAt": "2025-11-24T12:05:00.000Z",
  "__v": 0
}
```

**Invalid ID Response** (400 Bad Request):
```json
{
  "message": "Invalid task ID"
}
```

**Task Not Found Response** (404 Not Found):
```json
{
  "message": "Task not found"
}
```

**How It Works**:
1. Client sends PATCH request with task ID in URL
2. Server validates the ID format using `mongoose.Types.ObjectId.isValid()`
3. If invalid, returns 400 error
4. Server finds task by ID using `Task.findById()`
5. If not found, returns 404 error
6. Server toggles `completed` value (`true` → `false` or `false` → `true`)
7. Server saves the updated task
8. MongoDB updates the `updatedAt` timestamp automatically
9. Server returns the updated task

---

### 4. DELETE /tasks/:id - Delete Task

**Description**: Permanently deletes a task from the database

**Request**:
```http
DELETE /api/tasks/507f1f77bcf86cd799439013
```

**Response** (200 OK):
```json
{
  "message": "Task deleted successfully",
  "task": {
    "_id": "507f1f77bcf86cd799439013",
    "taskName": "Buy groceries",
    "description": "Get milk, eggs, bread, and vegetables",
    "completed": false,
    "createdAt": "2025-11-24T12:00:00.000Z",
    "updatedAt": "2025-11-24T12:00:00.000Z",
    "__v": 0
  }
}
```

**Invalid ID Response** (400 Bad Request):
```json
{
  "message": "Invalid task ID"
}
```

**Task Not Found Response** (404 Not Found):
```json
{
  "message": "Task not found"
}
```

**How It Works**:
1. Client sends DELETE request with task ID in URL
2. Server validates the ID format
3. If invalid, returns 400 error
4. Server deletes task using `Task.findByIdAndDelete()`
5. If task doesn't exist, returns 404 error
6. If successful, returns 200 with deleted task data
7. MongoDB permanently removes the task document

---

## Code Explanation

### 1. server.js - Main Server File

```javascript
const express = require('express');
const mongoose = require('mongoose');
const taskRoutes = require('./routes/task.routes');

const app = express();
const PORT = process.env.PORT || 5000;
const MONGODB_URI = process.env.MONGODB_URI || 'mongodb://localhost:27017/taskmanager';

// Middleware to parse JSON request bodies
app.use(express.json());

// Serve static files from 'public' folder
app.use(express.static('public'));

// Use task routes for all /api endpoints
app.use('/api', taskRoutes);

// Connect to MongoDB and start server
mongoose.connect(MONGODB_URI)
  .then(() => {
    console.log('Connected to MongoDB');
    app.listen(PORT, '0.0.0.0', () => {
      console.log(`Server is running on port ${PORT}`);
    });
  })
  .catch((error) => {
    console.error('MongoDB connection error:', error);
    process.exit(1);
  });
```

**Line-by-Line Explanation**:
- **Lines 1-3**: Import required modules
- **Line 5**: Create Express application instance
- **Lines 6-7**: Set port and MongoDB connection string (with defaults)
- **Line 10**: Enable JSON parsing for request bodies
- **Line 13**: Serve frontend files from 'public' folder
- **Line 16**: Mount task routes at '/api' path
- **Lines 19-29**: Connect to MongoDB, then start server on success, or exit on failure

---

### 2. models/task.model.js - Database Schema

```javascript
const mongoose = require('mongoose');

// Define the schema for tasks
const taskSchema = new mongoose.Schema({
  taskName: {
    type: String,
    required: [true, 'Task name is required']
  },
  description: {
    type: String,
    required: [true, 'Description is required']
  },
  completed: {
    type: Boolean,
    default: false
  }
}, {
  timestamps: true  // Automatically add createdAt and updatedAt
});

// Create and export the model
const Task = mongoose.model('Task', taskSchema);

module.exports = Task;
```

**Key Concepts**:
- **Schema**: Defines the structure of documents in the collection
- **required**: Makes fields mandatory
- **default**: Sets automatic default values
- **timestamps**: Auto-generates createdAt and updatedAt fields
- **Model**: Provides an interface to interact with the database

---

### 3. routes/task.routes.js - API Routes

```javascript
const express = require('express');
const router = express.Router();
const Task = require('../models/task.model');
const mongoose = require('mongoose');

// GET /tasks - Get all tasks
router.get('/tasks', async (req, res) => {
  try {
    const tasks = await Task.find({});
    res.status(200).json(tasks);
  } catch (error) {
    res.status(500).json({ message: 'Error fetching tasks', error: error.message });
  }
});

// POST /tasks - Create a new task
router.post('/tasks', async (req, res) => {
  try {
    const { taskName, description } = req.body;

    // Manual validation
    if (!taskName || !description) {
      return res.status(400).json({ 
        message: 'Validation failed', 
        errors: {
          taskName: !taskName ? 'Task name is required' : null,
          description: !description ? 'Description is required' : null
        }
      });
    }

    // Create new task
    const task = new Task({
      taskName,
      description,
      completed: false
    });

    const savedTask = await task.save();
    res.status(201).json(savedTask);
  } catch (error) {
    res.status(500).json({ message: 'Error creating task', error: error.message });
  }
});

// PATCH /tasks/:id - Toggle task completion
router.patch('/tasks/:id', async (req, res) => {
  try {
    const { id } = req.params;

    // Validate MongoDB ID
    if (!mongoose.Types.ObjectId.isValid(id)) {
      return res.status(400).json({ message: 'Invalid task ID' });
    }

    // Find task
    const task = await Task.findById(id);

    if (!task) {
      return res.status(404).json({ message: 'Task not found' });
    }

    // Toggle completion status
    task.completed = !task.completed;
    const updatedTask = await task.save();

    res.status(200).json(updatedTask);
  } catch (error) {
    res.status(500).json({ message: 'Error updating task', error: error.message });
  }
});

// DELETE /tasks/:id - Delete a task
router.delete('/tasks/:id', async (req, res) => {
  try {
    const { id } = req.params;

    // Validate MongoDB ID
    if (!mongoose.Types.ObjectId.isValid(id)) {
      return res.status(400).json({ message: 'Invalid task ID' });
    }

    // Delete task
    const task = await Task.findByIdAndDelete(id);

    if (!task) {
      return res.status(404).json({ message: 'Task not found' });
    }

    res.status(200).json({ message: 'Task deleted successfully', task });
  } catch (error) {
    res.status(500).json({ message: 'Error deleting task', error: error.message });
  }
});

module.exports = router;
```

**Key Concepts**:
- **router**: Express Router for organizing routes
- **async/await**: Modern JavaScript for handling asynchronous operations
- **try/catch**: Error handling pattern
- **req.body**: Contains data sent in POST requests
- **req.params**: Contains URL parameters (like :id)
- **res.status()**: Sets HTTP status code
- **res.json()**: Sends JSON response

---

## Error Handling

The API implements comprehensive error handling for various scenarios:

### 1. Validation Errors (400 Bad Request)
**When**: Required fields are missing

**Example**:
```javascript
POST /api/tasks
{
  "taskName": "Test"
  // Missing description
}

Response:
{
  "message": "Validation failed",
  "errors": {
    "taskName": null,
    "description": "Description is required"
  }
}
```

---

### 2. Invalid ID Format (400 Bad Request)
**When**: MongoDB ID is not in correct format

**Example**:
```javascript
PATCH /api/tasks/invalid-id-123

Response:
{
  "message": "Invalid task ID"
}
```

**Why**: MongoDB ObjectIDs must be 24 hex characters. The server validates this before querying the database to prevent errors.

---

### 3. Resource Not Found (404 Not Found)
**When**: Task with given ID doesn't exist

**Example**:
```javascript
DELETE /api/tasks/507f1f77bcf86cd799439011

Response:
{
  "message": "Task not found"
}
```

---

### 4. Server Errors (500 Internal Server Error)
**When**: Database connection fails or unexpected errors occur

**Example**:
```javascript
{
  "message": "Error fetching tasks",
  "error": "Connection to database failed"
}
```

**Why**: The try/catch blocks catch any unexpected errors and return a 500 status with error details.

---

## Testing the API

### Method 1: Using cURL (Command Line)

```bash
# Get all tasks
curl http://localhost:5000/api/tasks

# Create a task
curl -X POST http://localhost:5000/api/tasks \
  -H "Content-Type: application/json" \
  -d '{"taskName": "Learn Node.js", "description": "Complete the tutorial"}'

# Toggle completion (replace ID with actual task ID)
curl -X PATCH http://localhost:5000/api/tasks/507f1f77bcf86cd799439011

# Delete a task (replace ID with actual task ID)
curl -X DELETE http://localhost:5000/api/tasks/507f1f77bcf86cd799439011
```

### Method 2: Using Postman

1. Download and install [Postman](https://www.postman.com/)
2. Create a new request
3. Set the method (GET, POST, PATCH, DELETE)
4. Enter the URL (e.g., `http://localhost:5000/api/tasks`)
5. For POST requests, add JSON body in the "Body" tab
6. Click "Send"

### Method 3: Using the Frontend

1. Open `http://localhost:5000` in your browser
2. Use the visual interface to:
   - Add tasks using the form
   - View all tasks in the list
   - Click "Mark Complete" to toggle status
   - Click "Delete" to remove tasks

---

## Frontend Interface

The application includes a beautiful, animated frontend with the following features:

### Features
- ✨ **Modern Design**: Gradient background, card-based layout
- 🎨 **Smooth Animations**: 
  - Slide-in effect when tasks are added
  - Celebration animation with checkmark when completing tasks
  - Slide-out animation when deleting tasks
- 📱 **Responsive**: Works on mobile, tablet, and desktop
- 🔔 **User Feedback**: Success and error messages for all actions
- 🎯 **Real-time Updates**: Task count updates automatically

### How Frontend Connects to Backend

The frontend uses the **Fetch API** to communicate with the backend:

```javascript
// Example: Fetching all tasks
const response = await fetch('/api/tasks');
const tasks = await response.json();

// Example: Creating a task
const response = await fetch('/api/tasks', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ taskName, description })
});
```

---

## Common Issues & Solutions

### Issue 1: "MongoDB connection error"
**Solution**: 
- Check that MONGODB_URI is set correctly
- Ensure your IP is whitelisted in MongoDB Atlas
- Verify database username and password are correct

### Issue 2: "Port 5000 already in use"
**Solution**: 
- Change PORT in .env to a different number (e.g., 3000, 8000)
- Or kill the process using port 5000

### Issue 3: "Cannot POST /api/tasks"
**Solution**: 
- Make sure server is running
- Check that you're sending JSON with Content-Type header
- Verify the URL is correct

---

## Next Steps

Now that you have a working Task Manager API, you can:

1. **Add More Features**:
   - User authentication
   - Task categories/tags
   - Due dates and priorities
   - Task filtering and sorting
   - Search functionality

2. **Improve Code**:
   - Add input sanitization
   - Implement rate limiting
   - Add request logging
   - Write automated tests

3. **Deploy Your App**:
   - Use Replit's built-in deployment
   - Deploy to Heroku, Railway, or Render
   - Set up a custom domain

---

## Summary

Congratulations! You now have a complete, production-ready Task Manager API with:

✅ **Backend**: Node.js + Express + MongoDB  
✅ **Database**: Mongoose schemas with validation  
✅ **API**: RESTful endpoints with error handling  
✅ **Frontend**: Beautiful animated user interface  
✅ **Documentation**: Complete guide for beginners  

**Key Concepts You Learned**:
- Building REST APIs with Express
- Database modeling with Mongoose
- Async/await for handling asynchronous code
- Input validation and error handling
- Client-server communication
- Full-stack application architecture

Keep building and happy coding! 🚀
