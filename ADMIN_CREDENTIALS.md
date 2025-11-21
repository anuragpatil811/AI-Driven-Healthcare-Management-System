# 🔐 ADMIN CREDENTIALS

## ✅ Updated Admin Login

Your admin account has been successfully created and configured!

### **Login Credentials:**

```
Email:    avdhut@gmail.com
Password: Avdhut@09
Role:     Admin
```

---

## 🚀 How to Login

1. **Open your browser** and go to:
   ```
   http://localhost:5173/login
   ```

2. **Enter your credentials:**
   - Email: `avdhut@gmail.com`
   - Password: `Avdhut@09`

3. **Click "Sign In"**

4. **You will be redirected to the Admin Dashboard** where you can:
   - ✅ View system statistics
   - ✅ Approve/reject doctor registrations
   - ✅ Approve/reject laboratory registrations
   - ✅ Manage all users

---

## 📊 Admin Dashboard Features

### Statistics Overview:
- **Total Users** - Count of all registered users
- **Total Doctors** - Number of registered doctors
- **Total Laboratories** - Number of registered labs
- **Pending Approvals** - Doctors/Labs waiting for approval

### Pending Approvals Section:
- View all doctors and labs that need approval
- **Approve Button** ✅ - Activate the doctor/lab account
- **Reject Button** ❌ - Remove the registration

---

## 🔒 Security Notes

- Your admin account is **auto-approved** and has full system access
- Admin role gives you access to all administrative features
- The password is securely hashed in the database using bcrypt
- JWT token expires after 7 days

---

## 📝 Environment Configuration

The credentials are stored in:
```
backend/.env
```

```properties
ADMIN_EMAIL=avdhut@gmail.com
ADMIN_PASSWORD=Avdhut@09
```

---

## ✨ Next Steps

1. **Login to the admin panel**
2. **Test the approval workflow:**
   - Register as a doctor or lab (use /register page)
   - Login as admin
   - Approve the registration
   - Login as that doctor/lab to access their dashboard

3. **Explore admin features:**
   - Dashboard statistics
   - User management
   - Approval system

---

## 🎯 Servers Status

Make sure both servers are running:

- ✅ **Backend:** http://localhost:5000
- ✅ **Frontend:** http://localhost:5173

---

**Welcome to your Healthcare Admin Panel, Avdhut!** 🚀

*Created: October 18, 2025*
