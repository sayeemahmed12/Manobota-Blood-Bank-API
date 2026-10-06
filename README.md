# 🩸 Manobota Blood Bank API

A RESTful Blood Bank API built with **Django** and **Django REST Framework** to manage users, blood groups, donor profiles, authentication, and donor availability.

The API provides secure JWT-based authentication, donor profile management, blood-group filtering, pagination, and user-specific profile access.

---

## ✨ Features

* 🔐 JWT Authentication
* 👤 Custom User Model
* 📧 Email-based authentication
* 🩸 Blood Group Management
* 🧑‍🤝‍🧑 Donor Profile Management
* 📍 Donor location information
* ✅ Donor availability status
* 📊 Total donation tracking
* 🔎 Donor filtering by blood group and availability
* 📄 API pagination
* 🔑 Djoser authentication endpoints
* 🛡️ Permission-based API access
* 🌐 CORS configuration
* 🗄️ PostgreSQL database support
* 🚀 Production-ready Gunicorn configuration
* 📦 Static file handling with WhiteNoise

---

## 🛠️ Tech Stack

### Backend

* Python
* Django
* Django REST Framework
* Djoser
* Simple JWT
* Django Filters
* DRF Nested Routers
* DRF YASG

### Database

* PostgreSQL

### Deployment & Production

* Gunicorn
* WhiteNoise
* Render

### Other Tools

* Python Decouple
* django-cors-headers
* Pillow
* PyJWT
* Cryptography

---

## 📁 Project Structure

```text
Manobota-Blood-Bank-API/
│
├── API/
│   ├── urls.py
│   └── ...
│
├── django_app/
│   ├── models.py
│   ├── serializers.py
│   ├── views.py
│   ├── permissions.py
│   ├── paganations.py
│   └── ...
│
├── django_project/
│   ├── settings.py
│   ├── urls.py
│   ├── wsgi.py
│   └── ...
│
├── user_app/
│   ├── models.py
│   ├── serializers.py
│   ├── views.py
│   ├── manager.py
│   └── ...
│
├── templates/
│   └── email/
│
├── manage.py
├── requirements.txt
└── .gitignore
```

---

## 🗄️ Data Models

### User

The project uses a custom Django user model with email as the login field.

```text
User
├── id
├── full_name
├── email
└── phone_number
```

The custom user model removes the default username field and uses email authentication instead.

### Blood Group

```text
BloodGroup
├── id
└── name
```

### Donor Profile

```text
DonorProfile
├── id
├── user
├── blood_group
├── district
├── upazila
├── village
├── last_donation_date
├── available
└── total_donations
```

Each donor profile is associated with one user and one blood group.

---

# 🚀 Getting Started

## Prerequisites

Make sure you have the following installed:

* Python 3.10+
* PostgreSQL
* Git
* pip
* Virtual Environment

---

## 1. Clone the Repository

```bash
git clone https://github.com/sayeemahmed12/Manobota-Blood-Bank-API.git
```

Navigate into the project:

```bash
cd Manobota-Blood-Bank-API
```

---

## 2. Create a Virtual Environment

### Linux / macOS

```bash
python3 -m venv venv
source venv/bin/activate
```

### Windows

```bash
python -m venv venv
venv\Scripts\activate
```

---

## 3. Install Dependencies

```bash
pip install -r requirements.txt
```

The project uses Django, Django REST Framework, Simple JWT, Djoser, PostgreSQL support, Django Filters, CORS Headers, Gunicorn, WhiteNoise, and other dependencies listed in `requirements.txt`.

---

# 🔐 Environment Variables

Create a `.env` file in the root directory.

```env
SECRET_KEY=your-secret-key
DEBUG=True

DBNAME=your_database_name
DBUSER=your_database_user
DBPASSWORD=your_database_password
DBHOST=localhost
DBPORT=5432

EMAIL_BACKEND=django.core.mail.backends.smtp.EmailBackend
EMAIL_HOST=smtp.example.com
EMAIL_PORT=587
EMAIL_USE_TLS=True
EMAIL_HOST_USER=your-email
EMAIL_HOST_PASSWORD=your-email-password
DEFAULT_FROM_EMAIL=your-email
```

> ⚠️ Never commit your `.env` file or production secrets to GitHub.

The project reads its secret key, database configuration, debug setting, and email configuration through environment variables.

---

# 🗃️ Database Setup

Run migrations:

```bash
python manage.py makemigrations
python manage.py migrate
```

Create a superuser:

```bash
python manage.py createsuperuser
```

---

# ▶️ Run the Development Server

```bash
python manage.py runserver
```

The API will be available at:

```text
http://127.0.0.1:8000/
```

---

# 📡 API Endpoints

All application API endpoints are available under:

```text
/api/
```

The project uses Django REST Framework routers and Djoser authentication routes.

## 👤 User Registration

### Register User

```http
POST /api/register/
```

Example request:

```json
{
  "full_name": "John Doe",
  "email": "john@example.com",
  "phone_number": "01700000000",
  "password": "StrongPassword123!",
  "password2": "StrongPassword123!"
}
```

Example response:

