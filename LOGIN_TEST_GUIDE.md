# 🧪 LOGIN TEST GUIDE

**Date:** October 19, 2025  
**Backend:** ✅ Running on http://localhost:5000  
**Frontend:** ✅ Running on http://localhost:5174

---

## 🎯 TEST SCENARIOS

### **Scenario 1: Admin Login** ✅

**Credentials:**
```
Email: avdhut@gmail.com
Password: Avdhut@09
```

**Steps:**
1. Open: http://localhost:5174
2. Click "Login" button
3. Enter email: `avdhut@gmail.com`
4. Enter password: `Avdhut@09`
5. Click "Sign In"

**Expected Results:**
- ✅ Login successful
- ✅ Redirected to Dashboard
- ✅ See "ADMIN" badge in red
- ✅ See 3 columns: Logged Users, Pending Doctors, Pending Labs
- ✅ Only "Dashboard" in navigation menu
- ✅ JWT token stored in localStorage

**What to Check:**
- [ ] No errors in console
- [ ] Name shows: "Avdhut"
- [ ] Role badge shows: "ADMIN" in red
- [ ] Can see admin dashboard
- [ ] Can logout successfully

---

### **Scenario 2: Patient Registration & Login** 🆕

**Steps to Register:**
1. Go to: http://localhost:5174
2. Click "Register" link
3. Fill form:
   - Name: `Test Patient`
   - Email: `patient@test.com`
   - Password: `Test@123`
   - Phone: `+91 9876543210`
   - Role: Select **"Patient"**
4. Click "Sign Up"

**Expected Results:**
- ✅ Registration successful
- ✅ Message: "Registration successful"
- ✅ Auto-approved (isApproved: true)
- ✅ Can login immediately
- ✅ JWT token received

**Steps to Login:**
1. Logout if logged in
2. Click "Login"
3. Enter:
   - Email: `patient@test.com`
   - Password: `Test@123`
4. Click "Sign In"

**Expected Results:**
- ✅ Login successful
- ✅ See "PATIENT" badge
- ✅ Can see patient menu: Appointments, Lab Tests, etc.
- ✅ Can book appointments
- ✅ Can book lab tests

---

### **Scenario 3: Doctor Registration (Needs Approval)** 🩺

**Steps to Register:**
1. Go to: http://localhost:5174
2. Click "Register"
3. Fill form:
   - Name: `Dr. Test Doctor`
   - Email: `doctor@test.com`
   - Password: `Test@123`
   - Phone: `+91 9876543211`
   - Role: Select **"Doctor"**
   - Specialty: `Cardiology`
   - Qualification: `MBBS, MD`
   - Experience: `5` years
   - Consultation Fee: `500`
4. Click "Sign Up"

**Expected Results:**
- ✅ Registration successful
- ✅ Message: "Registration successful. Your account is pending admin approval."
- ✅ NOT auto-approved (isApproved: false)
- ✅ JWT token received

**Steps to Test Pending Status:**
1. Logout
2. Try to login:
   - Email: `doctor@test.com`
   - Password: `Test@123`
3. Login should work BUT...

**Expected Results:**
- ✅ Can login
- ❌ Cannot access doctor features
- ⏳ Shows "Pending approval" status
- ⏳ Appears in Admin's "Pending Doctors" list

**Steps to Approve:**
1. Logout from doctor
2. Login as Admin (`avdhut@gmail.com`)
3. Go to Dashboard
4. Look at "Pending Doctors" column
5. Find "Dr. Test Doctor"
6. Click ✅ **Approve** button

**Expected Results:**
- ✅ Doctor approved
- ✅ Doctor can now access all features
- ✅ Doctor moved from "Pending" to "Logged Users"

---

### **Scenario 4: Lab Registration (Needs Approval)** 🧪

**Steps to Register:**
1. Go to: http://localhost:5174
2. Click "Register"
3. Fill form:
   - Name: `City Test Lab`
   - Email: `lab@test.com`
   - Password: `Test@123`
   - Phone: `+91 9876543212`
   - Role: Select **"Laboratory"**
   - Address: `123 Main Street`
   - City: `Mumbai`
   - State: `Maharashtra`
   - Services: `Blood Test, X-Ray, MRI`
