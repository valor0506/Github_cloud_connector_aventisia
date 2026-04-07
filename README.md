# GitHub Cloud Connector

A REST API built with **Python + FastAPI** that connects to GitHub's API to manage repositories and issues.

---

## Setup Instructions

### 1. Clone the repository
```bash
git clone https://github.com/YOUR_USERNAME/github-connector.git
cd github-connector
```

### 2. Create a virtual environment
```bash
python -m venv .venv
source .venv/bin/activate      # Mac/Linux
.venv\Scripts\activate         # Windows
```

### 3. Install dependencies
```bash
pip install -r requirements.txt
```

### 4. Configure your GitHub Token

1. Go to [GitHub → Settings → Developer Settings → Personal Access Tokens → Tokens (classic)](https://github.com/settings/tokens)
2. Generate a new token with **`repo`** scope (needed to create issues)
3. Copy the token

```bash
cp .env.example .env
# Open .env and paste your token:
# GITHUB_TOKEN=ghp_your_token_here
```

---

## How to Run

```bash
uvicorn app.main:app --reload
```

The server starts at: **http://localhost:8000**

- Interactive API docs: **http://localhost:8000/docs**
- Alternative docs:     **http://localhost:8000/redoc**

---

## API Endpoints

### `GET /`
Health check.

**Response:**
```json
{ "status": "ok", "message": "GitHub Connector is running" }
```

---

### `GET /repos?username={username}`
Fetch public repositories for a GitHub user or organization.

**Example:**
```bash
curl "http://localhost:8000/repos?username=torvalds"
```

**Response:** Array of repository objects with name, stars, language, etc.

---

### `GET /list-issues?owner={owner}&repo={repo}`
List open issues from a repository.

**Example:**
```bash
curl "http://localhost:8000/list-issues?owner=tiangolo&repo=fastapi"
```

---

### `POST /create-issue`
Create a new issue in a repository.

**Request body (JSON):**
```json
{
  "owner": "your-username",
  "repo":  "your-repo",
  "title": "Bug: something is broken",
  "body":  "Steps to reproduce..."
}
```

**Example:**
```bash
curl -X POST "http://localhost:8000/create-issue" \
  -H "Content-Type: application/json" \
  -d '{"owner":"your-username","repo":"your-repo","title":"Test issue","body":"Created via API"}'
```

**Response:** Created issue with number and URL.

---

## Tech Stack

| Layer       | Technology          |
|-------------|---------------------|
| Language    | Python 3.11+        |
| Framework   | FastAPI             |
| HTTP Client | httpx (async)       |
| Validation  | Pydantic v2         |
| Server      | Uvicorn (ASGI)      |
| Secrets     | python-dotenv       |

---

## Project Structure

```
github-connector/
├── app/
│   ├── __init__.py        # Makes 'app' a package
│   ├── config.py          # Loads environment variables
│   ├── github_client.py   # All GitHub API calls
│   ├── models.py          # Request/Response data shapes
│   └── main.py            # FastAPI routes (entry point)
├── .env.example           # Template for secrets
├── .gitignore             # Excludes .env from git
├── requirements.txt       # Python dependencies
└── README.md
```