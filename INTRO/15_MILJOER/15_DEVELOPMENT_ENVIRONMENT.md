# DEVELOPMENT ENVIRONMENT - Commander-and-Agent

**Project:** Commander-and-Agent (Documentation Framework)
**Version:** 2.0.0
**Status:** DOKUMENTATION 100% - Implementation 0%
**Last Updated:** 2025-12-29
**Created:** 2025-12-24

---

## OVERVIEW

Development environment setup for the Commander-and-Agent system supporting 25 AI Commanders across Python and TypeScript platforms.

**Status:** Currently in documentation phase only. No development server is running. All setup is preparation for Phase 1 implementation.

---

## SYSTEM REQUIREMENTS

### Minimum Specifications

| Component | Requirement | Recommended |
|-----------|-------------|-------------|
| CPU | Quad-core | 8+ cores |
| RAM | 8GB | 16GB+ |
| Storage | 50GB SSD | 100GB+ SSD |
| Network | 10Mbps | 100Mbps+ |
| OS | Linux/macOS/Windows 11 | Ubuntu 22.04 LTS |

### Operating System Support

| OS | Version | Status | Notes |
|----|---------|--------|-------|
| Ubuntu | 22.04 LTS+ | Recommended | Primary dev environment |
| macOS | 12.0+ (Intel/M1+) | Supported | Development tested |
| Windows | 11 Pro/Enterprise | Supported | WSL2 recommended |

---

## REQUIRED TOOLS & VERSIONS

### Python Environment

```bash
# Primary Requirements
Python 3.12.x          - Language runtime
pip 24.0+              - Package manager
virtualenv 20.25+      - Environment isolation
poetry 1.7+            - Dependency management

# Recommended Versions as of 2025-12-29
Python 3.12.1 or later
pip 24.0 or later
```

### JavaScript/Node Environment

```bash
# Primary Requirements
Node.js 18.19+         - JavaScript runtime
npm 10.0+              - Package manager
yarn 4.0+              - Alternative package manager (optional)
npx                    - Package executor

# Recommended Versions as of 2025-12-29
Node.js 20.11 LTS or later
npm 10.2 or later
```

### Container Platform

```bash
# Docker
Docker Desktop 4.25+   - Container runtime
Docker Compose 2.20+   - Multi-container orchestration

# Verified Versions
Docker 25.0+
Docker Compose 2.24+
```

### Development Tools

```bash
Git 2.40+              - Version control
Visual Studio Code     - IDE (recommended)
Postman 11.0+          - API testing
```

---

## LOCAL DEVELOPMENT SETUP

### Step 1: Clone Repository

```bash
git clone https://github.com/opnureyes2-del/commander-and-agent.git
cd commander-and-agent
```

### Step 2: Python Environment Setup

```bash
# Create virtual environment
python3.12 -m venv venv

# Activate virtual environment
# On Linux/macOS:
source venv/bin/activate

# On Windows (PowerShell):
.\venv\Scripts\Activate.ps1

# Verify activation (should show (venv) prefix)
python --version  # Should show Python 3.12.x
```

### Step 3: Install Python Dependencies

```bash
# Upgrade pip
pip install --upgrade pip setuptools wheel

# Install project dependencies
pip install -r requirements.txt

# For development:
pip install -r requirements-dev.txt
```

### Step 4: Node.js Environment Setup

```bash
# Install Node.js dependencies
npm install

# Verify installation
node --version
npm --version
```

### Step 5: Docker Setup

```bash
# Start Docker Desktop (GUI)
# Or on Linux:
sudo systemctl start docker

# Verify Docker
docker --version
docker compose --version

# Run initial compose setup
docker compose up -d
```

### Step 6: Configuration Files

```bash
# Copy example environment files
cp .env.example .env
cp .env.example.local .env.local

# Edit .env with local settings
# Key variables:
# - DATABASE_URL=postgresql://user:pass@localhost:5432/commander
# - REDIS_URL=redis://localhost:6379
# - OPENAI_API_KEY=your_key_here
# - OLLAMA_HOST=http://localhost:11434
```

