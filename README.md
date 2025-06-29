# 🛡️ Django JWT Auth System with Role-Based Access Control (RBAC)

A clean and production-ready JWT-based authentication system built with Django REST Framework and `djangorestframework-simplejwt`.
Supports login/logout, token blacklisting, refresh token rotation, and role-based API access (RBAC) — ideal for real-world apps like e-commerce platforms (e.g., Amazon, eBay).

---

## 🚀 Features

- 🔐 **JWT Authentication** (Access & Refresh tokens)
- 🧠 **Token Blacklisting** on Logout
- 🔄 **Token Refresh Endpoint**
- 👤 **Role-Based Permissions**: `admin`, `seller`, `customer`
- ✅ **Protected APIs** by role
- 📝 **User Registration Endpoint**
- ⚙️ **Custom User Model** (`CustomUser`)

---

## 🧰 Stack

- Python 3.x
- Django 4.x
- Django REST Framework
- djangorestframework-simplejwt

---

## ⚙️ Setup Instructions

### 1. Clone the Repo

```bash
git clone https://github.com/Am-Issath/django-jwt-auth-starter.git

cd django-jwt-auth-starter
```

### 2. Create Virtual Environment

```bash
python -m venv venv
source venv/bin/activate  # For Windows: venv\Scripts\activate
```

### 3. Install Dependencies
```bash
pip install -r requirements.txt
``` 

### 4. Run Migrations

```bash
python manage.py makemigrations
python manage.py migrate
``` 

### 5. Create Superuser (Optional)

```bash
python manage.py createsuperuser
```

### 6. 🚀 Run server

```bash
python manage.py runserver
```

## 🔐 JWT Endpoints

### ✅ Login

```http
POST /api/token/
Content-Type: application/json

{
  "username": "admin",
  "password": "yourpassword"
}
```

#### Response:
```json
{
  "access": "access_token_here",
  "refresh": "refresh_token_here",
  "username": "admin",
  "role": "admin"
}
```

### 🔁 Refresh Token

```http
POST /api/token/refresh/
Content-Type: application/json

{
  "refresh": "<your_refresh_token>"
}

```

### 🚪 Logout (Blacklist Refresh Token)

```http
POST /api/logout/
Authorization: Bearer <access_token>
Content-Type: application/json

{
  "refresh": "<refresh_token>"
}
```

### 🧑 Register

```http
POST /api/register/
Content-Type: application/json

{
  "username": "johndoe",
  "email": "john@example.com",
  "password": "yourpass",
  "role": "seller"  # or 'admin' / 'customer'
}
```

### 🔒 Admin-only Endpoint

```http
GET /api/admin-only/
Authorization: Bearer <admin_access_token>
```

#### Expected Response (Admin Only):
```json
{
  "message": "Hello, Admin!"
}
```

# 🔐 JWT Configuration

In settings.py:
```python 
REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': (
        'rest_framework_simplejwt.authentication.JWTAuthentication',
    )
}

SIMPLE_JWT = {
    'ACCESS_TOKEN_LIFETIME': timedelta(minutes=15),
    'REFRESH_TOKEN_LIFETIME': timedelta(days=7),
    'ROTATE_REFRESH_TOKEN': True,
    'BLACKLIST_AFTER_ROTATION': True,
    'AUTH_HEADER_TYPES': ('Bearer',),
}
```

# 🔐 JWT Auth Flow

1. Login via /api/token/ to get access & refresh tokens.

2. Send access_token in every subsequent request header:
```Authorization: Bearer <access_token>```

3. When the access token expires, use the refresh_token at /api/token/refresh/ to obtain a new access token.

4. To logout: Call /api/logout/ with the refresh token in the request body.

# 🛡️ Role-Based Access Control (RBAC)

The CustomUser model includes a role field (admin, seller, customer). Protected views leverage custom permissions:

# ✍️ Author
##### Built with ❤️ by Issath
##### Backend Engineer · Blogger · Builder