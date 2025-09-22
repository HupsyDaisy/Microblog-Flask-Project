# Microblog Project

This repository follows along with [The Flask Mega-Tutorial](https://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-i-hello-world) by Miguel Grinberg.  
The project is a simple microblog application built step by step, with each chapter introducing new Flask concepts.

---

## Chapters Completed

### Chapter 1: Hello, World!
- Created a basic Flask application.
- Defined routes (`/` and `/index`).
- Rendered dynamic content using templates.
- Introduced Jinja2 template engine for loops, conditionals, and variables.

### Chapter 2: Templates
- Added a base layout (`base.html`) to avoid repetition in templates.
- Extended base layout in `index.html` and `login.html`.
- Passed variables (user, posts, title) from Flask routes to templates.
- Displayed multiple posts dynamically.

### Chapter 3: Web Forms
- Installed **Flask-WTF** for form handling.
- Created a `LoginForm` class in `forms.py`.
- Integrated the login form in `login.html`.
- Handled form validation and submitted data in `routes.py`.
- Added CSRF protection with a secret key.

### Chapter 4: Database (SQLAlchemy + Migrations)

Added Flask-SQLAlchemy models (User, Post) and relationships.

Configured the database (SQLite by default) via SQLALCHEMY_DATABASE_URI.

Introduced Flask-Migrate (Alembic) to version-control schema changes.

Typical migration workflow:

   ```bash
flask db init        # only once, to create migrations/ folder
flask db migrate -m "Initial tables"
flask db upgrade     # apply to the database
   ```

Version control tip: commit new migration scripts:

   ```bash
git add migrations/versions/<revision>.py
git commit -m "Add migration: <short description>"
   ```
The database file (e.g., app.db) should not be committed—see .gitignore below.

### Chapter 5: User Logins (Flask-Login)

Installed and configured Flask-Login.

Implemented secure password hashing with werkzeug.security.

Added login_user() / logout_user() routes and a @login_required decorator on protected pages.

Implemented a user_loader so Flask-Login can load users from the DB:

   ```python
# app/models.py
from flask_login import UserMixin
from app import db, login

class User(UserMixin, db.Model):
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(64), index=True, unique=True)
    email = db.Column(db.String(120), index=True, unique=True)
    password_hash = db.Column(db.String(256))

@login.user_loader
def load_user(id):
    return db.session.get(User, int(id))
   ```

Safe “next” redirects after login (only allow relative URLs):

   ```python

from urllib.parse import urlsplit
from flask import request, redirect, url_for

next_page = request.args.get('next')
if not next_page or urlsplit(next_page).netloc != '':
    next_page = url_for('index')
return redirect(next_page)
   ```

---

## How to Run

1. Clone the repository:
```bash
git clone https://github.com/HupsyDaisy/Microblog-Flask-Project.git
cd microblog
```

2. Create and activate a virtual environment:

```bash
python3 -m venv venv
source venv/bin/activate
On Windows: venv\Scripts\activate
```

3. Install dependencies:

```bash
pip install -r requirements.txt
```

4. Set environment variables:

This project uses a `.flaskenv` file (loaded by python-dotenv) to define environment variables for Flask.  
The file should be placed in the project root and can look like this:

```ini
FLASK_APP=microblog.py
```
5. Initialize the database and run migrations

```bash
flask db init        # first time only
flask db migrate -m "Initial tables"
flask db upgrade
```

6. Run the application:

```bash
flask run
```

Open http://localhost:5000
 in your browser.

## Project Structure

   ```bash
microblog/
│
├── app/
│   ├── __init__.py        # App factory / extensions (db, migrate, login)
│   ├── models.py          # SQLAlchemy models (User, Post)
│   ├── routes.py          # URL routes (index, login, logout, etc.)
│   ├── forms.py           # Web forms (Flask-WTF)
│   └── templates/         # HTML templates
│       ├── base.html
│       ├── index.html
│       └── login.html
│
├── migrations/            # Alembic migration scripts (commit these!)
├── microblog.py           # App entry point
├── requirements.txt       # Python dependencies
├── .flaskenv              # FLASK_APP=microblog.py, etc.
├── README.md              # This file
└── venv/                  # Virtual environment (not committed)
   ```

