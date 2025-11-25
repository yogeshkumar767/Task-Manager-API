# Quick Start - Fix MongoDB Connection Error

## 🚨 Current Error
```
MongoDB connection error: connect ECONNREFUSED 127.0.0.1:27017
```
This means MongoDB isn't running on your computer.

---

## ✅ Solution: Use MongoDB Atlas (Free Cloud Database)

### Option 1: Quick Setup (Recommended)

**Step 1:** Get MongoDB Atlas connection string
1. Go to: https://www.mongodb.com/cloud/atlas/register
2. Sign up (free, no credit card)
3. Create a free cluster (M0)
4. Create database user (username + password)
5. Whitelist IP: Add `0.0.0.0/0` (allow from anywhere)
6. Get connection string from "Connect" → "Connect your application"
7. It looks like: `mongodb+srv://username:password@cluster0.xxxxx.mongodb.net/taskmanager?retryWrites=true&w=majority`

**Step 2:** Set environment variable in PowerShell
```powershell
$env:MONGODB_URI="mongodb+srv://username:password@cluster0.xxxxx.mongodb.net/taskmanager?retryWrites=true&w=majority"
```

**Step 3:** Start server
```powershell
npm start
```

**Step 4:** Open in Chrome
```powershell
start chrome http://localhost:5000
```

---

### Option 2: Create .env File (Permanent)

1. Create a file named `.env` in your project root
2. Add this line (replace with your MongoDB Atlas connection string):
```
MONGODB_URI=mongodb+srv://username:password@cluster0.xxxxx.mongodb.net/taskmanager?retryWrites=true&w=majority
PORT=5000
```

3. Start server: `npm start`
4. Open Chrome: `start chrome http://localhost:5000`

---

## 📝 Detailed MongoDB Atlas Setup

See `MONGODB_SETUP.md` for step-by-step instructions with screenshots.

---

## 🎯 After Setup

Once MongoDB is connected, you'll see:
```
Connected to MongoDB
Server is running on port 5000
```

Then open: **http://localhost:5000** in Chrome!

---

## ⚡ Quick Test (Without MongoDB)

If you just want to see the frontend quickly, I can modify the code to work without a database temporarily. But for full functionality, you need MongoDB Atlas.

