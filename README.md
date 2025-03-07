# django-deployment-free


If you have already have django project setup and then you can start from `Step 6`

Step 1: Create an virtual environment (env)
Cmd: `python3 -m venv env`

Step 2: Activate the env
Cmd: `source env/bin/activate`

Step 3: Install Django
Cmd: `pip install django`

Step 4: Create Django project
Cmd: `django-admin startproject <project_name> .` # `.` creates all django files in current directory

Step 5: Check if Django server is running
Cmd: `python3 manage.py runserver`

Step 6: Now configure static files setting in `settings.py`. Add below lines in `settings.py` at last
```
STATICFILES_DIRS = [
    BASE_DIR / "static",
]

STATIC_ROOT = BASE_DIR / "staticfiles"
```

Step 7: Install 2 dependencies. Gunicorn (To run the wsgi server) and Whitenoise (To server static files)
Cmd: `pip install gunicorn whitenoise`

Step 8: Update `settings.py` to setup whitenoise
```
# -------------------------------------------------------

ALLOWED_HOSTS = ['*']

# -------------------------------------------------------
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'whitenoise.runserver_nostatic', # Added white noise in installed apps
    'django.contrib.staticfiles',
]

# -------------------------------------------------------

MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'whitenoise.middleware.WhiteNoiseMiddleware', # Added white noise in middleware
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]

# -------------------------------------------------------
# Enable WhiteNoise compression and caching
WHITENOISE_USE_FINDERS = True
STATICFILES_STORAGE = 'whitenoise.storage.CompressedManifestStaticFilesStorage'

# -------------------------------------------------------
```


Step 9: Collect all static files
Cmd: `python manage.py collectstatic`

Step 10: Run WSGI server and check if the static files are being loaded
Cmd: `gunicorn <project_name>.wsgi`

Visit: http://127.0.0.1:8000/admin

Step 11: Once everything is working fine stop the server and lets create a requirements.txt
Cmd: `pip freeze > requirements.txt`

Step 12: Create a `.gitignore` file and paste follwing files to ignore so that code stays clean and not irrelevant files are pushed to repo
.gitignore:```
*.pyc
env/
staticfiles/
```


## Here we have completed the django setup. Next would be the deployment part.

## Deployment on Render.com

Step 1: Visit render.com

Step 2: Login/Signup on the Platform

Step 3: Click on `New` and select `Web Service`

Step 4: Connect your Github/GitLab/Bitbucker account and selet the repo for deployment or if the repository is public then choose `Public Git Repository` option enter your repo link.

Step 5: Configuration of the project on render
```
Root Directory: `.`
Build Command: `pip install -r requirements.txt && python3 manage.py collectstatic --noinput && python3 manage.py makemigrations && python3 manage.py migrate`
Start Command: `gunicorn <project_name>.wsgi`
Instance Type: Free
```

Step 6: Click on `Deploy Web Service`

Wait for the deployment to be done. Once done

Visit Your Project public link provided by render and Your project is successfully deploy 🎉

Everytime you push on your `main` repo branch the project will get deployed automatically