---

## ENVIRONMENT VARIABLES

### Core Configuration

| Variable | Type | Required | Default | Purpose |
|----------|------|----------|---------|---------|
| `DEBUG` | boolean | No | false | Debug mode flag |
| `LOG_LEVEL` | string | No | INFO | Logging level |
| `PORT` | integer | No | 8000 | FastAPI port |
| `HOST` | string | No | 0.0.0.0 | FastAPI host |

### Database Configuration

| Variable | Type | Required | Default | Purpose |
|----------|------|----------|---------|---------|
| `DATABASE_URL` | string | Yes | N/A | PostgreSQL connection |
| `DATABASE_POOL_SIZE` | integer | No | 10 | Connection pool size |
| `DATABASE_MAX_OVERFLOW` | integer | No | 20 | Max overflow connections |

### Redis Configuration

| Variable | Type | Required | Default | Purpose |
|----------|------|----------|---------|---------|
| `REDIS_URL` | string | No | redis://localhost:6379 | Redis connection |
| `REDIS_DB` | integer | No | 0 | Redis database number |
| `CACHE_TTL` | integer | No | 3600 | Cache time-to-live (seconds) |

### LLM Configuration

| Variable | Type | Required | Default | Purpose |
|----------|------|----------|---------|---------|
| `OPENAI_API_KEY` | string | No | N/A | OpenAI API key |
| `OLLAMA_HOST` | string | No | http://localhost:11434 | Ollama endpoint |
| `OLLAMA_MODEL` | string | No | llama3:8b | Default Ollama model |
| `GEMINI_API_KEY` | string | No | N/A | Google Gemini key |
| `GROQ_API_KEY` | string | No | N/A | Groq API key |

### Search Engine Configuration

| Variable | Type | Required | Default | Purpose |
|----------|------|----------|---------|---------|
| `TAVILY_API_KEY` | string | No | N/A | Tavily research API |
| `EXA_API_KEY` | string | No | N/A | Exa semantic search |
| `BRAVE_SEARCH_KEY` | string | No | N/A | Brave search API |

### Speech Configuration

| Variable | Type | Required | Default | Purpose |
|----------|------|----------|---------|---------|
| `WHISPER_MODEL` | string | No | base | Whisper model size |
| `USE_FASTER_WHISPER` | boolean | No | false | Use faster-whisper |

---

## PORT ASSIGNMENTS

### Development Environment Port Mapping

| Service | Port | Protocol | Status | Notes |
|---------|------|----------|--------|-------|
| FastAPI Backend | 8000 | HTTP | Development | Main API server |
| React Frontend | 3000 | HTTP | Development | Frontend server |
| Postgres | 5432 | TCP | Docker | Local database |
| Redis | 6379 | TCP | Docker | Cache/sessions |
| Ollama | 11434 | HTTP | Optional | Local LLM |
| pgAdmin | 5050 | HTTP | Docker | DB administration |

### Verification Commands

```bash
# Check all ports in use
lsof -i -P -n | grep LISTEN

# Check specific service
netstat -tlnp | grep 8000
```

---

## DATABASE INITIALIZATION

### Using Docker Compose

```bash
# Start PostgreSQL container
docker compose up -d postgres

# Wait for database to be ready
docker compose exec postgres pg_isready -U commander

# Initialize schema
docker compose exec postgres psql -U commander -d commander -f /docker-entrypoint-initdb.d/schema.sql

# Verify initialization
docker compose exec postgres psql -U commander -d commander -c "\dt"
```

### Manual Setup (if needed)

```bash
# Connect to database
psql postgresql://commander:commander_pass@localhost:5432/commander

# Run initialization scripts
\i schema.sql

# Verify tables
\dt

# Exit
\q
```

---

## DEVELOPMENT SERVER STARTUP

### Quick Start (All Services)

```bash
# Start all Docker services
docker compose up -d

# Install Python dependencies
source venv/bin/activate
pip install -r requirements.txt

# Install Node dependencies
npm install

# Run development server
python -m uvicorn main:app --reload --host 0.0.0.0 --port 8000
```

