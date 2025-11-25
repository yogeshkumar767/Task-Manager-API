# How to Access Your TaskFlow API Project

## 🏠 **Current Status: Local Development**

Your project is currently running **locally on your computer** (not hosted online yet).

---

## 📍 **Where to Access Your Project**

### **1. Web Interface (Frontend)**
Once the server is running, open your browser and go to:
```
http://localhost:5000
```
This will show the Task Manager web interface where you can:
- Add tasks
- View all tasks
- Mark tasks as complete
- Delete tasks

### **2. API Endpoints (Backend)**
Your API is accessible at:
```
http://localhost:5000/api
```

**Available endpoints:**
- `GET http://localhost:5000/api/tasks` - Get all tasks
- `POST http://localhost:5000/api/tasks` - Create a task
- `PATCH http://localhost:5000/api/tasks/:id` - Toggle task completion
- `DELETE http://localhost:5000/api/tasks/:id` - Delete a task

---

## 🚀 **How to Start Your Server**

### **Option 1: Using npm start**
```bash
npm start
```

### **Option 2: Direct node command**
```bash
node server.js
```

### **What You'll See:**
```
Connected to MongoDB
Server is running on port 5000
```

---

## ✅ **Check if Server is Running**

### **Method 1: Check Terminal**
Look for the message: `Server is running on port 5000`

### **Method 2: Open Browser**
Go to `http://localhost:5000` - if you see the Task Manager interface, it's running!

### **Method 3: Test API**
Open a new terminal and run:
```bash
curl http://localhost:5000/api/tasks
```
Or use Postman/Thunder Client to test the API endpoints.

---

## 🌐 **To Host Your Project Online (Deployment)**

Currently, your project is **only accessible on your local computer**. To make it accessible online, you need to deploy it. Here are popular options:

### **Free Hosting Options:**

1. **Replit** (if you're using Replit)
   - Your project is already set up for Replit
   - Just run it and it will be accessible via Replit's URL

2. **Render** (Free tier available)
   - Sign up at https://render.com
   - Connect your GitHub repository
   - Deploy as a Web Service

3. **Railway** (Free tier available)
   - Sign up at https://railway.app
   - Connect your repository
   - Auto-deploys on push

4. **Heroku** (Paid, but has free alternatives)
   - More complex setup
   - Requires credit card for verification

### **What You'll Need for Deployment:**
- MongoDB Atlas account (free cloud database)
- Environment variables set up (MONGODB_URI, PORT)
- Your code pushed to GitHub (recommended)

---

## 📝 **Quick Reference**

| What | Where | How to Access |
|------|-------|---------------|
| **Web Interface** | `http://localhost:5000` | Open in browser |
| **API Base URL** | `http://localhost:5000/api` | Use in Postman/Thunder Client |
| **Server File** | `server.js` | Run with `node server.js` |
| **Frontend** | `public/index.html` | Served automatically at `/` |
| **Test Output** | Terminal | Run `npm test` |
| **Coverage Report** | `coverage/lcov-report/index.html` | Open in browser |

---

## 🔍 **Troubleshooting**

### **"Cannot GET /" or "Connection Refused"**
- **Solution**: Make sure the server is running (`npm start`)

### **"MongoDB connection error"**
- **Solution**: 
  - Make sure MongoDB is running (if using local MongoDB)
  - Or set up MongoDB Atlas and update MONGODB_URI

### **"Port 5000 already in use"**
- **Solution**: Change PORT in `.env` file or set environment variable:
  ```bash
  PORT=3000 node server.js
  ```

---

## 🎯 **Summary**

- **Local Access**: `http://localhost:5000` (only on your computer)
- **To Start**: Run `npm start` or `node server.js`
- **To Test**: Run `npm test`
- **To Deploy Online**: Use Render, Railway, or Replit

Your project is working perfectly locally! To share it with others or access it from anywhere, you'll need to deploy it to a cloud hosting service.

