# ⚙️ Meal Buddy — Local Setup Guide

> **This guide is for anyone who received the project as a ZIP file and wants to run it on their machine from scratch.**
> No prior experience with Django is required. Follow every step in order.

---

## 📋 Table of Contents

1. [System Requirements](#1-system-requirements)
2. [Install Python](#2-install-python)
3. [Verify Python Installation](#3-verify-python-installation)
4. [Extract the Project ZIP](#4-extract-the-project-zip)
5. [Open a Terminal in the Project Folder](#5-open-a-terminal-in-the-project-folder)
6. [Create a Virtual Environment](#6-create-a-virtual-environment)
7. [Activate the Virtual Environment](#7-activate-the-virtual-environment)
8. [Install All Dependencies](#8-install-all-dependencies)
9. [Configure Settings for Local Development](#9-configure-settings-for-local-development)
10. [Set Up the Database](#10-set-up-the-database)
11. [Run the Development Server](#11-run-the-development-server)
12. [Open the App in Your Browser](#12-open-the-app-in-your-browser)
13. [Login Credentials](#13-login-credentials)
14. [Stopping the Server](#14-stopping-the-server)
15. [Common Errors and Fixes](#15-common-errors-and-fixes)
16. [Quick-Start Cheat Sheet](#16-quick-start-cheat-sheet)

---

## 1. System Requirements

Before starting, make sure your machine meets the following:

| Requirement | Minimum | Recommended |
|-------------|---------|-------------|
| Operating System | Windows 10 / macOS 11 / Ubuntu 20.04 | Windows 11 / macOS 13 / Ubuntu 22.04 |
| Python | 3.11 | 3.12 or 3.13 |
| RAM | 512 MB free | 1 GB free |
| Disk Space | 200 MB free | 500 MB free |
| Internet | Required (CDN for Bootstrap/Fonts) | Stable broadband |

---

## 2. Install Python

> **Skip this step if Python 3.11 or higher is already installed on your system.**

### Windows

1. Go to **https://www.python.org/downloads/**
2. Click **"Download Python 3.x.x"** (the latest stable version)
3. Run the installer
4. **IMPORTANT:** On the first screen, tick **"Add python.exe to PATH"** before clicking Install

   ```
   ☑ Add python.exe to PATH    ← tick this box
   ```

5. Click **"Install Now"**
6. Wait for installation to complete, then click **"Close"**

### macOS

1. Go to **https://www.python.org/downloads/**
2. Download the macOS installer and run it, **OR** use Homebrew:
   ```bash
   brew install python@3.12
   ```

### Ubuntu / Debian Linux

```bash
sudo apt update
sudo apt install python3.12 python3.12-venv python3-pip -y
```

---

## 3. Verify Python Installation

Open a terminal (Command Prompt / PowerShell on Windows, Terminal on Mac/Linux) and run:

```bash
pip --version
```

Expected output (version must be 3.11 or higher):
```
Python 3.12.x
```

> **On macOS/Linux**, you may need to type `python3` instead of `python`.

Also verify pip is available:
```bash
pip --version
```

Expected output:
```
pip 24.x.x from ...
```

If either command fails, revisit Step 2 and make sure Python was added to PATH.

---

## 4. Extract the Project ZIP

1. Locate the ZIP file you received (e.g. `FinalMealmate.zip`)
2. Right-click it → **Extract All** (Windows) or double-click (macOS)
3. Choose a destination folder, e.g.:
   - Windows: `C:\Projects\FinalMealmate`
   - macOS/Linux: `~/Projects/FinalMealmate`
4. After extraction you should see a folder named **`FinalMealmate`** containing:
   ```
   FinalMealmate/
   ├── manage.py
   ├── requirements.txt
   ├── SETUP.md          ← this file
   ├── README.md
   ├── db.sqlite3
   ├── meal_buddy/
   └── delivery/
   ```

> If you see another nested `FinalMealmate/FinalMealmate/` folder, navigate into the inner one. The folder you work in must contain `manage.py`.

---

## 5. Open a Terminal in the Project Folder

### Windows (PowerShell — Recommended)

1. Open the extracted `FinalMealmate` folder in File Explorer
2. Click on the address bar at the top, type `powershell`, press **Enter**
3. PowerShell opens already inside the project folder ✓

### Windows (Command Prompt)

1. Open the extracted folder in File Explorer
2. Click address bar, type `cmd`, press **Enter**

### macOS / Linux

```bash
cd ~/Projects/FinalMealmate
```

### Verify you are in the right place

Run:
```bash
# Windows
dir manage.py

# macOS / Linux
ls manage.py
```

You must see `manage.py` listed. If not, `cd` into the correct folder before continuing.

---

## 6. Create a Virtual Environment

A virtual environment keeps this project's packages separate from everything else on your machine.

```bash
python -m venv venv
```

> On macOS/Linux use `python3` if `python` doesn't work:
> ```bash
> python3 -m venv venv
> ```

This creates a `venv/` folder inside your project. It only needs to be created **once**.

---

## 7. Activate the Virtual Environment

You must activate the venv **every time** you open a new terminal to work on this project.

### Windows — PowerShell
```powershell
.\venv\Scripts\Activate.ps1
```

### Windows — Command Prompt
```cmd
venv\Scripts\activate.bat
```

### macOS / Linux
```bash
source venv/bin/activate
```

**How to confirm it's active:**
Your terminal prompt will change to show `(venv)` at the start:
```
(venv) PS C:\Projects\FinalMealmate>
```

> **If PowerShell shows an error about execution policy**, run this once:
> ```powershell
> Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
> ```
> Then try activating again.

---

## 8. Install All Dependencies

With the venv active, run:

```bash
pip install -r requirements.txt
```

This installs all 14 required packages. Expected output ends with:
```
Successfully installed Django-6.0.3 asgiref-3.11.1 certifi-... ...
```

**Full list of what gets installed:**

| Package | Purpose |
|---------|---------|
| `Django 6.0.3` | Core web framework |
| `razorpay 2.0.1` | Payment gateway SDK |
| `whitenoise 6.8.2` | Serve static files in production |
| `gunicorn 23.0.0` | Production WSGI server |
| `python-decouple 3.8` | Load secrets from `.env` file |
| `requests 2.33.1` | HTTP client (used by Razorpay) |
| `asgiref 3.11.1` | Django async support |
| `sqlparse 0.5.5` | SQL formatting (Django internal) |
| `tzdata 2026.x` | Timezone data (Windows requirement) |
| `certifi`, `idna`, `urllib3`, `charset-normalizer`, `packaging` | Supporting libraries |

> **Do not skip this step.** Running the project without dependencies will cause an `ImportError` immediately.

---

## 9. Configure Settings for Local Development

The project ships with **production settings** which will not work locally without changes.

Open the file `meal_buddy/settings.py` in any text editor (Notepad, VS Code, etc.) and make **three changes**:

### Change 1 — Secret Key

Find:
```python
SECRET_KEY = 'your-secure-secret-key-here-change-this'
```
Replace with any long random string:
```python
SECRET_KEY = 'local-dev-any-long-random-string-goes-here-1234567890'
```

### Change 2 — Debug Mode

Find:
```python
DEBUG = False
```
Replace with:
```python
DEBUG = True
```

### Change 3 — Allowed Hosts

Find:
```python
ALLOWED_HOSTS = ['yourusername.pythonanywhere.com']
```
Replace with:
```python
ALLOWED_HOSTS = ['127.0.0.1', 'localhost']
```

**Save the file.**

> **Why are these changes needed?**
> - `DEBUG = False` with a misconfigured host shows a blank white error page instead of your app.
> - `ALLOWED_HOSTS` restricts which domain names Django responds to. `127.0.0.1` is your local machine's address.
> - The `SECRET_KEY` placeholder is intentionally invalid for production — it must be replaced locally too.

---

## 10. Set Up the Database

### Run Migrations

This creates all the database tables inside `db.sqlite3`:

```bash
python manage.py migrate
```

Expected output:
```
Operations to perform:
  Apply all migrations: admin, auth, contenttypes, delivery, sessions
Running migrations:
  Applying contenttypes.0001_initial... OK
  Applying auth.0001_initial... OK
  Applying delivery.0001_initial... OK
  ...
  Applying delivery.0005_cart... OK
  Applying sessions.0001_initial... OK
```

> If `db.sqlite3` already exists in the ZIP (it should), this command will confirm all migrations are already applied and exit cleanly. Either way is fine.

### Verify Database Health

```bash
python manage.py check
```

Expected:
```
System check identified no issues (0 silenced).
```

If you see any errors here, revisit Steps 8 and 9 before continuing.

---

## 11. Run the Development Server

```bash
python manage.py runserver
```

Expected output:
```
Watching for file changes with StatReloader
Performing system checks...

System check identified no issues (0 silenced).
Django version 6.0.3, using settings 'meal_buddy.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CTRL-BREAK.
```

The server is now running. **Leave this terminal open** — closing it stops the server.

> To use a different port (e.g. if 8000 is already in use):
> ```bash
> python manage.py runserver 8080
> ```

---

## 12. Open the App in Your Browser

Open any web browser and go to:

```
http://127.0.0.1:8000/
```

You should see the **Meal Buddy** landing page with orange branding and Sign Up / Sign In buttons.

### All Available Pages

| URL | Page |
|-----|------|
| `http://127.0.0.1:8000/` | Landing page |
| `http://127.0.0.1:8000/open_signin` | Login |
| `http://127.0.0.1:8000/open_signup` | Register new account |
| `http://127.0.0.1:8000/admin/` | Django admin panel |

---

## 13. Login Credentials

### App Admin (Restaurant Management)
Login at `http://127.0.0.1:8000/open_signin`

| Field | Value |
|-------|-------|
| Username | `admin` |
| Password | `123` |

Use this account to **add restaurants**, **manage menus**, and **delete entries**.

### Django Admin Panel
Login at `http://127.0.0.1:8000/admin/`

| Field | Value |
|-------|-------|
| Username | `admin` |
| Password | `Admin@1234` |

Use this panel to **view and edit the database directly**.

### Sample Customer Account

| Field | Value |
|-------|-------|
| Username | `rajat` |
| Password | *(check the database or create a new account via Sign Up)* |

> **To create your own customer account:** go to `http://127.0.0.1:8000/open_signup` and fill in the form.

---

## 14. Stopping the Server

Go back to the terminal where `runserver` is running and press:

```
Ctrl + C
```

The server will shut down. Your data in `db.sqlite3` is preserved.

---

## 15. Common Errors and Fixes

---

### ❌ `'python' is not recognized as an internal or external command`

**Cause:** Python was not added to PATH during installation.

**Fix (Windows):**
1. Search for **"Edit the system environment variables"** in the Start menu
2. Click **Environment Variables**
3. Under **System Variables**, find `Path` → Edit → New
4. Add: `C:\Users\<YourName>\AppData\Local\Programs\Python\Python312\`
5. Add: `C:\Users\<YourName>\AppData\Local\Programs\Python\Python312\Scripts\`
6. Click OK, close and reopen your terminal

---

### ❌ `.\venv\Scripts\Activate.ps1 cannot be loaded because running scripts is disabled`

**Cause:** PowerShell execution policy blocks scripts.

**Fix:**
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```
Then activate again.

---

### ❌ `ModuleNotFoundError: No module named 'django'`

**Cause:** Virtual environment is not active, or packages weren't installed.

**Fix:**
```bash
# Step 1 — activate the venv
.\venv\Scripts\Activate.ps1        # Windows PowerShell
source venv/bin/activate           # macOS / Linux

# Step 2 — install packages
pip install -r requirements.txt
```

---

### ❌ `DisallowedHost at /` — Invalid HTTP_HOST header

**Cause:** `ALLOWED_HOSTS` in settings.py does not include `127.0.0.1`.

**Fix:** Open `meal_buddy/settings.py` and set:
```python
ALLOWED_HOSTS = ['127.0.0.1', 'localhost']
```

---

### ❌ `django.db.utils.OperationalError: no such table`

**Cause:** Migrations have not been run.

**Fix:**
```bash
python manage.py migrate
```

---

### ❌ `Error: That port is already in use`

**Cause:** Something else (or another Django instance) is using port 8000.

**Fix:** Use a different port:
```bash
python manage.py runserver 8080
```
Then open `http://127.0.0.1:8080/` in your browser.

---

### ❌ Page loads but looks completely unstyled (no CSS)

**Cause:** Internet connection issue — Bootstrap and fonts are loaded from a CDN.

**Fix:** Make sure you have an active internet connection and refresh the page.

---

### ❌ `SyntaxError` or `IndentationError` in settings.py

**Cause:** A copy-paste error while editing `settings.py`.

**Fix:** Open the file carefully and check the lines you edited. Python is sensitive to indentation (spaces/tabs). Do not mix them.

---

## 16. Quick-Start Cheat Sheet

Once you have done the full setup once, every subsequent time you just need these commands:

```bash
# 1. Navigate to the project folder
cd path/to/FinalMealmate

# 2. Activate the virtual environment
.\venv\Scripts\Activate.ps1          # Windows PowerShell
source venv/bin/activate             # macOS / Linux

# 3. Start the server
python manage.py runserver

# 4. Open browser
#    http://127.0.0.1:8000/
```

That's it. Three commands and you're running.

---

## Folder Quick Reference

```
FinalMealmate/
├── manage.py              ← Django CLI — always run commands from here
├── requirements.txt       ← All Python packages needed
├── db.sqlite3             ← Database file (contains all data)
├── SETUP.md               ← This setup guide
├── README.md              ← Full project documentation
│
├── meal_buddy/
│   └── settings.py        ← Edit DEBUG, ALLOWED_HOSTS, SECRET_KEY here
│
└── delivery/
    ├── models.py          ← Database structure
    ├── views.py           ← Page logic
    ├── urls.py            ← URL routes
    └── templates/         ← HTML pages
        └── delivery/
```

---

> **Still stuck?** Re-read the step that failed, check the error message against Section 15,
> and make sure the virtual environment is active `(venv)` before running any command.
