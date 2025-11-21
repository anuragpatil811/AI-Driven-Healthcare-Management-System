# 👨‍💼 ADMIN PANEL - USER GUIDE

## ✅ What the Admin Can See and Do

### **🎯 Admin Dashboard Features**

The admin has a **completely different dashboard** from patients. Here's what you see:

---

## 📊 **Dashboard Statistics (Top Cards)**

### 1. **Total Users** (Blue Card)
- Shows count of ALL registered users
- Includes patients, doctors, and labs

### 2. **Doctors** (Green Card)
- Shows total number of registered doctors
- Both approved and pending doctors

### 3. **Laboratories** (Purple Card with Beaker Icon)
- Shows total number of registered labs
- Both approved and pending labs

### 4. **Pending Approvals** (Orange Card)
- Shows how many doctors/labs are waiting for approval
- **This is your action item!**

---

## 👥 **Pending Approvals Section**

This is the **main admin function** - approving doctors and laboratories.

### **How It Works:**

1. **Someone registers as Doctor or Lab**
   - They fill the registration form
   - They choose role: "Doctor" or "Lab"
   - Their account is created but **NOT APPROVED** yet
   - They **CANNOT login** until you approve them

2. **You see them in "Pending Approvals"**
   - Name, Email, Phone shown
   - Registration date shown
   - Role badge (Doctor or Lab)

3. **You verify if they are genuine**
   - Check their name
   - Check their email
   - Verify they are real doctors/labs

4. **You take action:**
   - ✅ **Click "Approve"** → They can now login and access system
   - ❌ **Click "Reject"** → Their registration is deleted

---

## 🔒 **Admin-Only Features**

### **What Admin SEES:**
- ✅ Dashboard with statistics
- ✅ List of all pending approvals
- ✅ Approve/Reject buttons
- ✅ User management

### **What Admin DOES NOT SEE:**
- ❌ Book Appointment (patient feature)
- ❌ My Appointments (patient feature)
- ❌ Lab Tests (patient feature)
- ❌ BMI Calculator (patient feature)
- ❌ AI Assistant (patient feature)

**Admin Navbar shows ONLY:** Dashboard

---

## 👤 **User Role Differences**

### **Admin** (Red badge - YOU)
```
Role: ADMIN
Access: Approve doctors/labs, view stats
Cannot: Book appointments, tests
```

### **Doctor** (Green badge)
```
Role: DOCTOR
Needs: Admin approval ✅
Can: View their appointments, add diagnosis
Cannot: Access admin panel
```

### **Lab** (Purple badge)
```
Role: LAB
Needs: Admin approval ✅
Can: View test bookings, upload results
Cannot: Access admin panel
```

### **Patient** (Blue badge)
```
Role: PATIENT
Needs: No approval (auto-approved)
Can: Book appointments, tests, use BMI calculator
Cannot: Access admin panel
```

---

## 🧪 **Testing the Approval Workflow**

### **Step-by-Step Test:**

1. **Open new browser window** (Incognito/Private mode)
   ```
   http://localhost:5173/register
   ```

2. **Register as Doctor:**
   ```
   Name: Dr. Test Doctor
   Email: doctor@test.com
   Password: Test@123
   Phone: +1234567890
   Role: Doctor ← IMPORTANT!
   ```

3. **Try to login as that doctor**
   - You will see: "Your account is pending approval"
   - Login will FAIL ❌

4. **Go back to your Admin dashboard**
   - You'll see "Dr. Test Doctor" in Pending Approvals
   - Email: doctor@test.com
   - Role: DOCTOR

5. **Click "Approve" button**
   - Success message appears
   - Doctor removed from pending list
   - Total Doctors count increases

6. **Now login as that doctor**
   - Email: doctor@test.com
   - Password: Test@123
   - Login SUCCESS ✅
   - They see Doctor Dashboard

---

## 🎨 **Visual Identification**

### **In Navbar (Top Right):**
```
┌─────────────────┐
│ 👤 Avdhut      │
│    ADMIN       │ ← Red text
└─────────────────┘
```

### **Dashboard Title:**
```
Admin Dashboard
Welcome back, Avdhut! Manage your healthcare system.
```

---

## 📋 **Quick Reference**

### **Your Login:**
```
Email: avdhut@gmail.com
Password: Avdhut@09
Role: Admin
```

### **What You Control:**
- ✅ Who can be a doctor in the system
- ✅ Who can be a lab in the system
- ✅ View all system statistics
- ✅ Monitor user activity

### **What You Don't Control:**
- ❌ Patient appointments (they book themselves)
- ❌ Lab tests (they book themselves)
- ⚠️ Patients are **auto-approved** (no admin approval needed)

---

## 🚀 **Common Admin Tasks**

### **Task 1: Approve a New Doctor**
1. Login as admin
2. Check "Pending Approvals" section
3. Verify doctor's information
4. Click "Approve" if genuine
5. Doctor can now login

### **Task 2: Reject Fake Registration**
1. Login as admin
2. Check "Pending Approvals" section
3. See suspicious registration
4. Click "Reject"
5. User is removed from system

### **Task 3: Check System Statistics**
1. Login as admin
2. View dashboard cards at top
3. See total users, doctors, labs
4. Monitor pending approvals count

---

## ⚠️ **Important Notes**

1. **Only Doctors and Labs need approval**
   - Patients are auto-approved when they register

2. **Admin cannot approve patients**
   - Patients don't appear in pending approvals

3. **One admin per system**
   - Your account: avdhut@gmail.com
   - Cannot register more admins via UI

4. **Admin sees clean dashboard**
   - No patient features cluttering the view
   - Focus on management only

---

**You are now the system administrator!** 🎯

Your job: Keep only genuine doctors and labs in the system by approving legitimate registrations.

