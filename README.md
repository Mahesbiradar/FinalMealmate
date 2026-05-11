# 🍔 Meal Buddy — Food Delivery Web App

A full-stack food delivery web application built with **Django 6**. Customers can browse restaurants, explore menus, add items to a cart, and pay securely via **Razorpay**. Admins can manage restaurants and menus through a dedicated dashboard.

---

## 📋 Table of Contents

- [Features](#-features)
- [Tech Stack](#-tech-stack)
- [Project Structure](#-project-structure)
- [Requirements](#-requirements)
- [Local Setup Guide](#-local-setup-guide)
- [Environment Configuration](#-environment-configuration)
- [Running the Project](#-running-the-project)
- [Default Credentials](#-default-credentials)
- [API / URL Reference](#-api--url-reference)
- [Database Overview](#-database-overview)
- [Payment Integration](#-payment-integration)
- [Deployment (PythonAnywhere)](#-deployment-pythonanywhere)
- [Known Limitations](#-known-limitations)

---

## ✨ Features

### Customer
- Browse all registered restaurants with photos and ratings
- View full menu for any restaurant (veg / non-veg badges)
- Add items to a personal cart
- Review cart with itemised total
- Secure checkout via Razorpay (UPI, cards, net banking)
- Order confirmation with delivery address

### Admin
- Add, edit, and delete restaurants
- Manage menu items per restaurant (add items with photo, price, veg flag)
- Full access to Django's built-in `/admin/` panel for database management

---

## 🛠 Tech Stack

| Layer | Technology |
|-------|-----------|
| Language | Python 3.11+ |
| Web Framework | Django 6.0.3 |
| Database | SQLite 3 (file-based, zero config) |
| ORM | Django ORM |
| Templates | Django Template Language (DTL) |
| Frontend | Bootstrap 5.3 · Bootstrap Icons · Google Fonts (Poppins) |
| Payment Gateway | Razorpay SDK 2.0.1 + Razorpay.js |
| HTTP Client | requests 2.33+ |
| Deployment Target | PythonAnywhere (WSGI) |

---

## 📁 Project Structure

```
FinalMealmate/
│
├── manage.py                        # Django CLI entry point
├── requirements.txt                 # Python dependencies
├── db.sqlite3                       # SQLite database (auto-created)
├── README.md                        # This file
│
├── meal_buddy/                      # Django PROJECT (configuration)
│   ├── settings.py                  # All app settings
│   ├── urls.py                      # Root URL dispatcher
│   ├── wsgi.py                      # WSGI entry for production
│   └── asgi.py                      # ASGI entry (async support)
│
├── delivery/                        # Main Django APP
│   ├── models.py                    # Database models (Customer, Restaurant, Item, Cart)
│   ├── views.py                     # All view functions (business logic)
│   ├── urls.py                      # App-level URL patterns
│   ├── admin.py                     # Django admin registrations
│   ├── apps.py                      # App config
│   ├── migrations/                  # Database migration files
│   │   ├── 0001_initial.py
│   │   ├── 0002_restaurant.py
│   │   ├── 0003_item.py
│   │   ├── 0004_alter_item_vegeterian.py
│   │   └── 0005_cart.py
│   └── templates/
│       └── delivery/
│           ├── base.html            # Master layout (navbar, footer, CSS/JS)
│           ├── index.html           # Landing / welcome page
│           ├── signin.html          # Login form
│           ├── signup.html          # Registration form
│           ├── fail.html            # Login failure page
│           ├── success.html         # Generic success page
│           ├── admin_home.html      # Admin dashboard
│           ├── add_restaurant.html  # Add restaurant form
│           ├── show_restaurants.html# Restaurant list (admin)
│           ├── update_restaurant.html  # Edit restaurant form
│           ├── update_menu.html     # Menu management
│           ├── customer_home.html   # Restaurant cards (customer)
│           ├── customer_menu.html   # Food item cards (customer)
│           ├── cart.html            # Shopping cart
│           ├── checkout.html        # Razorpay checkout
│           └── orders.html          # Order confirmation
│
└── venv/                            # Python virtual environment (not committed)
```

---

## ✅ Requirements

### System Requirements

| Requirement | Minimum Version | Notes |
|-------------|----------------|-------|
| Python | 3.11 | 3.12 / 3.13 / 3.14 also supported |
| pip | 23+ | Bundled with Python |
| Git | Any recent version | For cloning the repo |
| Internet connection | — | Required to load Bootstrap/Fonts CDN at runtime |

### Python Packages

Listed in `requirements.txt`:

| Package | Version | Purpose |
|---------|---------|---------|
| `Django` | 6.0.3 | Web framework — routing, ORM, templates, admin |
| `razorpay` | 2.0.1 | Razorpay Python SDK — create payment orders server-side |

> All other packages (`asgiref`, `sqlparse`, `requests`, `tzdata`, etc.) are installed automatically as transitive dependencies.

### No Additional Services Required

- **Database:** SQLite — file-based, no server needed
- **Frontend build:** None — Bootstrap loaded via CDN
- **Message broker / cache:** Not used

---

## 🚀 Local Setup Guide

Follow these steps exactly to get the project running on your machine.

### Step 1 — Clone the Repository

```bash
git clone https://github.com/Gamana/FinalMealmate.git
cd FinalMealmate
```

### Step 2 — Create a Python Virtual Environment

**Windows (PowerShell):**
```powershell
python -m venv venv
.\venv\Scripts\Activate.ps1
```

**macOS / Linux:**
```bash
python3 -m venv venv
source venv/bin/activate
```

> After activation your terminal prompt will show `(venv)`.
> Always make sure the venv is active before running any project commands.

### Step 3 — Install Dependencies

```bash
pip install -r requirements.txt
```

Expected output — all of the following are installed:
```
Django-6.0.3
razorpay-2.0.1
asgiref-3.11.1
sqlparse-0.5.5
requests-2.33.1
certifi, idna, urllib3, charset-normalizer
tzdata-2026.x
```

### Step 4 — Configure Settings for Local Development

Open `meal_buddy/settings.py` and make the following changes:

```python
# Change this ↓
SECRET_KEY = 'your-secure-secret-key-here-change-this'
# To any long random string, e.g. ↓
SECRET_KEY = 'django-insecure-local-dev-replace-me-with-50-random-chars'

# Change this ↓
DEBUG = False
# To ↓
DEBUG = True

# Change this ↓
ALLOWED_HOSTS = ['yourusername.pythonanywhere.com']
# To ↓
ALLOWED_HOSTS = ['127.0.0.1', 'localhost']
```

> **Why?** The repository ships with production-hardened settings (DEBUG off, restricted hosts). Running with `DEBUG = False` locally will give you a blank error page instead of useful stack traces, and `ALLOWED_HOSTS` must include `127.0.0.1` for the dev server.

### Step 5 — Run Database Migrations

```bash
python manage.py migrate
```

This creates all tables in `db.sqlite3`. Expected output:
```
Operations to perform:
  Apply all migrations: admin, auth, contenttypes, delivery, sessions
Running migrations:
  Applying contenttypes.0001_initial... OK
  Applying auth.0001_initial... OK
  ...
  Applying delivery.0005_cart... OK
  Applying sessions.0001_initial... OK
```

### Step 6 — (Optional) Create a Django Superuser

Needed only if you want to access the Django admin panel at `/admin/`.

```bash
python manage.py createsuperuser
```

Follow the prompts to set a username, email, and password.

### Step 7 — (Optional) Seed Sample Data

The repository already contains a pre-seeded `db.sqlite3` with:
- 2 restaurants (Truffles, Dominos)
- 6 menu items
- Several customer accounts
- An app-level admin account (`username: admin`, `password: 123`)
- A Django superuser (`username: admin`)

If you start fresh (deleted `db.sqlite3`), create the admin customer manually:

```bash
python manage.py shell -c "
from delivery.models import Customer
Customer.objects.create(
    username='admin',
    password='admin123',
    email='admin@mealbuddy.com',
    mobile='9999999999',
    address='Admin Office, Meal Buddy HQ'
)
print('Admin customer created.')
"
```

### Step 8 — Verify Everything is Working

```bash
python manage.py check
```

Expected: `System check identified no issues (0 silenced).`

---

## ⚙️ Environment Configuration

All configuration lives in `meal_buddy/settings.py`. Key variables:

| Setting | Development Value | Production Value |
|---------|-----------------|-----------------|
| `DEBUG` | `True` | `False` |
| `SECRET_KEY` | Any string | Long random secret (keep private) |
| `ALLOWED_HOSTS` | `['127.0.0.1', 'localhost']` | `['yourdomain.pythonanywhere.com']` |
| `DATABASES` | SQLite (default) | PostgreSQL recommended |
| `RAZORPAY_KEY_ID` | Test key (in repo) | Live key from Razorpay dashboard |
| `RAZORPAY_KEY_SECRET` | Test secret (in repo) | Live secret from Razorpay dashboard |

> **Security note:** For any real deployment, move `SECRET_KEY` and Razorpay credentials into environment variables or a `.env` file and never commit them to Git.

---

## ▶️ Running the Project

### Start the Development Server

```bash
python manage.py runserver
```

The server starts at: **http://127.0.0.1:8000/**

### Custom Port

```bash
python manage.py runserver 8080
```

### Access Points

| URL | Description |
|-----|-------------|
| `http://127.0.0.1:8000/` | Landing page |
| `http://127.0.0.1:8000/open_signin` | Customer / Admin login |
| `http://127.0.0.1:8000/open_signup` | New customer registration |
| `http://127.0.0.1:8000/admin/` | Django admin panel |

---

## 🔑 Default Credentials

### App Login — Admin Role
Used at `http://127.0.0.1:8000/open_signin`

| Field | Value |
|-------|-------|
| Username | `admin` |
| Password | `123` |

Logs into the **restaurant management dashboard** (add/edit/delete restaurants and menus).

### Django Admin Panel
Used at `http://127.0.0.1:8000/admin/`

| Field | Value |
|-------|-------|
| Username | `admin` |
| Password | `Admin@1234` *(or whatever you set during `createsuperuser`)* |

Full database management UI — view and edit all records directly.

### Sample Customer Accounts
Pre-seeded in the database:

| Username | Password |
|----------|----------|
| `rajat` | *(check DB or reset)* |
| `deep` | *(check DB or reset)* |

To inspect or reset any customer's password via the shell:
```bash
python manage.py shell -c "
from delivery.models import Customer
c = Customer.objects.get(username='rajat')
print('Current password:', c.password)
c.password = 'newpassword'
c.save()
"
```

---

## 🗺 API / URL Reference

All routes are server-rendered (HTML responses). No JSON API.

### Public Routes

| Method | URL | View | Description |
|--------|-----|------|-------------|
| GET | `/` | `index` | Landing page |
| GET | `/open_signin` | `open_signin` | Sign-in form |
| GET | `/open_signup` | `open_signup` | Sign-up form |
| POST | `/signin` | `signin` | Authenticate user |
| POST | `/signup` | `signup` | Register new customer |

### Admin Routes

| Method | URL | View | Description |
|--------|-----|------|-------------|
| GET | `/open_add_restaurant` | `open_add_restaurant` | Add restaurant form |
| POST | `/add_restaurant` | `add_restaurant` | Create restaurant |
| GET | `/open_show_restaurant` | `open_show_restaurant` | List all restaurants |
| GET | `/open_update_restaurant/<id>` | `open_update_restaurant` | Edit restaurant form |
| POST | `/update_restaurant/<id>` | `update_restaurant` | Save restaurant edits |
| GET | `/delete_restaurant/<id>` | `delete_restaurant` | Delete restaurant |
| GET | `/open_update_menu/<id>` | `open_update_menu` | Menu management page |
| POST | `/update_menu/<id>` | `update_menu` | Add menu item |

### Customer Routes

| Method | URL | View | Description |
|--------|-----|------|-------------|
| GET | `/view_menu/<restaurant_id>/<username>` | `view_menu` | View restaurant menu |
| GET | `/add_to_cart/<item_id>/<username>` | `add_to_cart` | Add item to cart |
| GET | `/show_cart/<username>` | `show_cart` | View cart |
| GET | `/checkout/<username>/` | `checkout` | Razorpay checkout |
| GET | `/orders/<username>/` | `orders` | Order confirmation |

---

## 🗄 Database Overview

SQLite database (`db.sqlite3`) with four custom models:

```
Customer ──< Cart >── Item ──> Restaurant
```

| Model | Key Fields | Notes |
|-------|-----------|-------|
| `Customer` | `username`, `password`, `email`, `mobile`, `address` | Custom auth; no Django User link |
| `Restaurant` | `name`, `picture`, `cuisine`, `rating` | Top-level entity |
| `Item` | `name`, `description`, `price`, `vegeterian`, `picture` | FK → Restaurant |
| `Cart` | `customer` (FK), `items` (M2M → Item) | One cart per customer; cleared after order |

### Useful Shell Commands

```bash
# Django interactive shell
python manage.py shell

# List all restaurants
from delivery.models import Restaurant
Restaurant.objects.all().values('id', 'name', 'rating')

# List all customers
from delivery.models import Customer
Customer.objects.values('username', 'email')

# Check a cart
from delivery.models import Cart, Customer
c = Customer.objects.get(username='rajat')
cart = Cart.objects.filter(customer=c).first()
print(cart.items.all())
```

---

## 💳 Payment Integration

The project uses **Razorpay** in test mode.

### How it Works

1. Customer clicks **Proceed to Checkout** from the cart page
2. Django calls `razorpay.Client.order.create()` server-side → gets an `order_id`
3. Razorpay.js renders a payment modal in the browser
4. On payment success, the browser is redirected to `/orders/<username>/`
5. The cart is cleared and an order confirmation is shown

### Test Mode Credentials

The repository ships with Razorpay **test** credentials in `settings.py`:

```python
RAZORPAY_KEY_ID     = 'rzp_test_lT6VV3Hhr4ayCQ'
RAZORPAY_KEY_SECRET = 'eFILRtRtJyDqNpE4Qkz5a3K9'
```

In test mode, use these **test card details**:

| Field | Value |
|-------|-------|
| Card Number | `4111 1111 1111 1111` |
| Expiry | Any future date (e.g. `12/29`) |
| CVV | Any 3 digits (e.g. `123`) |
| OTP | `1234` (Razorpay test OTP) |

> To accept real payments, replace the credentials with your **live** Razorpay keys from [https://dashboard.razorpay.com](https://dashboard.razorpay.com).

---

## 🌐 Deployment (PythonAnywhere)

### Steps

1. **Upload files** — zip the project (exclude `venv/`) and upload, or `git clone` directly in the PythonAnywhere console.

2. **Create a virtual environment** on PythonAnywhere:
   ```bash
   mkvirtualenv --python=python3.11 mealbuddy
   pip install -r requirements.txt
   ```

3. **Configure the Web App:**
   - Source code: `/home/<username>/FinalMealmate`
   - Virtualenv: `/home/<username>/.virtualenvs/mealbuddy`
   - WSGI file: point to `meal_buddy/wsgi.py`

4. **Update settings.py for production:**
   ```python
   DEBUG = False
   ALLOWED_HOSTS = ['yourusername.pythonanywhere.com']
   SECRET_KEY = '<generate a new 50-char random key>'
   ```

5. **Run migrations:**
   ```bash
   python manage.py migrate
   python manage.py collectstatic
   ```

6. **Reload** the web app from the PythonAnywhere dashboard.

---

## ⚠️ Known Limitations

These are areas to improve before using in any real production environment:

| Issue | Impact | Suggested Fix |
|-------|--------|--------------|
| Passwords stored in **plaintext** | High — security risk | Use `django.contrib.auth` or `make_password` / `check_password` |
| **No session auth** — username in URL | High — any user can access another's cart | Implement `request.session` after login |
| **No Razorpay payment verification** | High — orders confirmed without verifying payment | Verify `razorpay_payment_id` signature server-side |
| Hardcoded Razorpay keys in `settings.py` | Medium | Move to `.env` file with `python-decouple` |
| `FloatField` for money | Medium — floating-point rounding errors | Use `DecimalField(max_digits=10, decimal_places=2)` |
| No order history model | Medium — cart is cleared after order | Add an `Order` model to persist paid orders |
| Typo: `vegeterian` in code | Low | Rename to `vegetarian` throughout |
| `GET` requests mutating state (`add_to_cart`, `delete_restaurant`) | Low | Change to `POST` with CSRF-protected forms |

---

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/your-feature-name`
3. Make your changes and test locally
4. Commit: `git commit -m "Add: your feature description"`
5. Push: `git push origin feature/your-feature-name`
6. Open a Pull Request

---

## 📄 License

This project is open-source and available under the [MIT License](LICENSE).

---

<div align="center">
  Made with ❤️ using Django &nbsp;|&nbsp; <strong>Meal Buddy</strong> — Delicious food, delivered fast.
</div>
