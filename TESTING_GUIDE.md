# TESTING GUIDE - ROLE-BASED ROUTING FIX

## CRITICAL: Clear All Browser Data First

1. **Open Browser DevTools** (F12)
2. **Go to Application tab** → Storage → Local Storage
3. **Delete ALL entries:**
   - Delete `user`
   - Delete `role`
   - Delete any other keys
4. **Close DevTools**
5. **Hard Refresh Browser** (Ctrl+Shift+R or Cmd+Shift+R on Mac)

## TEST STEP 1: Student Login

```
1. Navigate to: http://localhost:5173 (or your frontend URL)
2. You should see Login page with role selector
3. Click "Student" button
4. Enter:
   - Enrollment No.: STU001
   - Password: (check database for correct password)
5. Click "Login as Student"
```

### Expected Results:
```
Browser Console should show:
  ✓ "[Login] Attempting login with role: student"
  ✓ "[Login] Login response received, user: {id:'STU001', ...}"
  ✓ "[Login] === LOGIN SUCCESSFUL ==="
  ✓ "[Login] Navigating to /student-dashboard"
  ✓ "[App.isStudent] role: student -> isStudent: true"
  ✓ No error messages
```

### Browser Storage Check:
Open DevTools → Application → LocalStorage:
```
Key: "role"     Value: "student"  ✓ (NOT "admin")
Key: "user"     Value: {...}      ✓ (Contains role: "student")
```

### Page Display:
```
✓ Should show STUDENT Dashboard
✓ Should show Student Sidebar menu (Dashboard, Assignments, Marks, Performance)
✗ Should NOT show Admin menu
✗ Should NOT show Faculty menu
```

---

## TEST STEP 2: Try to Access Faculty Route (without logout)

```
1. While logged in as student, manually navigate to:
   http://localhost:5173/faculty-dashboard
```

### Expected:
```
✗ Should NOT show faculty dashboard
✓ Should redirect to /login automatically
✓ Browser console should show:
  "[App.isFaculty] role: student -> isFaculty: false"
```

---

## TEST STEP 3: Logout and Login as Faculty

```
1. Click Logout button in Student Dashboard
2. Verify browser console shows:
   "handleLogout: Removed user and role from localStorage"
3. Verify localStorage is cleared:
   - "role" key is GONE
   - "user" key is GONE
4. Login form should show
5. Click "Faculty" button
6. Enter:
   - Faculty ID: FAC001
   - Password: (check database)
7. Click "Login as Faculty"
```

### Expected Results:
```
Browser Console:
  ✓ "[Login] Attempting login with role: faculty"
  ✓ "[Login] Navigating to /faculty-dashboard"
  ✓ "[App.isFaculty] role: faculty -> isFaculty: true"
```

### Browser Storage:
```
Key: "role"     Value: "faculty"  ✓ (NOT "student" or "admin")
Key: "user"     Value: {...}      ✓ (Contains role: "faculty")
```

### Page Display:
```
✓ Should show FACULTY Dashboard
✓ Should show Faculty Sidebar menu (Dashboard, Assignments, Pending Evaluations, My Students, Marks)
✗ Should NOT show Student menu
✗ Should NOT show Admin menu
```

---

## TEST STEP 4: Logout and Login as Admin

```
1. Click Logout
2. Verify localStorage is cleared
3. Navigate to: http://localhost:5173/admin/login
4. Login with:
   - Admin ID: admin
   - Password: admin@123
5. Click "Login as Admin"
```

### Expected Results:
```
Browser Console:
  ✓ "[Login or AdminLogin] Navigating to /admin-dashboard"
  ✓ "[App.isAdmin] role: admin -> isAdmin: true"
```

### Browser Storage:
```
Key: "role"     Value: "admin"    ✓
Key: "user"     Value: {...}      ✓ (Contains role: "admin")
```

### Page Display:
```
✓ Should show ADMIN Dashboard
✓ Should show Admin Sidebar menu (Dashboard, Add Faculty, Add Student)
✗ Should NOT show Student menu
✗ Should NOT show Faculty menu
```

---

## If Tests FAIL:

### Issue: Still showing Admin Dashboard for Student
```
1. Open Browser DevTools (F12)
2. Go to Console tab
3. Run this command:
   localStorage.clear()
4. Type: location.reload()
5. Try login again
6. Capture ALL console output
```

### Issue: Seeing Console Errors
```
1. Share the EXACT error message
2. Include line number if available
3. Describe what page/action caused the error
```

### Issue: localStorage not being cleared on logout
```
Check Sidebar.jsx and TopNavbar.jsx have:
  localStorage.removeItem('user');
  localStorage.removeItem('role');
```

---

## DEBUGGING COMMANDS (Browser Console)

Check current role:
```javascript
localStorage.getItem('role')
```

Check user object:
```javascript
JSON.parse(localStorage.getItem('user'))
```

Clear everything:
```javascript
localStorage.clear()
```

---

## Expected Console Output Pattern

### Successful Student Login:
```
[Login] Attempting login with role: student
[Login] Login response received, user: {id:'STU001', name:'Student Name', role:'student'}
[Login] === LOGIN SUCCESSFUL ===
[Login] Stored user: {"id":"STU001","name":"Student Name","role":"student"}
[Login] Stored role: student
[Login] localStorage["role"]: student
[Login] localStorage["user"]: {"id":"STU001","name":"Student Name","role":"student"}
[Login] Checking user.role: student
[Login] Navigating to /student-dashboard
[App.isStudent] role: student -> isStudent: true
```

---

## What to Report Back

After running tests, please tell me:
1. ✓ Or ✗ - Does student login go to /student-dashboard?
2. ✓ Or ✗ - Does faculty login go to /faculty-dashboard?
3. ✓ Or ✗ - Does admin login go to /admin-dashboard?
4. Browser console output (copy-paste the log messages)
5. localStorage contents (what keys and values are stored)
6. Any error messages you see

This will help me identify what's still wrong.