```json
{
  "message": "User registered successfully.",
  "user": {
    "full_name": "John Doe",
    "email": "john@example.com",
    "phone_number": "01700000000"
  }
}
```

The registration endpoint validates email uniqueness and confirms that both password fields match.

---

# 🔑 Authentication

Authentication is handled through **Djoser + Simple JWT**.

### Obtain JWT Token

```http
POST /api/auth/jwt/create/
```

Example:

```json
{
  "email": "john@example.com",
  "password": "StrongPassword123!"
}
```

### Refresh Token

```http
POST /api/auth/jwt/refresh/
```

### Verify Token

```http
POST /api/auth/jwt/verify/
```

Authenticated requests should include:

```http
Authorization: Bearer <access_token>
```

The access token lifetime is configured for 1 hour, while refresh tokens last 7 days.

---

# 🩸 Blood Group API

### List Blood Groups

```http
GET /api/blood-group/
```

### Get Blood Group

```http
GET /api/blood-group/{id}/
```

### Create Blood Group

```http
POST /api/blood-group/
```

### Update Blood Group

```http
PUT /api/blood-group/{id}/
```

### Delete Blood Group

```http
DELETE /api/blood-group/{id}/
```

Blood-group management uses a DRF `ModelViewSet` with administrator-level write permissions.

---

# 🧑‍🤝‍🧑 Donor API

### List Donors

```http
GET /api/donor/
```

### Get Donor

```http
GET /api/donor/{id}/
```

### Create Donor Profile

```http
POST /api/donor/
```

Example:

```json
{
  "blood_group": 1,
  "district": "Dhaka",
  "upazila": "Dohar",
  "village": "Example Village",
  "last_donation_date": null,
  "available": true
}
```

The authenticated user is automatically assigned to the donor profile, and a user cannot create more than one donor profile.

### Update Donor Profile

```http
PUT /api/donor/{id}/
```

### Delete Donor Profile

```http
DELETE /api/donor/{id}/
```

---

# 🔎 Donor Filtering

Donors can be filtered by blood group and availability.

### Filter by Blood Group

```http
GET /api/donor/?blood_group=1
```

### Filter by Availability

```http
GET /api/donor/?available=true
```

### Combine Filters

```http
GET /api/donor/?blood_group=1&available=true
```

The donor endpoint uses Django Filter, Search, and Ordering backends along with custom pagination.

---

# 👤 My Donor Profile

Authenticated users can access their own donor profile through:

```http
GET /api/my-donor-profile/
```

This endpoint only returns the donor profile belonging to the currently authenticated user.

---

# 📧 Password Reset

Djoser is configured for password reset functionality.

Password reset endpoints are available under:

```text
/api/auth/
```

The project also includes a custom password reset email template.

---

# 🔒 Permissions

The API uses Django REST Framework permission classes to control access.

Examples:

* Public users can register.
* Authenticated users can manage their own donor profile.
* Blood group modifications require appropriate permissions.
* Users cannot create multiple donor profiles.
* Donor information can be filtered and paginated.

---

# 📄 API Documentation

The project includes **DRF YASG** for API documentation generation.

You can extend the project to expose Swagger/OpenAPI documentation for easier API testing and integration.

---

# 🌐 Deployment

The backend is configured for production deployment using:

* Gunicorn
* WhiteNoise
* PostgreSQL
* Environment variables
* CORS configuration

Example Gunicorn command:

```bash
gunicorn django_project.wsgi:application
```

The Django project also includes configuration for Render deployment.

---

# 🧪 Testing API

You can test the API using:

* Postman
* Insomnia
* Thunder Client
* Swagger/OpenAPI
* cURL

Example:

```bash
curl http://127.0.0.1:8000/api/blood-group/
```

---

# 🔮 Future Improvements

Some features that could be added in future versions:

* 🔍 Advanced donor search
* 📍 Nearby donor search using location
* 📱 SMS notifications
* 📧 Automated donor notifications
* 🏥 Hospital management
* 🩸 Blood request management
* 🚨 Emergency blood requests
* 📊 Blood donation statistics
* 🗺️ Location-based donor matching
* 🔔 Real-time notifications
* 📚 Complete Swagger API documentation
* 🧪 Automated API tests

---

# 🤝 Contributing

Contributions are welcome!

### 1. Fork the repository

```bash
git fork https://github.com/sayeemahmed12/Manobota-Blood-Bank-API
```

### 2. Clone your fork

```bash
git clone https://github.com/YOUR_USERNAME/Manobota-Blood-Bank-API.git
```

### 3. Create a new branch

```bash
git checkout -b feature/your-feature
```

### 4. Make your changes

### 5. Commit your changes

```bash
git add .
git commit -m "Add new feature"
```

### 6. Push your branch

```bash
git push origin feature/your-feature
```

### 7. Open a Pull Request

---

# 👨‍💻 Author

### Sayeem Ahmed

Full-Stack Web Developer passionate about building modern web applications, REST APIs, and scalable backend systems.

* GitHub: [@sayeemahmed12](https://github.com/sayeemahmed12)

---

# ⭐ Support

If you find this project useful, consider giving the repository a ⭐ on GitHub.

**Every contribution and feedback is appreciated!**

---

## 📜 License

This project is currently available for educational and development purposes.
