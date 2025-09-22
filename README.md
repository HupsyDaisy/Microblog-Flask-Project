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

---

## How to Run

1. Clone the repository:
   ```bash
   git clone https://github.com/HupsyDaisy/Microblog-Flask-Project.git
   cd microblog

2. Create and activate a virtual environment:

python3 -m venv venv
source venv/bin/activate   # On Windows: venv\Scripts\activate


3. Install dependencies:

pip install -r requirements.txt


4. Set environment variables (Linux/macOS):

This project uses a `.flaskenv` file to define environment variables for Flask.  
The file should be placed in the project root and can look like this:

FLASK_APP=microblog.py


6. Run the application:

flask run

Open http://localhost:5000
 in your browser.

## Project Structure

microblog/
│
├── app/
│   ├── __init__.py       # Application factory
│   ├── routes.py         # URL routes
│   ├── forms.py          # Web forms (Flask-WTF)
│   └── templates/        # HTML templates
│       ├── base.html
│       ├── index.html
│       └── login.html
│
├── venv/                 # Virtual environment
├── microblog.py          # App entry point
├── requirements.txt      # Python dependencies
└── README.md             # This file