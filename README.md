# Job-Portal-Project-A-Full-Stack-Application
Job Portal Project A Full Stack Application

Job Hunt – Full Stack Job Portal

A full-stack job portal application built with the MERN stack (MongoDB, Express.js, React, Node.js) featuring authentication, role-based access (Student / Recruiter), job posting, job search, filtering, applying, and resume uploads.

Features

🔐 Authentication & Roles – Students and Recruiters have separate login/signup flows.

👤 Profile Management – Edit bio, skills, resume, profile picture.

💼 Job Management – Recruiters can create, edit, and manage job postings.

🔎 Search & Filter – Search by keyword, location, category, or company.

📄 Applications – Students can apply to jobs, recruiters can view, accept, or reject applicants.

☁️ Resume & Logo Upload – Integrated with Cloudinary for file storage.

🎨 Frontend – Built with React + Vite + ShadCN UI (beautiful and responsive).

Tech Stack

Frontend: React (Vite), ShadCN UI, TailwindCSS

Backend: Node.js, Express.js

Database: MongoDB (Mongoose ODM)

Authentication: JWT + bcrypt + cookie-parser

File Uploads: Multer + Cloudinary

Project Structure

job-portal/
│── backend/ → Express.js backend
│ ├── models/ → Mongoose models (User, Job, Company, Application)
│ ├── controllers/ → Business logic
│ ├── routes/ → API routes
│ └── utils/ → DB connection, Cloudinary, etc.
│── frontend/ → React frontend (Vite)
│── README.md

Getting Started
1. Clone the Repository
git clone https://github.com/yourusername/job-portal.git
cd job-portal

2. Setup Backend
cd backend
npm install


Create a .env file in backend/ with the following:

PORT=8000
MONGO_URI=your_mongodb_connection_string
JWT_SECRET=your_secret_key
CLOUDINARY_CLOUD_NAME=your_cloud_name
CLOUDINARY_API_KEY=your_api_key
CLOUDINARY_API_SECRET=your_api_secret


Run the backend:

npm run dev


By default, it runs on http://localhost:8000
.

3. Setup Frontend
cd ../frontend
npm install


Create a .env file in frontend/ with the following:

VITE_BACKEND_URL=http://localhost:8000


Run the frontend:

npm run dev


By default, it runs on http://localhost:5173
.

API Testing

You can test APIs using Postman or Thunder Client. Example endpoints:

GET /api/jobs → Fetch jobs

POST /api/auth/register → Register a user

POST /api/auth/login → Login as Student/Recruiter



Deployment

Backend → Deploy on Render, Railway, or Vercel Functions

Frontend → Deploy on Vercel or Netlify

Database → Use MongoDB Atlas
