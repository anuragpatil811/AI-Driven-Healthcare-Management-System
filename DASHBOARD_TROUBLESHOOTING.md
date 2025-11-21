# 🔧 Dashboard Not Showing - Troubleshooting Guide

## 🎯 Problem

The dashboard page shows empty/blank cards with no content visible.

---

## 🔍 Common Causes

### **1. Not Logged In**
- If you're not logged in, you'll be redirected to /login
- Check if you see the login page or dashboard

### **2. User Data Not Loaded**
- Dashboard needs user data from localStorage
- If user data is missing, components can't render properly

### **3. Frontend Not Connected to Backend**
- Backend API calls may be failing
- Check browser console for errors

### **4. CSS/Styling Issues**
- Elements might be there but invisible due to styling
- Cards might be rendering but with no content

---

## ✅ **Quick Fixes**

### **Fix 1: Clear Browser Cache and Login Again**

1. **Press `Ctrl + Shift + Delete`**
2. **Clear:**
   - ✅ Cached images and files
   - ✅ Cookies and site data
3. **Close browser completely**
4. **Reopen and go to:** `http://localhost:5173`
5. **Login with:** avdhut@gmail.com / Avdhut@09

---

### **Fix 2: Check Console for Errors**

1. **Press `F12`** (open Developer Tools)
2. **Go to "Console" tab**
3. **Look for red error messages**
4. **Common errors:**
   - `Cannot read property 'name' of null` → User not loaded
   - `Network error` → Backend not running
   - `404 Not Found` → API endpoint missing

---

### **Fix 3: Check if User is Logged In**

1. **Press `F12`** (Developer Tools)
2. **Go to "Console" tab**
3. **Type:**
   ```javascript
   localStorage.getItem('currentUser')
   ```
4. **Press Enter**
5. **Should show:** User data in JSON format
6. **If null:** You need to login again

---

### **Fix 4: Check Backend Connection**

1. **Open new tab:** `http://localhost:5000`
2. **Should see:** Backend API dashboard
3. **If not loading:** Backend server is down
4. **Start backend:**
   ```powershell
   cd backend
   npm start
   ```

---

## 🎯 **Specific Issue: Admin vs Patient Dashboard**

Looking at your screenshot, you're at `localhost:5173/dashboard`

### **If you logged in as ADMIN:**
You should see **AdminDashboard** with:
- Stats cards (Total Users, Doctors, Labs, Pending Approvals)
- Three columns (Logged Users, Doctors, Labs)

### **If you logged in as PATIENT:**
You should see **Patient Dashboard** with:
- Stats cards (Appointments, Lab Tests, Health Score, Last Visit)
- Quick action cards (Book Appointment, Lab Tests, etc.)
- Nearby doctors section
- Today's appointments

### **If you see BLANK:**
- User data not loaded correctly
- Need to login again

---

## 🔧 **Debug Steps**

### **Step 1: Check What You See**

Open Developer Tools (`F12`) → Console → Type:

```javascript
// Check if user is logged in
const user = JSON.parse(localStorage.getItem('currentUser'))
console.log('User:', user)
console.log('Role:', user?.role)
```

**Expected Output:**
```javascript
User: { id: "...", name: "Avdhut", email: "avdhut@gmail.com", role: "admin", ... }
Role: "admin"
```

---

### **Step 2: Check Page Route**

In Console, type:
```javascript
console.log('Current path:', window.location.pathname)
```

**Should show:** `/dashboard`

---

### **Step 3: Force Re-render**

In Console, type:
```javascript
window.location.reload()
```

This will reload the page with fresh data.

---

## 🚀 **Proper Login Flow**

1. **Go to:** `http://localhost:5173`
2. **Should redirect to:** `/login`
3. **Enter credentials:**
   - Email: `avdhut@gmail.com`
   - Password: `Avdhut@09`
4. **Click Login**
5. **Should redirect to:** `/dashboard`
6. **Should see:** Admin Dashboard with stats

---

## 🎨 **What Admin Dashboard Should Look Like**

```
╔════════════════════════════════════════════╗
║  Admin Dashboard                           ║
║  Welcome back, Avdhut!                     ║
╠════════════════════════════════════════════╣
║                                            ║
║  ┌──────────┐ ┌──────────┐ ┌──────────┐  ║
║  │  Total   │ │ Doctors  │ │   Labs   │  ║
║  │  Users   │ │    0     │ │    0     │  ║
║  │    1     │ └──────────┘ └──────────┘  ║
║  └──────────┘ ┌──────────┐               ║
║               │ Pending  │               ║
║               │    0     │               ║
║               └──────────┘               ║
║                                            ║
║  ┌──────────┐ ┌──────────┐ ┌──────────┐  ║
║  │ Logged   │ │ Doctors  │ │  Labs    │  ║
║  │  Users   │ │ (Click)  │ │ (Click)  │  ║
║  │          │ │    →     │ │    →     │  ║
║  └──────────┘ └──────────┘ └──────────┘  ║
╚════════════════════════════════════════════╝
```

---

## 🐛 **Common Issues & Solutions**

### **Issue 1: Blank Page**
**Cause:** User not logged in  
**Fix:** Go to `/login` and login again

### **Issue 2: Only Header Visible**
**Cause:** API calls failing  
**Fix:** Check backend is running on port 5000

### **Issue 3: Cards Show But Empty**
**Cause:** No data in database  
**Fix:** Normal! You have no doctors/labs yet

### **Issue 4: Page Keeps Redirecting**
**Cause:** Token expired or invalid  
**Fix:** Clear localStorage and login again

---

## 💡 **Quick Test**

**Open Console (`F12`) and paste this:**

```javascript
// Complete diagnostic
console.log('=== DASHBOARD DIAGNOSTIC ===')
console.log('1. URL:', window.location.href)
console.log('2. User:', localStorage.getItem('currentUser'))
console.log('3. Is Authenticated:', !!localStorage.getItem('currentUser'))

const user = JSON.parse(localStorage.getItem('currentUser') || '{}')
console.log('4. User Role:', user.role)
console.log('5. User Name:', user.name)

if (user.role) {
  console.log('✅ User is logged in as:', user.role.toUpperCase())
} else {
  console.log('❌ User is NOT logged in')
  console.log('   → Go to /login')
}
```

---

## 🎯 **Most Likely Solution**

Based on your screenshot showing an empty dashboard:

**You need to:**
1. ✅ **Close all browser tabs**
2. ✅ **Open fresh:** `http://localhost:5173`
3. ✅ **It will redirect to:** `/login`
4. ✅ **Login with:** avdhut@gmail.com / Avdhut@09
5. ✅ **Dashboard should load** with admin stats!

---

## 📝 **If Still Broken**

Take a screenshot of:
1. **Browser Console** (F12 → Console tab) - show any errors
2. **Network Tab** (F12 → Network tab) - show failed requests
3. **LocalStorage** (F12 → Application → Local Storage)

This will help diagnose the exact issue!

---

**Try the "Complete Diagnostic" script in console first!** 🔍
