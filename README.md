# Secure File Sharing System – Django Backend

A secure file-sharing backend system built using Django and SQLite, with support for:

- Two user roles: **Ops** (admin uploaders) and **Clients** (verified downloaders)
- JWT-based authentication and email verification
- Secure file uploads and token-based downloads
- Complete API coverage with Postman Collection

---

## Project Structure

```
SECURE_FILE_SHARE/
├── backend/
│   ├── files/                 # File upload/download logic
│   ├── media/uploads/        # Stored uploaded files
│   ├── secure_file_share/    # Django settings and WSGI
│   ├── uploads/              # File metadata or storage logic
│   └── users/                # User models and auth (Ops & Clients)
│
├── .gitignore
├── db.sqlite3                # SQLite database
├── manage.py                 # Django entry point
├── requirements.txt          # Python dependencies
├── postman/                  # Postman collection + env
```

---

##  Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/YOUR_USERNAME/secure-share.git
cd secure-share
```

### 2. Create Virtual Environment

```bash
python -m venv venv
```

On Windows:

```bash
venv\Scripts\activate
```

On Mac/Linux:

```bash
source venv/bin/activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Run Migrations

```bash
python manage.py migrate
```

### 5. Start the Development Server

```bash
python manage.py runserver
```

---

##  Authentication & User Flow

### 1. Client Signup

**POST** `/api/client/signup/`  
Creates a new client user

---

### 2. Email Verification

**GET** `/api/client/verify/?token=<verification_token>`  
Activates the client account

---

### 3. Client Login

**POST** `/api/client/login/`  
Returns JWT token for authentication

---

### 4. Upload File (Ops only)

**POST** `/api/file/upload/`  
Authentication: Ops JWT required  
Accepts only `.pptx`, `.docx`, `.xlsx`

---

### 5. List Uploaded Files (Client only)

**GET** `/api/file/list/`  
Returns list of uploaded file names and metadata

---

### 6. Generate Download Token

**POST** `/api/file/download-token/`  
Authentication: Client JWT required  
Body: `{ "filename": "example.pptx" }`

---

### 7. Download File

**GET** `/api/file/download/?token=<download_token>`  
One-time secure file download for client

---

## 📊 API Endpoint Summary

| Endpoint                             | Method | Role    | Auth Required | Description                          |
|--------------------------------------|--------|---------|----------------|--------------------------------------|
| /api/client/signup/                  | POST   | Client  | ❌              | Register a new client                |
| /api/client/verify/                  | GET    | Client  | ❌              | Verify email via token               |
| /api/client/login/                   | POST   | Client  | ❌              | Login and receive JWT                |
| /api/file/upload/                    | POST   | Ops     | ✅ (JWT)        | Upload file (.pptx/.docx/.xlsx)      |
| /api/file/list/                      | GET    | Client  | ✅ (JWT)        | List uploaded files                  |
| /api/file/download-token/            | POST   | Client  | ✅ (JWT)        | Generate one-time download token     |
| /api/file/download/?token=<token>    | GET    | Client  | ❌              | Download file using token            |

---

##  Postman Collection

You can import the Postman Collection:  
https://github.com/CSEExplorer/secure_file_share/blob/master/postman/Secure%20File%20Sharing%20API%20Collection.postman_collection.json

### Features

- Token extraction & reuse (JWT, email verification, download tokens)
- Pre-request scripts to set headers and tokens
- Full request chaining from signup to download

---

##  Security Highlights

- Role-based access control (Ops and Clients)
- JWT authentication
- One-time encrypted download links
- File validation based on extension
- Email verification before login

---

##  Notes

- JWTs should be passed in headers:
```bash
Authorization: Bearer <your_token_here>
```

- Email verification tokens are printed in the console for development.
- Download tokens expire immediately after first use.
- The backend returns proper error codes for failed auth or invalid links.

---

##  Deployment Plan

- Use Docker with `Dockerfile` and `docker-compose.yml`
- Gunicorn as WSGI app server
- Nginx reverse proxy (optional)
- SSL setup with Let's Encrypt (Certbot)
- Use PostgreSQL in production (SQLite for local dev)
- Email service: SMTP or SendGrid for real verification

---



