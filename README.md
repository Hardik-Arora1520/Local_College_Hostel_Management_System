# Hostel Mess Feedback System

A web-based feedback and rating system for hostel mess/food services. Students can log in, rate meals, and submit feedback; admins can view all feedback, reply to it, and manage responses.

Built with a **Flask + SQLite** backend and a single-page **HTML/CSS/JS** frontend.

## Features

- 🔐 JWT-based authentication (register/login)
- 🏠 Hostel selection (Boys'/Girls' hostels with warden info)
- ⭐ Meal feedback submission (food rating, menu rating, comments)
- 🛠️ Admin dashboard — view all feedback and reply to students
- 🍽️ Sample food ratings (breakfast/lunch/dinner/snacks)
- 🌗 Light/dark theme toggle (frontend)

## Tech Stack

| Layer      | Tech                                  |
|------------|----------------------------------------|
| Backend    | Python, Flask, Flask-CORS              |
| Auth       | PyJWT, Werkzeug (password hashing)     |
| Database   | SQLite                                 |
| Frontend   | HTML, CSS, JavaScript (single page)    |

## Project Structure

```
Hostel_Food_Management/
├── Server/
│   ├── server.py       # Flask app, routes, JWT auth
│   ├── models.py       # DB helper functions
│   ├── config.py       # DB path & JWT config (env-based)
│   ├── init_db.py      # Creates tables, seeds admin + sample food ratings
│   ├── add_user.py     # CLI script to add a user manually
│   └── __init__.py
├── Static/
│   └── index.Html      # Frontend (login, dashboard, feedback UI)
├── mess.db              # SQLite database (generated)
└── requirements.txt
```

## Setup

### 1. Clone and create a virtual environment

```bash
git clone <your-repo-url>
cd Hostel_Food_Management
python -m venv venv
```

Activate it:
- **Windows**: `venv\Scripts\activate`
- **macOS/Linux**: `source venv/bin/activate`

### 2. Install dependencies

```bash
pip install -r requirements.txt
```

### 3. Initialize the database

```bash
cd Server
python init_db.py
```

This creates `mess.db` with the required tables and seeds:
- Sample food ratings
- A default admin account:
  - **Email**: `admin@tits.ac.in`
  - **Password**: `Admin@123`

  > ⚠️ Change this password immediately in a real deployment.

### 4. Run the server

```bash
python server.py
```

The app will be available at **http://localhost:5000**.

## Environment Variables

The app reads JWT config from the environment (see `config.py`):

| Variable            | Default                          | Description                  |
|---------------------|-----------------------------------|-------------------------------|
| `MESS_JWT_SECRET`   | `change_this_random_secret_now`  | Secret used to sign JWTs      |
| `JWT_EXP_DAYS`      | `7`                                | Token expiry, in days         |

> ⚠️ **Security note**: `server.py` currently has its own hardcoded `JWT_SECRET`, separate from `config.py`. Before deploying, make sure `server.py` reads the secret from `config.py`/environment instead of using a hardcoded value.

## API Endpoints

| Method | Endpoint                        | Auth required | Description                          |
|--------|----------------------------------|:-------------:|----------------------------------------|
| GET    | `/api/hostels`                  | ❌            | List hostels and wardens               |
| POST   | `/api/register`                 | ❌            | Register a new user                    |
| POST   | `/api/login`                    | ❌            | Log in, returns JWT                    |
| GET    | `/api/me`                       | ✅            | Get current user's profile             |
| POST   | `/api/feedback`                 | ✅            | Submit feedback for a meal             |
| GET    | `/api/feedback`                 | ✅            | Get feedback (own, or all if admin)    |
| DELETE | `/api/feedback/<id>`            | ✅            | Delete feedback (owner or admin)       |
| POST   | `/api/feedback/<id>/reply`      | ✅ (admin)    | Reply to a piece of feedback           |

## Adding Users Manually

```bash
cd Server
python add_user.py
```

Follow the interactive prompts to create a new user (with optional hostel/room info and admin flag).

## Notes / Known Issues

- The `.gitignore` should exclude `venv/`, `__pycache__/`, and ideally `mess.db` (it's a generated file with user data).
- `server.py` and `config.py` both define JWT settings — consolidate to one source of truth before production use.

## License
