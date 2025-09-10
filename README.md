# Job-Portal-Project-A-Full-Stack-Application
Job Portal Project A Full Stack Application

Prerequisites

Node.js v16+ (recommend v18+) and npm.

Git (optional).

MongoDB: either a local MongoDB server or a MongoDB Atlas cluster.

(Optional) a Cloudinary account if you want file uploads to work.

Two terminal windows/tabs (one for backend, one for frontend).






0) Unzip / position project

Assuming you already extracted jobportal-yt-main.zip to a folder like ~/projects/jobportal-yt-main:

cd ~/projects/jobportal-yt-main


You should see backend/ and frontend/ directories.

IMPORTANT: Fix a blocking bug (required)

Open backend/models/application.model.js and replace the incorrect export at the bottom with the correct one.

Problem: file currently exports User and references userSchema (wrong file). This prevents the server from starting.

Fix: open the file and ensure the last line is:

// at the end of backend/models/application.model.js
export const Application = mongoose.model("Application", applicationSchema);


(If the file currently has export const User = mongoose.model('User', userSchema); — replace that line with the correct line above.)






1) Create .env for backend

In backend/ create a file named .env with these variables (fill placeholders):

# backend/.env
PORT=3000
MONGO_URI=mongodb://localhost:27017/jobportal   # or your Atlas URI
SECRET_KEY=your_jwt_secret_here                 # generate a long random string
CLOUD_NAME=your_cloudinary_cloud_name
API_KEY=your_cloudinary_api_key
API_SECRET=your_cloudinary_api_secret
NODE_ENV=development
FRONTEND_URL=http://localhost:5173              # optional: used for CORS


Tip: generate a strong SECRET_KEY:

# on mac/linux
node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"


If you do not plan to use Cloudinary right now, you can leave CLOUD_* blank, but some routes that upload files may fail.



2) Install & run backend
cd backend
npm install
# start in dev mode (nodemon) or production:
npm run dev     # recommended for development (nodemon)
# or
node index.js


Expected output: Server running at port 3000 and a successful MongoDB connection message (depends on code). If you see errors, read the "Troubleshooting" section below.



3) Configure & run frontend

Create a frontend env file (optional but recommended): frontend/.env

VITE_API_URL=http://localhost:3000/api/v1


Install & run:

cd ../frontend
npm install
npm run dev


Vite will print the dev URL (usually http://localhost:5173). Open that in your browser.

If the project does not read VITE_API_URL, open the frontend source and find where API base URL is defined (search for http:// or api/v1) and update to http://localhost:3000/api/v1.




4) Sequence to start everything

Start MongoDB (local) or ensure your Atlas connection string works.

Start backend (cd backend && npm run dev).

Start frontend (cd frontend && npm run dev).

Open http://localhost:5173 (or the Vite URL shown) and test.




5) Quick smoke tests (curl / Postman)
Register (multipart)
curl -X POST "http://localhost:3000/api/v1/user/register" \
  -F "fullname=Test User" -F "email=test@example.com" -F "phoneNumber=9876543210" \
  -F "password=Password123" -F "role=user" \
  -F "file=@./path/to/sample_resume.pdf"

Login (JSON) — stores JWT in cookie
curl -i -c cookies.txt -X POST "http://localhost:3000/api/v1/user/login" \
  -H "Content-Type: application/json" \
  -d '{"email":"test@example.com","password":"Password123","role":"user"}'

Use cookie to call an authenticated route (example: get applied jobs)
curl -b cookies.txt "http://localhost:3000/api/v1/application/get"

Apply to a job (replace <JOB_ID>):

Note: the route in the analyzed code was implemented as GET /application/apply/:id. Ideally it should be POST. Check route file; if still GET, use GET — but best practice is POST.

If route is POST:

curl -X POST -b cookies.txt "http://localhost:3000/api/v1/application/apply/<JOB_ID>"


If route is GET (current code may be GET), then:

curl -b cookies.txt "http://localhost:3000/api/v1/application/apply/<JOB_ID>"




6) Common troubleshooting

Server crashes on start

Check the fix above (application model). Look at error trace: ReferenceError: userSchema is not defined or Model not defined => fix the model export.

Mongo connection errors

Ensure MONGO_URI is correct and MongoDB is running. For local Mongo: sudo service mongod start (Linux) or use MongoDB Compass/Atlas.

CORS issues

Set FRONTEND_URL in .env and ensure backend cors() is configured to allow that origin with credentials: true.

File uploads failing

Ensure Cloudinary env variables are set. If you don’t use Cloudinary, comment out upload code or provide placeholders.

Cookie not sent in frontend requests

When calling from the browser, include credentials: fetch(url, { credentials: 'include', ... }).

Backend must set cors with credentials: true.

Wrong cookie flag httpsOnly

The code had a typo: it should be httpOnly. That typo won’t crash server but will prevent secure cookie behavior. Search httpsOnly in backend/controllers/user.controller.js and replace with httpOnly and set secure: process.env.NODE_ENV==='production'.




7) Useful developer tips

Run backend with npm run dev (nodemon) for auto-reload.

Use Postman or Insomnia to test endpoints easier (cookie support helps).

Add .env.example to version control listing required env variables (do not commit real secrets).

If you want a single command to run backend + frontend, consider concurrently or create a root package.json script. Example:

npm install -g concurrently
concurrently "cd backend && npm run dev" "cd frontend && npm run dev"




8) Optional: start Mongo using Docker

If you prefer Docker:

# docker-compose.yml (example)
version: "3.8"
services:
  mongo:
    image: mongo:6
    restart: always
    ports:
      - "27017:27017"
    volumes:
      - mongo-data:/data/db
volumes:
  mongo-data:


Then docker-compose up -d and use mongodb://localhost:27017/jobportal as MONGO_URI.




9) Checklist before demo

 backend/.env set with MONGO_URI, SECRET_KEY, CLOUD_* (if used)

 Fixed application.model.js export

 npm install run in both backend and frontend

 Backend running (Server running at port 3000)

 Frontend running (Vite dev server open)

 One test user registered and logged in
