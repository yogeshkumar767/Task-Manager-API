# Quick Start Guide - 5 Minutes to Running App

This guide will get you up and running in 5 minutes.

## Step 1: Get MongoDB (2 minutes)

### Option A: MongoDB Atlas (Recommended - Free Cloud Database)
1. Go to https://www.mongodb.com/cloud/atlas
2. Click "Try Free" and sign up
3. Create a new cluster (select the FREE tier)
4. Wait for cluster to be created (1-2 minutes)
5. Click "Connect" → "Connect your application"
6. Copy the connection string
7. Replace `<password>` with your database password

### Option B: Already Have MongoDB?
Skip to Step 2!

## Step 2: Set Environment Variable (30 seconds)

In Replit (or your .env file), set:
```
MONGODB_URI=your_connection_string_here
```

Example:
```
MONGODB_URI=mongodb+srv://myuser:mypassword123@cluster0.abc.mongodb.net/taskmanager
```

## Step 3: Whitelist IP Address (1 minute)

In MongoDB Atlas:
1. Go to "Network Access" (left sidebar)
2. Click "Add IP Address"
3. Select "Allow Access from Anywhere" (adds `0.0.0.0/0`)
4. Click "Confirm"

## Step 4: Run the Server (10 seconds)

```bash
node server.js
```

You should see:
```
Connected to MongoDB
Server is running on port 5000
```

## Step 5: Open the App (10 seconds)

Open your browser and go to:
```
http://localhost:5000
```

You should see the Task Manager interface!

## That's It! 🎉

You now have a fully functional Task Manager app running.

---

## Quick Test

Try these actions to verify everything works:

1. **Create a Task**
   - Fill in "Task Name" (e.g., "Buy milk")
   - Fill in "Description" (e.g., "Get 2% milk from store")
   - Click "Add Task"
   - ✅ You should see the task appear in the list below

2. **Complete a Task**
   - Click "Mark Complete" on any task
   - ✅ You should see a celebration animation and checkmark
   - ✅ Task should change color to green
   - ✅ Text should have a strikethrough

3. **Delete a Task**
   - Click "Delete" on any task
   - Confirm the deletion
   - ✅ Task should slide out and disappear

---

## Troubleshooting

### "MongoDB connection error"
- ✅ Check your MONGODB_URI is set correctly
- ✅ Make sure you replaced `<password>` with your actual password
- ✅ Verify your IP is whitelisted (use 0.0.0.0/0 to allow all)

### "Port 5000 already in use"
- ✅ Stop any other programs using port 5000
- ✅ Or change PORT environment variable to 3000 or 8000

### "Cannot GET /"
- ✅ Make sure server.js is running
- ✅ Check for any error messages in terminal

---

## Next Steps

1. **Read the Full Documentation**
   - Open `README.md` for complete guide
   - Check `API_DOCUMENTATION.md` for API reference

2. **Test the API Directly**
   ```bash
   # Get all tasks
   curl http://localhost:5000/api/tasks
   
   # Create a task
   curl -X POST http://localhost:5000/api/tasks \
     -H "Content-Type: application/json" \
     -d '{"taskName": "Test", "description": "Testing API"}'
   ```

3. **Customize the Frontend**
   - Edit `public/index.html` to change colors, text, layout
   - Add new features like categories, due dates, priorities

4. **Add More Features**
   - User authentication
   - Task filtering and search
   - Export tasks to PDF/CSV
   - Email notifications

---

## Project Files

- `server.js` - Main server (connects database, starts server)
- `models/task.model.js` - Database schema
- `routes/task.routes.js` - API endpoints
- `public/index.html` - User interface
- `README.md` - Complete documentation
- `API_DOCUMENTATION.md` - API quick reference

---

## Need Help?

1. Check the error message in your terminal
2. Read the Error Handling section in README.md
3. Make sure all prerequisites are installed
4. Verify environment variables are set correctly

Happy coding! 🚀