### Individual Service Startup

```bash
# Terminal 1: Database & Cache
docker compose up -d postgres redis

# Terminal 2: Python backend
source venv/bin/activate
uvicorn main:app --reload --port 8000

# Terminal 3: Frontend
npm run dev

# Terminal 4: Ollama (if local LLM desired)
ollama serve
```

### Health Checks

```bash
# Check API health
curl http://localhost:8000/health

# Check database
curl http://localhost:8000/db/health

# Check Redis
redis-cli ping

# Check Ollama (if running)
curl http://localhost:11434/api/status
```

---

## IDE CONFIGURATION

### Visual Studio Code

Recommended extensions:
- `Python` (Microsoft)
- `Pylance` (Microsoft) - Python language server
- `isort` (Microsoft) - Import sorting
- `Prettier` (Esben Petersen) - Code formatting
- `ESLint` (Microsoft) - JavaScript linting
- `Docker` (Microsoft) - Docker support
- `REST Client` (Huachao Mao) - API testing
- `SQLTools` (Matheus Teixeira) - Database tools

Settings file `.vscode/settings.json`:

```json
{
  "python.defaultInterpreterPath": "${workspaceFolder}/venv/bin/python",
  "python.linting.enabled": true,
  "python.linting.pylintEnabled": true,
  "python.formatting.provider": "black",
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "[python]": {
    "editor.defaultFormatter": "ms-python.python",
    "editor.formatOnSave": true
  }
}
```

---

## CODE QUALITY TOOLS

### Python Code Quality

```bash
# Format code
black src/

# Sort imports
isort src/

# Lint code
flake8 src/

# Type checking
mypy src/

# All checks together
make lint
```

### JavaScript Code Quality

```bash
# Format code
prettier --write .

# Lint code
eslint src/

# All checks
npm run lint
```

---

## TESTING SETUP

### Python Testing

```bash
# Run all tests
pytest

# Run with coverage
pytest --cov=src

# Run specific test
pytest tests/unit/test_commanders.py

# Run with verbose output
pytest -v
```

### JavaScript Testing

```bash
# Run all tests
npm test

# Run with coverage
npm run test:coverage

# Run specific test file
npm test -- test/commanders.test.ts
```

---

## TROUBLESHOOTING

### Common Issues

| Issue | Symptom | Solution |
|-------|---------|----------|
| Port already in use | `Address already in use` | `lsof -i :8000` then kill process |
| Python version mismatch | `ModuleNotFoundError` | Verify `python --version` is 3.12+ |
| Database connection failed | `connection refused` | Ensure Docker container running |
| Virtual env not activated | `pip not found` | Run `source venv/bin/activate` |
| Node modules missing | `Cannot find module` | Run `npm install` |
| Docker not running | `Cannot connect to Docker` | Start Docker Desktop |

### Log Checking

```bash
# View Docker logs
docker compose logs -f postgres

# View FastAPI logs
# Logs appear in terminal where uvicorn is running

# View application logs
tail -f logs/application.log
```

---

## PERFORMANCE OPTIMIZATION

### Development Mode Performance

| Setting | Value | Impact |
|---------|-------|--------|
| Auto-reload | Enabled | Slower startup, faster iteration |
| Debug mode | True | More verbose logging |
| Pool size | 10 | Adequate for development |
| Cache | Enabled | Faster repeated requests |

### Production Mode Performance

| Setting | Value | Impact |
|---------|-------|--------|
| Auto-reload | Disabled | Fast startup |
| Debug mode | False | Minimal overhead |
| Pool size | 20+ | Handles concurrent requests |
| Cache | Enabled & optimized | Maximum efficiency |

---

## ÆNDRINGSLOG

| Dato | Tid | Handling | Af |
|------|-----|----------|-----|
| 2026-01-01 | 23:50 | 15_DEVELOPMENT_ENVIRONMENT.md oprettet | Kv1nt |

---

**Document Status:** READY FOR IMPLEMENTATION
**Authority:** Development Team
**Next Milestone:** Verify all tools installed before Phase 1