4. Click "Sign Up"

**Expected Results:**
- ✅ Registration successful
- ✅ Message: "Your account is pending admin approval."
- ✅ NOT auto-approved (isApproved: false)

**Steps to Approve:**
1. Login as Admin
2. Go to Dashboard
3. Look at "Pending Labs" column
4. Find "City Test Lab"
5. Click ✅ **Approve** button

**Expected Results:**
- ✅ Lab approved
- ✅ Lab can now access all features

---

## 🔐 AUTHENTICATION FEATURES TO TEST

### **1. Password Encryption** ✅

**Test Method:**
```powershell
# Check database
cd backend
node -e "const mongoose = require('mongoose'); mongoose.connect('mongodb://localhost:27017/healthcare-db').then(async () => { const user = await mongoose.connection.db.collection('users').findOne({email:'avdhut@gmail.com'}); console.log('Password in DB:', user.password); process.exit(); });"
```

**Expected:**
```
Password in DB: $2a$10$SOqrOrl4T0AUB... (60 chars, encrypted)
```

---

### **2. JWT Token (7-day validity)** ✅

**Test Method:**
1. Login as any user
2. Open Browser DevTools (F12)
3. Go to: Console
4. Type:
```javascript
localStorage.getItem('token')
```

**Expected:**
```
"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
```

**Decode Token:**
```javascript
// Copy token and paste below
const token = "your_token_here";
const payload = JSON.parse(atob(token.split('.')[1]));
console.log('Issued:', new Date(payload.iat * 1000));
console.log('Expires:', new Date(payload.exp * 1000));
console.log('Valid for:', (payload.exp - payload.iat) / 86400, 'days');
```

**Expected:**
```
Valid for: 7 days
```

---

### **3. Role-Based Access** ✅

**Test Cases:**

**A. Admin Access:**
```
Login as: avdhut@gmail.com
Can see: Admin Dashboard ✅
Can see: Pending Approvals ✅
Can see: All Users ✅
Cannot see: Patient features ❌
```

**B. Patient Access:**
```
Login as: patient@test.com
Can see: Appointments ✅
Can see: Lab Tests ✅
Can see: BMI Calculator ✅
Cannot see: Admin panel ❌
```

**C. Doctor Access (after approval):**
```
Login as: doctor@test.com
Can see: Doctor Dashboard ✅
Can see: My Appointments ✅
Cannot see: Admin panel ❌
Cannot see: Patient features ❌
```

**D. Lab Access (after approval):**
```
Login as: lab@test.com
Can see: Lab Dashboard ✅
Can see: My Bookings ✅
Cannot see: Admin panel ❌
```

---

### **4. Auto-Approval Logic** ✅

**Test Results:**

| User Type | Auto-Approved? | Can Login Immediately? | Needs Admin Approval? |
|-----------|---------------|------------------------|----------------------|
| Admin     | ✅ YES        | ✅ YES                 | ❌ NO                |
| Patient   | ✅ YES        | ✅ YES                 | ❌ NO                |
| Doctor    | ❌ NO         | ⏳ Limited Access      | ✅ YES               |
| Lab       | ❌ NO         | ⏳ Limited Access      | ✅ YES               |

---

## 🧪 QUICK TEST CHECKLIST

### **Basic Login Tests:**
- [ ] Admin can login with correct credentials
- [ ] Admin cannot login with wrong password
- [ ] Error message shown for invalid credentials
- [ ] Token stored in localStorage after login
- [ ] User redirected to dashboard after login
- [ ] User can logout successfully
- [ ] Token removed from localStorage after logout

### **Registration Tests:**
- [ ] Patient can register and login immediately
- [ ] Doctor can register but needs approval
- [ ] Lab can register but needs approval
- [ ] Error shown if email already exists
- [ ] Password must be at least 6 characters
- [ ] Email validation works

