# 🔧 Quick Fix - Test Appointment System

## **Current Status:**
- ✅ Frontend is running on port 5174
- ✅ Dr. Sarah is logged in
- ⚠️ Dashboard shows 0 appointments
- ❓ Need to verify backend is running

## **Step 1: Check Backend Server**

Open a **NEW terminal** and run:

```powershell
cd backend
npm start
```

**Expected Output:**
```
Server running on port 5000
MongoDB Connected: ...
```

**If you see errors:**
- Make sure MongoDB is running
- Check if port 5000 is available

## **Step 2: Verify Backend is Working**

Open browser and go to:
```
http://localhost:5000/api/health
```

**Should see:**
```json
{
  "status": "OK",
  "message": "Healthcare API is running"
}
```

## **Step 3: Test Booking an Appointment**

### **Option A: Book via Frontend (Recommended)**

1. **In browser, logout** from Dr. Sarah
2. **Login as patient:**
   - Email: ritesh@gmail.com (or any patient account)
   - Password: (your patient password)
3. **Click "Book Appointment"** in navbar
4. **Select a location** (e.g., Pune, Mumbai, Delhi)
5. **Should see Dr. Sarah** in the doctor list
   - If you don't see her, she might not have location set
6. **Click her card** → Fill form:
   - Date: Tomorrow
   - Time: Any slot
   - Reason: "Test appointment"
7. **Click "Confirm Appointment"**
8. **Should see:** ✅ Success message

### **Option B: Create Test Appointment Directly**

If frontend booking doesn't work, you can create one via backend:

1. **Get Dr. Sarah's User ID from database:**
   - Open MongoDB Compass
   - Database: healthcare-db
   - Collection: users
   - Find: `{ email: "sarah12@gmail.com" }`
   - Copy her `_id`

2. **Get a patient's User ID:**
   - Find: `{ email: "ritesh@gmail.com" }` (or any patient)
   - Copy their `_id`

3. **Create appointment manually in MongoDB:**
   - Collection: appointments
   - Insert document:
   ```json
   {
     "patientId": "paste_patient_id_here",
     "doctorId": "paste_dr_sarah_user_id_here",
     "appointmentDate": "2025-10-21T00:00:00.000Z",
     "timeSlot": {
       "startTime": "10:00 AM",
       "endTime": "10:30 AM"
     },
     "reasonForVisit": "Test appointment",
     "symptoms": "Testing the system",
     "status": "scheduled",
     "createdAt": "2025-10-20T12:00:00.000Z",
     "updatedAt": "2025-10-20T12:00:00.000Z"
   }
   ```

## **Step 4: Verify Doctor Can See Appointment**

1. **Login as Dr. Sarah** (sarah12@gmail.com)
2. **Should automatically open Doctor Dashboard**
3. **Refresh the page** (Ctrl+R)
4. **Should now see:**
   - Total Appointments: 1 (or more)
   - The appointment card with patient details

## **Common Issues & Solutions:**

### **Issue 1: Backend Not Running**
**Error:** "Failed to load appointments"
**Solution:** 
```powershell
cd backend
npm start
```

### **Issue 2: Can't See Doctors in Booking Page**
**Possible Causes:**
- Doctors don't have location/city set
- Doctor is not approved

**Solution:**
1. Check database: users collection
2. Find Dr. Sarah's record
3. Make sure:
   - `isApproved: true`
   - `role: "doctor"`

**Or update via MongoDB:**
```javascript
db.users.updateOne(
  { email: "sarah12@gmail.com" },
  { 
    $set: { 
      isApproved: true,
      role: "doctor"
    }
  }
)
```

### **Issue 3: Doctor List Shows "Failed to load doctors"**
**Solution:** Backend is not running. Start it with `npm start` in backend folder.

### **Issue 4: Can't Book Appointment**
**Check in browser console (F12):**
- Look for red errors
- Check Network tab for failed requests
- Verify you're logged in as a patient

## **Debug Checklist:**

- [ ] Backend server is running (port 5000)
- [ ] Frontend is running (port 5173/5174)
- [ ] MongoDB is running
- [ ] Dr. Sarah is approved (isApproved: true)
- [ ] Patient account exists
- [ ] Patient is logged in when booking
- [ ] Browser console shows no errors

## **Quick Test Flow:**

```
1. Start backend → npm start (in backend folder)
2. Start frontend → Already running on 5174 ✅
3. Login as patient → ritesh@gmail.com
4. Book appointment → Select Dr. Sarah
5. Logout → Login as Dr. Sarah
6. Check dashboard → Should see appointment
```

## **Expected Result:**

**Doctor Dashboard Should Show:**
```
Total Appointments: 1
Today's Appointments: 0 (or 1 if today)
Upcoming: 1
Completed: 0

[Appointment Card]
Patient Name: Ritesh
Date: Tomorrow
Time: 10:00 AM
Reason: Test appointment
Status: Scheduled
```

## **If Still Not Working:**

1. Check browser console for errors (F12 → Console)
2. Check Network tab (F12 → Network) for failed API calls
3. Check backend terminal for error messages
4. Verify MongoDB has data in collections:
   - users
   - doctors
   - appointments

Let me know what you see and I'll help fix it! 🔧
