# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Development Commands

### Backend (FastAPI)
```bash
# Start backend server
cd backend
python main.py

# Or using uvicorn directly
cd backend
uvicorn main:app --reload --host 0.0.0.0 --port 8000
```

### Frontend (Streamlit)
```bash
# Start frontend application
cd frontend
streamlit run main.py
```

### Setup
```bash
# Install dependencies
pip install -r requirements.txt

# Optional: Create virtual environment
python -m venv venv
# Windows: venv\Scripts\activate
# Linux/Mac: source venv/bin/activate
```

## Architecture Overview

This is a full-stack todo application with clear separation between backend API and frontend UI:

### Backend Architecture
- **FastAPI application** with modular router design
- **JWT authentication** with bcrypt password hashing
- **JSON file database** (`../db/db.json`) shared between modules
- **Router separation**:
  - `api_user.py`: Authentication endpoints (/register, /login, /profile)
  - `api_todo.py`: CRUD operations (/todos, /todos/{id})
  - `main.py`: FastAPI app assembly and uvicorn server

### Frontend Architecture
- **Streamlit application** with screen-based navigation
- **Session state management** for authentication and user data
- **Modular screen design**:
  - `main.py`: App entry point and navigation logic
  - `auth_screen.py`: Login/register interface
  - `todo_screen.py`: Todo management interface
  - `profile_screen.py`: User profile and statistics
  - `api_client.py`: HTTP client for backend communication

### Data Flow
- Frontend communicates with backend via REST API calls
- JWT tokens stored in Streamlit session state for authentication
- User authorization enforced at API level using token verification
- Database operations use shared JSON file with user isolation

### Key Implementation Details
- **Database path**: Backend uses `../db/db.json` (relative to backend/ directory)
- **API Base URL**: Frontend hardcoded to `http://localhost:8000`
- **Authentication flow**: Login sets token in session state, all API calls include Bearer token
- **User isolation**: All todo operations filtered by user_id derived from JWT token

## API Endpoints

### Authentication
- `POST /register` - User registration
- `POST /login` - User login (returns JWT token)
- `GET /profile` - Get user profile (requires auth)

### Todos
- `GET /todos` - Get user's todos (requires auth)
- `POST /todos` - Create todo (requires auth)
- `GET /todos/{todo_id}` - Get specific todo (requires auth)
- `PUT /todos/{todo_id}` - Update todo (requires auth)
- `DELETE /todos/{todo_id}` - Delete todo (requires auth)