### **Approval Tests:**
- [ ] Admin can see pending doctors
- [ ] Admin can see pending labs
- [ ] Admin can approve doctors
- [ ] Admin can approve labs
- [ ] Admin can reject doctors/labs
- [ ] Approved users can access all features

### **Security Tests:**
- [ ] Password not visible in API response
- [ ] Password encrypted in database
- [ ] Token required for protected routes
- [ ] Invalid token rejected
- [ ] Expired token rejected (after 7 days)
- [ ] Wrong role cannot access restricted routes

---

## 🎯 STEP-BY-STEP TEST PROCEDURE

### **Step 1: Test Admin Login**

1. **Open Browser:** http://localhost:5174
2. **Click:** "Login" button
3. **Enter:**
   - Email: `avdhut@gmail.com`
   - Password: `Avdhut@09`
4. **Click:** "Sign In"
5. **Verify:**
   - ✅ Login successful
   - ✅ Shows "ADMIN" badge
   - ✅ See admin dashboard with 3 columns

---

### **Step 2: Test Patient Registration**

1. **Logout** from admin
2. **Click:** "Register"
3. **Fill form:**
   - Name: `Test Patient`
   - Email: `patient@test.com`
   - Password: `Test@123`
   - Phone: `+91 9876543210`
   - Role: Patient
4. **Click:** "Sign Up"
5. **Verify:**
   - ✅ Success message shown
   - ✅ Redirected to login or dashboard
6. **Login** with patient credentials
7. **Verify:**
   - ✅ Can see patient features
   - ✅ Shows "PATIENT" badge

---

### **Step 3: Test Doctor Approval Workflow**

1. **Logout**
2. **Register as Doctor:**
   - Email: `doctor@test.com`
   - Role: Doctor
3. **Verify:**
   - ✅ Shows "Pending approval" message
4. **Logout**
5. **Login as Admin**
6. **Go to Dashboard**
7. **Verify:**
   - ✅ Doctor appears in "Pending Doctors"
8. **Click:** ✅ Approve button
9. **Verify:**
   - ✅ Doctor approved
   - ✅ Doctor moved to "Logged Users"
10. **Logout**
11. **Login as Doctor**
12. **Verify:**
    - ✅ Can access doctor features
    - ✅ Shows "DOCTOR" badge

---

### **Step 4: Test JWT Token**

1. **Login** as any user
2. **Press F12** (DevTools)
3. **Console tab**
4. **Type:**
```javascript
localStorage.getItem('token')
```
5. **Verify:**
   - ✅ Token exists
   - ✅ Starts with "eyJhbGci..."

---

### **Step 5: Test Password Encryption**

1. **Open new terminal**
2. **Run:**
```powershell
cd backend
node viewDatabase.js
```
3. **Verify:**
   - ✅ All passwords start with `$2a$10$`
   - ✅ Passwords are 60 characters long
   - ✅ No plain text passwords

---

## 📱 BROWSER TEST INTERFACE

**Now Open Your Browser:**

🌐 **Frontend URL:** http://localhost:5174

**You should see:**
- Healthcare Management System homepage
- Login button in top right
- Register link
- Clean, modern interface

**First Test:**
1. Click "Login"
2. Enter admin credentials
3. Watch the magic happen! ✨

---

## 🔍 DEBUGGING TIPS

### **If login fails:**
1. Check backend is running: http://localhost:5000/api/health
2. Check console for errors (F12)
3. Check Network tab for API calls
4. Verify credentials are correct

### **If token not saved:**
1. Check localStorage in DevTools
2. Check console for errors
3. Try clearing browser cache

### **If approval not working:**
1. Refresh admin dashboard
2. Check browser console
3. Verify backend logs

---

## ✅ SUCCESS CRITERIA

**Login Test Passes If:**
- ✅ Admin can login
- ✅ Password encrypted in database
- ✅ JWT token generated (7-day expiry)
- ✅ Role-based access working
- ✅ Auto-approval working (Patient/Admin)
- ✅ Manual approval working (Doctor/Lab)
- ✅ Token verified on protected routes
- ✅ Logout working correctly

---

**Ready to test! Open http://localhost:5174 in your browser!** 🚀

