# 🎯 COMPLETE TESTING GUIDE

## Method 1: Browser Testing Dashboard ⭐ EASIEST

### Step 1: Open Browser
```
http://localhost:5000
```

### Step 2: Click Buttons to Test
- **Test Health Check** → Verifies server is running
- **Test Admin Login** → Gets JWT token (watch the response!)
- **Get Doctors List** → Shows all doctors

### Step 3: Observe Responses
The dashboard shows live JSON responses at the bottom!

---

## Method 2: Using Postman 🚀 RECOMMENDED

### Setup:
1. Download: https://www.postman.com/downloads/
2. Open Postman
3. Create new Collection: "Healthcare API"

### Test 1: Health Check
```
Method: GET
URL: http://localhost:5000/api/health
```
Click "Send" → Should see `{"status":"OK"}`

### Test 2: Admin Login
```
Method: POST
URL: http://localhost:5000/api/auth/login
Headers: Content-Type: application/json
Body (raw JSON):
{
  "email": "admin@healthcare.com",
  "password": "Admin@123"
}
```
Click "Send" → Copy the `token` from response!

### Test 3: Get Dashboard Stats (Protected Route)
```
Method: GET
URL: http://localhost:5000/api/admin/stats
Headers:
  - Authorization: Bearer YOUR_TOKEN_HERE
  (Replace YOUR_TOKEN_HERE with token from Test 2)
```
Click "Send" → Should see statistics!

### Test 4: Register a Patient
```
Method: POST
URL: http://localhost:5000/api/auth/register
Body (raw JSON):
{
  "name": "John Doe",
  "email": "john@example.com",
  "password": "password123",
  "phone": "+1234567890",
  "role": "patient",
  "gender": "male",
  "dateOfBirth": "1990-01-01"
}
```

### Test 5: Register a Doctor (Will Need Approval)
```
Method: POST
URL: http://localhost:5000/api/auth/register
Body (raw JSON):
{
  "name": "Dr. Sarah Johnson",
  "email": "sarah@hospital.com",
  "password": "doctor123",
  "phone": "+1234567891",
  "role": "doctor",
  "doctorData": {
    "specialty": "Cardiologist",
    "experience": 10,
    "licenseNumber": "MD123456",
    "consultationFee": 500,
    "hospitalAffiliation": {
      "name": "City Hospital",
      "address": "123 Medical St",
      "city": "Hadapsar"
    },
    "availableDays": ["Monday", "Tuesday", "Wednesday", "Thursday", "Friday"],
    "availableTimeSlots": [
      {
        "day": "Monday",
        "startTime": "09:00",
        "endTime": "17:00"
      }
    ]
  }
}
```

### Test 6: View Pending Approvals (Admin Only)
```
Method: GET
URL: http://localhost:5000/api/admin/pending-approvals
Headers:
  - Authorization: Bearer YOUR_ADMIN_TOKEN
```
Should see the doctor waiting for approval!

### Test 7: Approve the Doctor
```
Method: PUT
URL: http://localhost:5000/api/admin/approve/DOCTOR_USER_ID
(Replace DOCTOR_USER_ID with the _id from Test 6 response)
Headers:
  - Authorization: Bearer YOUR_ADMIN_TOKEN
```

### Test 8: Doctor Can Now Login
```
Method: POST
URL: http://localhost:5000/api/auth/login
Body:
{
  "email": "sarah@hospital.com",
  "password": "doctor123"
}
```
Should work! isApproved will be true.

---

## Method 3: PowerShell Commands 💻

### Test Health Check
```powershell
curl http://localhost:5000/api/health
```

### Test Login
```powershell
$body = @{
    email = "admin@healthcare.com"
    password = "Admin@123"
} | ConvertTo-Json

$response = Invoke-RestMethod -Method Post -Uri "http://localhost:5000/api/auth/login" -Body $body -ContentType "application/json"

# View response
$response

# Save token for later use
$token = $response.token
Write-Host "Token: $token"
```

### Test Protected Endpoint
```powershell
$headers = @{
    "Authorization" = "Bearer $token"
}

Invoke-RestMethod -Method Get -Uri "http://localhost:5000/api/admin/stats" -Headers $headers
```

---

## Method 4: Using the Frontend 🎨

### Step 1: Install axios (if not done)
```powershell
npm install axios
```

