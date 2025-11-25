# Task Manager API - Quick Reference Guide

## Base URL
```
http://localhost:5000/api
```

## Endpoints Summary

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| GET | `/tasks` | Get all tasks | No |
| POST | `/tasks` | Create a new task | No |
| PATCH | `/tasks/:id` | Toggle task completion | No |
| DELETE | `/tasks/:id` | Delete a task | No |

---

## 1. GET /tasks

### Description
Retrieves all tasks from the database.

### Request
```http
GET /api/tasks
```

### Response (200 OK)
```json
[
  {
    "_id": "507f1f77bcf86cd799439011",
    "taskName": "Complete project documentation",
    "description": "Write comprehensive README",
    "completed": false,
    "createdAt": "2025-11-24T10:00:00.000Z",
    "updatedAt": "2025-11-24T10:00:00.000Z"
  }
]
```

### cURL Example
```bash
curl http://localhost:5000/api/tasks
```

---

## 2. POST /tasks

### Description
Creates a new task with the provided name and description.

### Request
```http
POST /api/tasks
Content-Type: application/json

{
  "taskName": "Buy groceries",
  "description": "Get milk, eggs, bread"
}
```

### Request Body Parameters

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| taskName | String | Yes | The name of the task |
| description | String | Yes | Detailed description |

### Success Response (201 Created)
```json
{
  "_id": "507f1f77bcf86cd799439013",
  "taskName": "Buy groceries",
  "description": "Get milk, eggs, bread",
  "completed": false,
  "createdAt": "2025-11-24T12:00:00.000Z",
  "updatedAt": "2025-11-24T12:00:00.000Z"
}
```

### Error Response (400 Bad Request)
```json
{
  "message": "Validation failed",
  "errors": {
    "taskName": "Task name is required",
    "description": null
  }
}
```

### cURL Example
```bash
curl -X POST http://localhost:5000/api/tasks \
  -H "Content-Type: application/json" \
  -d '{
    "taskName": "Buy groceries",
    "description": "Get milk, eggs, bread"
  }'
```

---

## 3. PATCH /tasks/:id

### Description
Toggles the completion status of a task (marks it complete or incomplete).

### Request
```http
PATCH /api/tasks/507f1f77bcf86cd799439013
```

### URL Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| id | String | Yes | MongoDB ObjectId of the task |

### Success Response (200 OK)
```json
{
  "_id": "507f1f77bcf86cd799439013",
  "taskName": "Buy groceries",
  "description": "Get milk, eggs, bread",
  "completed": true,
  "createdAt": "2025-11-24T12:00:00.000Z",
  "updatedAt": "2025-11-24T12:05:00.000Z"
}
```

### Error Responses

**Invalid ID (400 Bad Request)**
```json
{
  "message": "Invalid task ID"
}
```

**Task Not Found (404 Not Found)**
```json
{
  "message": "Task not found"
}
```

### cURL Example
```bash
curl -X PATCH http://localhost:5000/api/tasks/507f1f77bcf86cd799439013
```

---

## 4. DELETE /tasks/:id

### Description
Permanently deletes a task from the database.

### Request
```http
DELETE /api/tasks/507f1f77bcf86cd799439013
```

### URL Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| id | String | Yes | MongoDB ObjectId of the task |

### Success Response (200 OK)
```json
{
  "message": "Task deleted successfully",
  "task": {
    "_id": "507f1f77bcf86cd799439013",
    "taskName": "Buy groceries",
    "description": "Get milk, eggs, bread",
    "completed": false
  }
}
```

### Error Responses

**Invalid ID (400 Bad Request)**
```json
{
  "message": "Invalid task ID"
}
```

**Task Not Found (404 Not Found)**
```json
{
  "message": "Task not found"
}
```

### cURL Example
```bash
curl -X DELETE http://localhost:5000/api/tasks/507f1f77bcf86cd799439013
```

---

## HTTP Status Codes

| Status Code | Meaning | When It's Used |
|-------------|---------|----------------|
| 200 OK | Success | GET, PATCH, DELETE successful |
| 201 Created | Resource created | POST successful |
| 400 Bad Request | Invalid input | Validation errors, invalid ID |
| 404 Not Found | Resource not found | Task doesn't exist |
| 500 Internal Server Error | Server error | Database errors, unexpected issues |

---

## Common Validation Rules

### Task Name
- Required: Yes
- Type: String
- Cannot be empty

### Description
- Required: Yes
- Type: String
- Cannot be empty

### Completed
- Required: No
- Type: Boolean
- Default: `false`

---

## JavaScript/Fetch Examples

### Get All Tasks
```javascript
async function getTasks() {
  const response = await fetch('/api/tasks');
  const tasks = await response.json();
  console.log(tasks);
}
```

### Create a Task
```javascript
async function createTask(taskName, description) {
  const response = await fetch('/api/tasks', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({ taskName, description })
  });
  const newTask = await response.json();
  console.log(newTask);
}
```

### Toggle Task Completion
```javascript
async function toggleTask(taskId) {
  const response = await fetch(`/api/tasks/${taskId}`, {
    method: 'PATCH'
  });
  const updatedTask = await response.json();
  console.log(updatedTask);
}
```

### Delete a Task
```javascript
async function deleteTask(taskId) {
  const response = await fetch(`/api/tasks/${taskId}`, {
    method: 'DELETE'
  });
  const result = await response.json();
  console.log(result);
}
```

---

## Error Handling Best Practices

### Always Check Response Status
```javascript
const response = await fetch('/api/tasks');

if (!response.ok) {
  const error = await response.json();
  console.error('Error:', error.message);
  return;
}

const tasks = await response.json();
```

### Handle Network Errors
```javascript
try {
  const response = await fetch('/api/tasks');
  const tasks = await response.json();
} catch (error) {
  console.error('Network error:', error);
}
```

---

## Testing Checklist

- [ ] Can retrieve all tasks (empty array if no tasks)
- [ ] Can create a task with valid data
- [ ] Cannot create a task without taskName
- [ ] Cannot create a task without description
- [ ] Can toggle task completion status
- [ ] Cannot update task with invalid ID
- [ ] Cannot update non-existent task
- [ ] Can delete a task
- [ ] Cannot delete task with invalid ID
- [ ] Cannot delete non-existent task

---

## Tips for Beginners

1. **Test with cURL first** - It's the quickest way to test your API
2. **Use Postman** - Great for saving and organizing requests
3. **Check server logs** - Always check the terminal where your server is running
4. **Validate IDs** - MongoDB IDs are 24 hex characters (0-9, a-f)
5. **Use try/catch** - Always wrap API calls in try/catch blocks
6. **Check network tab** - Use browser DevTools to debug frontend requests

---

## Database Schema

```javascript
Task {
  _id: ObjectId          // Auto-generated by MongoDB
  taskName: String       // Required
  description: String    // Required
  completed: Boolean     // Default: false
  createdAt: Date        // Auto-generated timestamp
  updatedAt: Date        // Auto-updated timestamp
  __v: Number            // Version key (MongoDB internal)
}
```
