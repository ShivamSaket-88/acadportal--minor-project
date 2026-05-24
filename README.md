# 🎓 AcadPortal

AcadPortal is a full-stack academic management system developed for colleges and universities to manage assignments, submissions, student progress, marks evaluation, and faculty-student communication efficiently.

---

# 🚀 Features

## 👨‍🏫 Faculty Module
- Create assignments
- Upload assignment files
- View student submissions
- Evaluate submissions
- Add and manage marks
- View analytics and student progress
- Automatic email notifications to students

---

## 👨‍🎓 Student Module
- View assignments
- Submit assignment files
- Check marks and progress
- View pending assignments
- Upcoming deadline reminders
- Email/profile management

---

# 📧 Email Notification System
Whenever faculty creates a new assignment:
- Students automatically receive email notifications
- Includes assignment title, deadline, subject, branch, and semester

Implemented using:
- NodeMailer
- Gmail SMTP
- App Password Authentication

---

# 📊 Analytics System
Faculty can:
- View submitted vs pending assignments
- Track student performance
- Monitor evaluation status

---

# 🛠️ Tech Stack

## Frontend
- React.js
- Axios
- Chart.js
- React ChartJS 2

## Backend
- Node.js
- Express.js
- Multer.js
- Nodemailer

## Database
- MySQL

---

# 📂 Project Structure

```bash
acadportal/
│
├── frontend/
├── backend/
│   ├── controllers/
│   ├── routes/
│   ├── services/
│   ├── uploads/
│   └── config/
│
├── package.json
└── README.md

