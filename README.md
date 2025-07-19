📐 System Architecture

```
Frontend (React + TypeScript)
        ↓
Backend (Django + Django REST Framework)
        ↓
Database (PostgreSQL / SQLite)
        ↓
File Storage (Local / S3 for production)
```
```
Frontend handles authentication, role-based routing, assignment submission/viewing.

Backend provides RESTful APIs for managing users, assignments, and submissions.

Authentication is handled via JWT (access/refresh tokens).

File Uploads are supported for assignments and submissions.
```
 Core Entities & Relationships (ER Diagram in Tabular Format)
Entity	Fields
```
User	id, email, name, password, role (student/teacher), timestamps
Assignment	id, title, description, due_date, teacher_id, file
Submission	id, assignment_id, student_id, file, comment, submitted_at
```
Relationships
```
A Teacher can create many Assignments.

A Student can submit one Submission per Assignment.

Each Submission is linked to both Student and Assignment.
```
 API Endpoints
```
🔸 1. Teacher Creates Assignment

POST /api/create-assignment/
Headers: Authorization: Bearer <JWT>
Body: {
  "title": "Math Homework",
  "description": "Page 42, exercises 1-10",
  "due_date": "2025-07-22",
  "file": (optional file)
}
Access: Teacher only
```
🔸 2. Student Submits Assignment
```
POST /api/submit-assignment/<assignment_id>/
Headers: Authorization: Bearer <JWT>
Body (multipart/form-data):
{
  "file": <uploaded file>,
  "comment": "Please find attached my homework."
}
Access: Student only
```

🔸 3. Teacher Views Submissions
```
GET /api/view-submissions/<assignment_id>/
Headers: Authorization: Bearer <JWT>
Returns: List of student submissions for the assignment

Access: Teacher only
```
🔐 Authentication Strategy
JWT Authentication:
```
Upon login/signup, server returns access and refresh tokens.

access token is used for protected routes.

refresh token is used to obtain a new access token.

Role-based Access:

Users have a role field (student or teacher).

API views check request.user.role for permission handling.
```
Teachers can:
```
Create assignments

View all submissions
```
Students can:
```
View assignments

Submit their assignments
```

 Future Scalability Suggestions
Database:
```
Use PostgreSQL in production for better performance and relational handling.

Add indexing on frequently queried fields (e.g., assignment_id, user_id).

File Storage:

Use Amazon S3 or Google Cloud Storage for scalable file uploads.

Add file validation (size, type) at upload time.
```
Caching:
```
Use Redis to cache frequently accessed data like assignment lists or user profiles.
```
Rate Limiting:
```
Protect endpoints using DRF's throttling classes to prevent abuse.
```
CI/CD Integration:
```
Add GitHub Actions or GitLab CI for automated testing and deployment.
```

Frontend Optimization:

Use lazy loading for assignment and submission lists.

Optimize file preview/download with cloud URLs.