### Step 2: Use the API helper
```javascript
// In your React component
import { authAPI, doctorAPI, appointmentAPI } from './utils/api';

// Login
const handleLogin = async () => {
  try {
    const response = await authAPI.login({
      email: 'admin@healthcare.com',
      password: 'Admin@123'
    });
    
    console.log('Success!', response.data);
    localStorage.setItem('token', response.data.token);
  } catch (error) {
    console.error('Error:', error.response?.data);
  }
};

// Get doctors
const fetchDoctors = async () => {
  const response = await doctorAPI.getAll({ city: 'Hadapsar' });
  console.log('Doctors:', response.data.data);
};
```

---

## 🧪 Complete Test Scenario

### Scenario: Admin approves doctor, patient books appointment

**Step 1:** Admin logs in
```
POST /api/auth/login
→ Get admin token
```

**Step 2:** Doctor registers
```
POST /api/auth/register (with doctorData)
→ Doctor created but isApproved = false
```

**Step 3:** Admin sees pending approval
```
GET /api/admin/pending-approvals (with admin token)
→ See doctor in list
```

**Step 4:** Admin approves doctor
```
PUT /api/admin/approve/:doctorId (with admin token)
→ Doctor now approved
```

**Step 5:** Patient registers
```
POST /api/auth/register (role: patient)
→ Patient auto-approved
```

**Step 6:** Patient logs in
```
POST /api/auth/login
→ Get patient token
```

**Step 7:** Patient views doctors
```
GET /api/doctors
→ See approved doctor in list
```

**Step 8:** Patient books appointment
```
POST /api/appointments (with patient token)
{
  "doctorId": "...",
  "appointmentDate": "2025-10-20",
  "timeSlot": { "startTime": "10:00", "endTime": "10:30" },
  "reasonForVisit": "General checkup"
}
→ Appointment created
```

**Step 9:** Doctor logs in and sees appointment
```
POST /api/auth/login (doctor credentials)
GET /api/appointments/doctor-appointments (with doctor token)
→ See patient's appointment
```

**Step 10:** Doctor completes appointment
```
PUT /api/appointments/:id/complete (with doctor token)
{
  "diagnosis": "Healthy",
  "prescription": [...],
  "vitalSigns": {...}
}
→ Appointment marked complete
```

✅ **COMPLETE FLOW TESTED!**

---

## 📊 What Each Response Means

### Success Response (200-299)
```json
{
  "success": true,
  "message": "Operation successful",
  "data": { ... }
}
```
✅ Everything worked!

### Validation Error (400)
```json
{
  "success": false,
  "message": "Validation failed",
  "errors": [
    { "field": "email", "message": "Invalid email" }
  ]
}
```
❌ Check your input data

### Unauthorized (401)
```json
{
  "success": false,
  "message": "Not authorized. Please login."
}
```
❌ Need to login or token expired

### Forbidden (403)
```json
{
  "success": false,
  "message": "Your account is pending approval"
}
```
❌ Don't have permission

### Not Found (404)
```json
{
  "success": false,
  "message": "Resource not found"
}
```
❌ Wrong URL or ID

### Server Error (500)
```json
{
  "success": false,
  "message": "Internal Server Error"
}
```
❌ Something wrong on server (check server logs)

---

## 🎯 Quick Reference

| What to Test | Endpoint | Method | Needs Auth? |
|-------------|----------|--------|------------|
| Health check | `/api/health` | GET | No |
| Login | `/api/auth/login` | POST | No |
| Register | `/api/auth/register` | POST | No |
| Get profile | `/api/auth/me` | GET | Yes |
| Get doctors | `/api/doctors` | GET | No |
| Dashboard stats | `/api/admin/stats` | GET | Yes (Admin) |
| Pending approvals | `/api/admin/pending-approvals` | GET | Yes (Admin) |
| Approve user | `/api/admin/approve/:id` | PUT | Yes (Admin) |
| Book appointment | `/api/appointments` | POST | Yes (Patient) |
| My appointments | `/api/appointments/my-appointments` | GET | Yes (Patient) |

---

## 💡 Pro Tips

1. **Save your Postman collection** - You can reuse tests
2. **Use environment variables in Postman** - Store base URL and tokens
3. **Check server logs** - They show what's happening
4. **Use browser DevTools** - Network tab shows requests/responses
5. **Test error cases** - Try wrong passwords, missing fields, etc.

---

Ready to test? Start with Method 1 (Browser Dashboard) - it's the easiest! 🚀
