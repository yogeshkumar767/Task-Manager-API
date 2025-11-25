# MongoDB Setup Guide - Quick Start

## The Problem
Your server is trying to connect to MongoDB at `localhost:27017`, but MongoDB isn't installed/running on your computer.

## Solution: Use MongoDB Atlas (Free Cloud Database)

MongoDB Atlas is free and doesn't require installation!

---

## Step-by-Step Setup (5 minutes)

### Step 1: Create MongoDB Atlas Account
1. Go to https://www.mongodb.com/cloud/atlas/register
2. Sign up for a free account (no credit card required)
3. Choose the **FREE** tier (M0 Sandbox)

### Step 2: Create a Cluster
1. After signing up, click **"Build a Database"**
2. Choose **"M0 FREE"** tier
3. Select a cloud provider and region (choose closest to you)
4. Click **"Create"** (takes 1-3 minutes)

### Step 3: Create Database User
1. Go to **"Database Access"** (left sidebar)
2. Click **"Add New Database User"**
3. Choose **"Password"** authentication
4. Enter a username (e.g., `taskflowuser`)
5. Enter a password (save this - you'll need it!)
6. Click **"Add User"**

### Step 4: Whitelist Your IP
1. Go to **"Network Access"** (left sidebar)
2. Click **"Add IP Address"**
3. Click **"Allow Access from Anywhere"** (adds `0.0.0.0/0`)
   - Or add your specific IP for better security
4. Click **"Confirm"**

### Step 5: Get Connection String
1. Go to **"Database"** (left sidebar)
2. Click **"Connect"** on your cluster
3. Choose **"Connect your application"**
4. Copy the connection string (looks like):
   ```
   mongodb+srv://<username>:<password>@cluster0.xxxxx.mongodb.net/?retryWrites=true&w=majority
   ```
5. Replace `<username>` with your database username
6. Replace `<password>` with your database password
7. Add database name at the end: `/taskmanager` (before the `?`)
   
   **Final format:**
   ```
   mongodb+srv://taskflowuser:yourpassword@cluster0.xxxxx.mongodb.net/taskmanager?retryWrites=true&w=majority
   ```

### Step 6: Set Environment Variable
Create a `.env` file in your project root:

**Windows (PowerShell):**
```powershell
$env:MONGODB_URI="mongodb+srv://username:password@cluster0.xxxxx.mongodb.net/taskmanager?retryWrites=true&w=majority"
```

**Or create `.env` file:**
```
MONGODB_URI=mongodb+srv://username:password@cluster0.xxxxx.mongodb.net/taskmanager?retryWrites=true&w=majority
PORT=5000
```

---

## Alternative: Quick Test Without MongoDB

If you just want to test the frontend quickly, I can modify the server to work without MongoDB temporarily. But for full functionality, you need MongoDB.

---

## After Setup

1. Restart your server: `npm start`
2. You should see: `Connected to MongoDB`
3. Open Chrome: `http://localhost:5000`

---

## Need Help?

If you get stuck, let me know and I can help you through any step!

