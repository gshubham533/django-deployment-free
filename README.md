# Deploy Your Django Projects For Free

A step-by-step guide on deploying Django projects for free.

---

## 📌 Prerequisites

- Ensure you have Python installed (preferably Python 3.x).
- Basic knowledge of Django and virtual environments.

If you already have a Django project set up, start from **Step 6**.

---

## 🚀 Setting Up Your Django Project

### Step 1: Create a Virtual Environment
```bash
python3 -m venv env
```

### Step 2: Activate the Virtual Environment
```bash
source env/bin/activate  # On macOS/Linux
env\Scripts\activate     # On Windows
```

### Step 3: Install Django
```bash
pip install django
```

### Step 4: Create a Django Project
```bash
django-admin startproject <project_name> .  # The `.` places files in the current directory
```

### Step 5: Run the Django Development Server
```bash
python3 manage.py runserver
```
Visit: [http://127.0.0.1:8000](http://127.0.0.1:8000)

---

## 📁 Configuring Static Files

### Step 6: Update `settings.py`
Add the following lines at the bottom:
```python
STATICFILES_DIRS = [
    BASE_DIR / "static",
]
STATIC_ROOT = BASE_DIR / "staticfiles"
```

### Step 7: Install Dependencies
```bash
pip install gunicorn whitenoise
```

### Step 8: Update `settings.py` to Use WhiteNoise
Modify the following sections:

#### 🔹 `ALLOWED_HOSTS` (For now, allow all hosts)
```python
ALLOWED_HOSTS = ['*']
```

#### 🔹 `INSTALLED_APPS`
```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'whitenoise.runserver_nostatic',  # Added WhiteNoise
    'django.contrib.staticfiles',
]
```

#### 🔹 `MIDDLEWARE`
```python
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'whitenoise.middleware.WhiteNoiseMiddleware',  # Added WhiteNoise middleware
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]
```

#### 🔹 Enable WhiteNoise Compression & Caching
```python
WHITENOISE_USE_FINDERS = True
STATICFILES_STORAGE = 'whitenoise.storage.CompressedManifestStaticFilesStorage'
```

---

## 🔄 Collect Static Files & Run Server

### Step 9: Collect Static Files
```bash
python manage.py collectstatic
```

### Step 10: Run Gunicorn to Test Deployment Readiness
```bash
gunicorn <project_name>.wsgi
```
Visit: [http://127.0.0.1:8000/admin](http://127.0.0.1:8000/admin)

---

## 📌 Preparing for Deployment

### Step 11: Generate `requirements.txt`
```bash
pip freeze > requirements.txt
```

### Step 12: Create a `.gitignore` File
Add the following to `.gitignore` to avoid pushing unnecessary files:
```gitignore
*.pyc
env/
staticfiles/
```

---

## 🚀 Deploying on Render.com

### Step 1: Visit [Render.com](https://render.com)

### Step 2: Login/Signup

### Step 3: Click on `New` → `Web Service`

### Step 4: Connect Your GitHub/GitLab/Bitbucket Repository
- If your repository is public, select `Public Git Repository` and enter your repo link.

### Step 5: Configure the Render Project
```bash
Root Directory: `.`
Build Command: `pip install -r requirements.txt && python3 manage.py collectstatic --noinput && python3 manage.py makemigrations && python3 manage.py migrate`
Start Command: `gunicorn <project_name>.wsgi`
Instance Type: Free
```

### Step 6: Click `Deploy Web Service`

### 🎉 Final Step: Your Project is Live!
Once deployed, visit the public URL provided by Render.

**Note:** Every push to the `main` branch triggers an automatic deployment.

---

## 🎯 Additional Notes
- If using a database like PostgreSQL, ensure proper configuration in `settings.py`.
- For production, consider adding environment variables for sensitive settings.
- Enable Django security settings before public deployment.

Happy Coding! 🚀

