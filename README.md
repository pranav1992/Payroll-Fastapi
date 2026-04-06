# Payroll FastAPI

Payroll FastAPI is a layered FastAPI service for managing users, tasks, task assignments, time logs, work logs, and monthly remittances. It uses SQLModel with SQLite and exposes REST endpoints with FastAPI docs.

**Features**
- User management
- Task catalog and task assignments
- Work logs and time logs (validated durations)
- Remittance calculation and payment workflows
- SQLite-backed persistence with auto table creation

**Tech Stack**
- FastAPI
- SQLModel + SQLite
- Pydantic v2
- Pytest

**Project Structure**
- `app/api` FastAPI routers and exception handling
- `app/application` service layer and facade
- `app/domain` Pydantic schemas and domain exceptions
- `app/infrastructure/db` SQLModel models, repositories, and DB setup
- `test` unit and API tests

**Setup**
Requirements:
- Python 3.9+

Install dependencies:
- `uv sync`
- `python -m pip install -e .`

**Run**
- `uv run uvicorn app.main:app --reload`
- `python main.py`

Swagger UI:
- `http://localhost:8000/docs`

ReDoc:
- `http://localhost:8000/redoc`

On startup the app creates `db.sqlite` in the project root.

**API Overview**
Users:
- POST `/users/create-user/` Create a user
- GET `/users/get-all-users/` List users

Tasks:
- POST `/tasks/create-task/` Create a task
- GET `/tasks/get-all-tasks/` List tasks
- POST `/tasks/create-task-assignment/` Assign a task to a user
- GET `/tasks/get-task-assignments/` Query assignments by `user_id`
- GET `/tasks/get-task-assignments/` Query assignments by `task_id`
- GET `/tasks/get-all-assigned-tasks/` List all assignments

Worklogs:
- POST `/worklogs/create-worklog/` Create a worklog
- GET `/worklogs/get-all-worklogs/` List worklogs
- GET `/worklogs/get-worklog/` Query a worklog
- GET `/worklogs/get-worklog-by-user-id/` List worklogs by user
- GET `/worklogs/get-worklog-by-task-id/` List worklogs by task
- GET `/worklogs/list-all-worklogs` List worklogs with amounts

Timelogs:
- POST `/time-log/create-time-log/` Create a timelog
- GET `/time-log/get-all-time-logs/` List timelogs

Remittances:
- POST `/remittances/calculate-month/` Preview remittance for a user
- POST `/remittances/pay-month/` Pay a user for a month
- POST `/remittances/generate-remittances-for-all-users` Bulk remittance

**Examples**
Create a user:
```json
{
  "name": "Aarav Sharma",
  "employee_id": "EMP"
}
```

Create a task:
```json
{
  "name": "Payroll Review",
  "description": "Monthly payroll verification"
}
```

Assign a task:
```json
{
  "user_id": "6e2d4c4f-86d2-4b8c-9ff2-bf3a83f69a1b",
  "task_id": "3d2d6c57-1c8d-4ed8-8b83-0d0f2f7b5a1a"
}
```

Create a worklog:
```json
{
  "user_id": "6e2d4c4f-86d2-4b8c-9ff2-bf3a83f69a1b",
  "task_id": "3d2d6c57-1c8d-4ed8-8b83-0d0f2f7b5a1a",
  "task_assignment_id": "f2a2f4e0-87f5-4c9f-8e9e-7f7c3b1d3a8c",
  "year": 2026,
  "month": 4
}
```

Create a timelog:
```json
{
  "task_id": "3d2d6c57-1c8d-4ed8-8b83-0d0f2f7b5a1a",
  "user_id": "6e2d4c4f-86d2-4b8c-9ff2-bf3a83f69a1b",
  "task_assignment_id": "f2a2f4e0-87f5-4c9f-8e9e-7f7c3b1d3a8c",
  "start_time": "2026-04-06T09:00:00Z",
  "end_time": "2026-04-06T13:00:00Z"
}
```

Preview a remittance:
```json
{
  "user_id": "6e2d4c4f-86d2-4b8c-9ff2-bf3a83f69a1b",
  "year": 2026,
  "month": 4,
  "rate_per_hour": "25.00"
}
```

Bulk remittance for all users:
```json
{
  "year": 2026,
  "month": 4,
  "rate_per_hour": "25.00"
}
```

**Tests**
- `pytest`

**Contributing**
- Keep changes focused and small; add tests for new behavior.
- Run `pytest` before opening a PR.
- Follow existing module boundaries:
  `app/api` for HTTP layer, `app/application` for services,
  `app/domain` for schemas, `app/infrastructure` for persistence.

**Development Workflow**
- Install dependencies: `uv sync`
- Run the app: `uv run uvicorn app.main:app --reload`
- Run tests: `pytest`

**Notes**
- Request/response models live in `app/domain/schema.py`.
- Database tables are defined in `app/infrastructure/db/models.py`.
