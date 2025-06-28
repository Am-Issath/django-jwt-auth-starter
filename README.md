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
git clone [https://github.com/your-username/django-jwt-auth-rbac.git](https://github.com/your-username/django-jwt-auth-rbac.git)
cd django-jwt-auth-rbac

### 2. Create Virtual Environment

```bash
python -m venv venv
source venv/bin/activate  # For Windows: venv\Scripts\activate

3. Install Dependencies
Bash

pip install -r requirements.txt
4. Run Migrations
Bash

python manage.py makemigrations
python manage.py migrate
5. Create Superuser (Optional)
Bash

python manage.py createsuperuser
🔑 Auth Endpoints
Endpoint

Method

Auth Required

Description

/api/token/

POST

❌

Login — returns access + refresh

/api/token/refresh/

POST

❌

Get new access token using refresh

/api/register/

POST

❌

Register new user with role

/api/logout/

POST

✅

Logout (blacklist refresh token)

/api/admin-only/

GET

✅ + Admin

Admin-only endpoint


Export to Sheets
🔐 JWT Auth Flow
Login via /api/token/ to get access & refresh tokens.

Send access_token in every subsequent request header:
Authorization: Bearer <access_token>

When the access token expires, use the refresh_token at /api/token/refresh/ to obtain a new access token.

To logout: Call /api/logout/ with the refresh token in the request body.

🛡️ Role-Based Access Control (RBAC)
The CustomUser model includes a role field (admin, seller, customer). Protected views leverage custom permissions:

Python

permission_classes = [IsAuthenticated, IsAdmin]
You can easily create seller-only or customer-only views by using IsSeller or IsCustomer permissions respectively.

🧪 Testing the APIs (Manually or Postman)
🔐 Login
POST /api/token/

JSON

{
  "username": "admin",
  "password": "yourpassword"
}
🛑 Logout
POST /api/logout/

Headers:
Authorization: Bearer <access_token>

Body:

JSON

{
  "refresh": "<refresh_token>"
}
👮 Admin Only
GET /api/admin-only/

Headers:
Authorization: Bearer <access_token>

If the user is not an admin, the API will return:

JSON

{
  "detail": "You do not have permission to perform this action."
}
📌 Settings
Relevant settings in your settings.py:

Python

# settings.py

REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': (
        'rest_framework_simplejwt.authentication.JWTAuthentication',
    )
}

SIMPLE_JWT = {
    'ACCESS_TOKEN_LIFETIME': timedelta(minutes=15),
    'REFRESH_TOKEN_LIFETIME': timedelta(days=7),
    'ROTATE_REFRESH_TOKENS': True,
    'BLACKLIST_AFTER_ROTATION': True,
    'AUTH_HEADER_TYPES': ('Bearer',),
}

AUTH_USER_MODEL = 'accounts.CustomUser'


✍️ Author
Built with ❤️ by Issath
Backend Engineer · Blogger · Builder

