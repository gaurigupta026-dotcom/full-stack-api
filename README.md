Full Stack Assessment - User Management REST API
Tech Stack
Node.js + Express.js — Backend framework
MongoDB + Mongoose — Database & ODM
bcryptjs — Password hashing
jsonwebtoken — JWT authentication
Multer — File/image upload handling
dotenv — Environment variables
cors — Cross-Origin Resource Sharing
Project Structure
project/
├── server.js                  # Entry point
├── package.json
├── .env.example               # Env variable template
├── config/
│   └── db.js                  # MongoDB connection
├── models/
│   └── User.js                # User schema
├── controllers/
│   ├── authController.js      # Auth logic
│   └── userController.js      # User CRUD logic
├── middleware/
│   ├── authMiddleware.js      # JWT protect middleware
│   └── uploadMiddleware.js    # Multer config
├── routes/
│   ├── authRoutes.js          # /api/auth routes
│   └── userRoutes.js          # /api/users routes
└── uploads/
    └── profiles/              # Uploaded images stored here
Setup Instructions
1. Install dependencies
npm install
2. Configure environment
cp .env.example .env
Edit .env:

PORT=5000
MONGO_URI=mongodb://localhost:27017/fullstack_assessment
JWT_SECRET=your_super_secret_key
JWT_EXPIRE=7d
3. Start the server
# Production
npm start

# Development (with auto-reload)
npm run dev
API Documentation
Base URL: http://localhost:5000
🔐 Authentication APIs
1. Register User
POST /api/auth/register

Body (JSON):

{
  "name": "John Doe",
  "email": "john@example.com",
  "password": "password123"
}
Response:

{
  "success": true,
  "message": "User registered successfully",
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6...",
  "user": {
    "id": "64abc...",
    "name": "John Doe",
    "email": "john@example.com",
    "profileImage": null,
    "createdAt": "2024-01-01T00:00:00.000Z"
  }
}
2. Login User
POST /api/auth/login

Body (JSON):

{
  "email": "john@example.com",
  "password": "password123"
}
Response:

{
  "success": true,
  "message": "Login successful",
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6...",
  "user": { ... }
}
3. Get Logged-in User Profile (Protected)
GET /api/auth/me

Headers:

Authorization: Bearer <your_jwt_token>
Response:

{
  "success": true,
  "user": {
    "id": "64abc...",
    "name": "John Doe",
    "email": "john@example.com",
    "profileImage": "uploads/profiles/profile-123.jpg",
    "createdAt": "...",
    "updatedAt": "..."
  }
}
👥 User Management APIs
4. Get All Users
GET /api/users

Response:

{
  "success": true,
  "count": 2,
  "users": [ { ... }, { ... } ]
}
5. Get Single User by ID
GET /api/users/:id

Response:

{
  "success": true,
  "user": { "id": "64abc...", "name": "John Doe", ... }
}
6. Update User
PUT /api/users/:id

Body (JSON): (send only fields to update)

{
  "name": "Jane Doe",
  "email": "jane@example.com"
}
Response:

{
  "success": true,
  "message": "User updated successfully",
  "user": { ... }
}
7. Delete User
DELETE /api/users/:id

Response:

{
  "success": true,
  "message": "User deleted successfully",
  "deletedUser": { "id": "64abc...", "name": "John Doe", "email": "john@example.com" }
}
🖼️ Extra Functional API
8. Upload Profile Image (Protected — uses Multer)
POST /api/users/upload-profile

Headers:

Authorization: Bearer <your_jwt_token>
Content-Type: multipart/form-data
Body (form-data):

Key: profileImage   Type: File   Value: <select image>
Response:

{
  "success": true,
  "message": "Profile image uploaded successfully",
  "profileImage": "uploads/profiles/profile-1234567890.jpg",
  "imageUrl": "http://localhost:5000/uploads/profiles/profile-1234567890.jpg"
}
Testing with Postman
Register → copy the token
Login → copy the token
For protected routes, add Header:
Key: Authorization
Value: Bearer <token>
For upload, use form-data with key profileImage
