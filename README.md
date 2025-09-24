# Microblog Project

This repository follows along with [The Flask Mega-Tutorial](https://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-i-hello-world) by Miguel Grinberg.  The project is a simple microblog application built step by step, with each chapter introducing new Flask concepts.

---

## Chapters Completed

### Chapter 1: Hello, World!
- Created a basic Flask application.
- Defined routes (`/` and `/index`).
- Rendered dynamic content using templates.
- Jinja2 template engine for loops, conditionals, and variables.

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

- Added Flask-SQLAlchemy models (User, Post) and relationships.

- Configured the database (SQLite by default) via SQLALCHEMY_DATABASE_URI.

- Flask-Migrate (Alembic) to version-control schema changes.

Typical migration workflow:

   ```bash
flask db init        # only once, to create migrations/ folder
flask db migrate -m "Initial tables"
flask db upgrade     # apply to the database
   ```

### Chapter 5: User Logins (Flask-Login)

- Installed and configured Flask-Login.

- Implemented secure password hashing with werkzeug.security.

- Added login_user() / logout_user() routes and a @login_required decorator on protected pages.

- Implemented a user_loader so Flask-Login can load users from the DB:

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
### Chapter 7: Error Handling

- Added custom 404 and 500 error pages.

- Configured logging (e.g., rotating file or email on errors) for production diagnostics.

- Ensured DB session rollback on unhandled exceptions to keep the session clean.

### Chapter 8: Followers

- Implemented a self-referential followers relationship so users can follow/unfollow each other.

- Added routes to follow/unfollow and updated the user profile page to show follower/followed counts.

- Built a personalized feed that shows posts from the current user and people they follow.

- Kept a public Explore page showing all posts (most recent first).

### Chapter 9: Pagination

- Paginated post lists (index & explore) using db.paginate() with SQLAlchemy.

- Added Next / Previous links in templates, and a POSTS_PER_PAGE config setting.

### Chapter 10: Email

- Configured Flask-Mail.

- Implemented the password reset flow.

- Added user token helpers: get_reset_password_token() and verify_reset_password_token().

- Created routes /reset_password_request and /reset_password/<token>, plus email templates (txt/html).

- In development, the reset link can be logged to console instead of sending email; in production, use SMTP. If using blueprints, ensure endpoints are namespaced (e.g., auth.reset_password_request).

---

## How to Run

1. Clone the repository:
```bash
git clone https://github.com/HupsyDaisy/Microblog-Flask-Project.git
cd microblog
```

2. Create and activate a virtual environment:

```bash
python -m venv venv
source venv/bin/activate    #linux/macOS
venv\Scripts\activate       #windows
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
│   ├── __init__.py        # App factory / extensions 
│   ├── errors.py          # errors
│   ├── models.py          # SQLAlchemy models 
│   ├── routes.py          # Routes 
│   ├── forms.py           # Web forms 
│   └── templates/
│       ├── base.html
│       ├── index.html
│       ├── explore.html
│       ├── user.html
│       ├── edit_profile.html
│       ├── _post.html
│       ├── 404.html
│       └── 500.html
│
├── migrations/            # Alembic migration scripts 
├── microblog.py           # App entry point
├── requirements.txt       # Python dependencies
├── .flaskenv              # FLASK_APP=microblog.py, etc.
├── README.md              # This file
└── venv/                  # Virtual environment
   ```

