# SETUP GUIDE - Commander-and-Agent

**Project:** Commander-and-Agent (Documentation Framework)
**Version:** 2.0.0
**Status:** DOKUMENTATION 100% KOMPLET
**Last Updated:** 2025-12-29
**Created:** 2025-12-24

---

## QUICK START (15 Minutes)

### Prerequisites Check
```bash
# Verify Python 3.12+
python3 --version

# Verify Node.js 18+
node --version

# Verify Docker
docker --version
docker-compose --version

# Verify Git
git --version
```

### Clone & Setup
```bash
# Clone repository
git clone <repository-url>
cd commander-and-agent

# Create Python virtual environment
python3.12 -m venv venv
source venv/bin/activate  # On Windows: .\venv\Scripts\Activate.ps1

# Install dependencies
pip install -r requirements.txt
npm install

# Start Docker services
docker-compose up -d

# Verify setup
python -m pytest tests/
npm test
```

---

## DETAILED SETUP (60 Minutes)

### Step 1: System Preparation (5 minutes)

**Windows Users:**
```bash
# Install Windows Subsystem for Linux 2 (WSL2) if not present
wsl --install

# Install Docker Desktop with WSL2 backend
# Download from https://www.docker.com/products/docker-desktop
```

**macOS Users:**
```bash
# Install Homebrew if needed
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Install Docker Desktop
brew install --cask docker
```

**Linux Users:**
```bash
# Ubuntu/Debian
sudo apt-get update
sudo apt-get install python3.12 python3.12-venv nodejs npm docker.io docker-compose

# Start Docker
sudo systemctl start docker
sudo usermod -aG docker $USER
```

### Step 2: Clone Repository (2 minutes)

```bash
# Clone the repository
git clone https://github.com/opnureyes2-del/commander-and-agent.git
cd commander-and-agent

# Verify directory structure
ls -la
# Should show: CLAUDE.md, ROADMAP.md, INTRO/, [25 Commander folders], etc.
```

### Step 3: Python Environment Setup (5 minutes)

```bash
# Create virtual environment
python3.12 -m venv venv

# Activate virtual environment
# On Linux/macOS:
source venv/bin/activate

# On Windows (PowerShell):
.\venv\Scripts\Activate.ps1

# On Windows (CMD):
venv\Scripts\activate.bat

# Verify activation (should show (venv) prefix)
python --version  # Should show Python 3.12.x
pip --version    # Should show pip 24.0+
```

### Step 4: Install Python Dependencies (5 minutes)

```bash
# Upgrade pip
pip install --upgrade pip setuptools wheel

# Install project dependencies
pip install -r requirements.txt

# Verify installation
python -c "import fastapi; print(f'FastAPI {fastapi.__version__} installed')"
```

### Step 5: Node.js Environment Setup (3 minutes)

```bash
# Verify Node.js installation
node --version  # Should show v18.19+
npm --version   # Should show 10.2+

# Install project dependencies
npm install

# Verify installation
npm list
```

### Step 6: Configuration Setup (5 minutes)

```bash
# Copy environment template
cp .env.example .env

# Edit .env file with your settings
nano .env  # or use your preferred editor

# Key variables to configure:
# - DATABASE_URL=postgresql://user:pass@localhost:5432/commander
# - REDIS_URL=redis://localhost:6379
# - OPENAI_API_KEY=your_key_here
# - OLLAMA_HOST=http://localhost:11434
```

### Step 7: Docker Setup (10 minutes)

```bash
# Verify Docker is running
docker --version
docker ps

# Start Docker services
docker-compose up -d

# Verify services are running
docker-compose ps
# All services should show "Up"

# Wait for database to be ready
docker-compose exec postgres pg_isready -U commander

# Initialize database schema
docker-compose exec backend python -m alembic upgrade head
```

### Step 8: Verification Testing (10 minutes)

```bash
# Test Python environment
python -m pytest tests/ -v

# Test Node environment
npm test

# Test API health
curl http://localhost:8000/health
# Should return {"status": "ok"}

# Test database connection
curl http://localhost:8000/db/health
# Should return database status

# Test Redis connection
docker-compose exec redis redis-cli ping
# Should return "PONG"
```

### Step 9: IDE Configuration (5 minutes)

**Visual Studio Code:**

```bash
# Open VS Code
code .

# Install recommended extensions (automatic prompt)
# Manually install if needed:
# - Python (Microsoft)
# - Pylance (Microsoft)
# - Prettier (Esben Petersen)
# - ESLint (Microsoft)
# - Docker (Microsoft)
```

**Create VS Code settings:**

Create `.vscode/settings.json`:
```json
{
  "python.defaultInterpreterPath": "${workspaceFolder}/venv/bin/python",
  "python.linting.enabled": true,
  "python.formatting.provider": "black",
  "editor.formatOnSave": true,
  "[python]": {
    "editor.defaultFormatter": "ms-python.python"
  }
}
```

---

## CONFIGURATION DEEP DIVE

### Environment Variables Explained

**Database Configuration**

```bash
# PostgreSQL connection string
DATABASE_URL=postgresql://user:password@postgres:5432/commander

# Connection pool settings
DATABASE_POOL_SIZE=10              # Number of connections
DATABASE_MAX_OVERFLOW=20           # Additional connections allowed
DATABASE_ECHO=false                # Log SQL queries
```

**Redis Configuration**

```bash
# Redis connection
REDIS_URL=redis://redis:6379
REDIS_DB=0                         # Database number (0-15)
CACHE_TTL=3600                     # Cache time-to-live in seconds
```

**LLM Configuration**

```bash
# Local LLM (Ollama)
OLLAMA_HOST=http://ollama:11434
OLLAMA_MODEL=llama3:8b

# Cloud LLMs
OPENAI_API_KEY=sk-...
GEMINI_API_KEY=AIza...
GROQ_API_KEY=gsk_...
```

**Application Settings**

```bash
# Logging
LOG_LEVEL=INFO                     # DEBUG, INFO, WARNING, ERROR
DEBUG=false                        # Enable debug mode

# Server
HOST=0.0.0.0                       # Bind address
PORT=8000                          # API port
WORKERS=4                          # Number of worker processes
```

---

## DEVELOPMENT SERVER STARTUP

### Local Development (All Services)

```bash
# Terminal 1: Start Docker services
docker-compose up -d

# Terminal 2: Run Python backend
source venv/bin/activate
uvicorn main:app --reload --host 0.0.0.0 --port 8000

# Terminal 3: Run React frontend
npm run dev

# Terminal 4: Monitor logs
docker-compose logs -f
```

### Access Points

| Service | URL | Purpose |
|---------|-----|---------|
| FastAPI | http://localhost:8000 | REST API |
| Docs | http://localhost:8000/docs | API documentation |
| Frontend | http://localhost:3000 | React application |
| pgAdmin | http://localhost:5050 | Database admin |
| Ollama | http://localhost:11434 | Local LLM |

### Health Checks

```bash
# Check all services
curl http://localhost:8000/health

# Check database
curl http://localhost:8000/db/health

# Check cache
curl http://localhost:8000/cache/health

# Check LLM
curl http://localhost:11434/api/status
```

---

## FIRST PROJECT TASKS

### Task 1: Explore Documentation (30 minutes)

```bash
# Read core documentation
cat CLAUDE.md                 # System overview
cat ROADMAP.md               # Implementation plan
cat AGENT_INDEX.md           # Commander reference

# Explore INTRO DNA
ls INTRO/
ls INTRO/10_ARKITEKTUR/
ls INTRO/15_MILJOER/
ls INTRO/20_PROJEKTER/
```

### Task 2: Explore Commander Structure (30 minutes)

```bash
# List all Commander folders
ls | grep -E "^[A-Z]_"

# Explore Chat Commander structure
ls -la Chat_Commander/
ls Chat_Commander/01_Planning/
cat Chat_Commander/01_Planning/ROLLE.md
```

### Task 3: Run Tests (15 minutes)

```bash
# Activate environment
source venv/bin/activate

# Run all tests
pytest tests/ -v

# Run specific test
pytest tests/unit/test_commanders.py -v

# Run with coverage
pytest --cov=src tests/
```

### Task 4: Code Exploration (30 minutes)

```bash
# Explore project structure
find . -type f -name "*.py" | head -20
find . -type f -name "*.ts" | head -20

# Read main entry point
cat main.py

# Check requirements
cat requirements.txt | head -20
```

---

## TROUBLESHOOTING COMMON ISSUES

### Issue: Python version mismatch

**Symptom:** `python3: command not found` or version not 3.12+

**Solution:**
```bash
# Install Python 3.12 on Ubuntu
sudo apt-get install python3.12 python3.12-venv

# On macOS
brew install python@3.12
brew unlink python
brew link python@3.12

# Then recreate virtual environment
rm -rf venv
python3.12 -m venv venv
source venv/bin/activate
```

### Issue: Docker not running

**Symptom:** `Cannot connect to Docker daemon`

**Solution:**
```bash
# Start Docker
# Windows/macOS: Open Docker Desktop

# Linux:
sudo systemctl start docker

# Add user to docker group
sudo usermod -aG docker $USER
# Log out and back in
```

### Issue: Port already in use

**Symptom:** `Address already in use: ('0.0.0.0', 8000)`

**Solution:**
```bash
# Find process using port
lsof -i :8000

# Kill process (if not needed)
kill -9 <PID>

# Or change port in .env
echo "PORT=8001" >> .env
```

### Issue: Database connection failed

**Symptom:** `connection refused` or `psycopg2.OperationalError`

**Solution:**
```bash
# Verify PostgreSQL is running
docker-compose ps postgres

# Check logs
docker-compose logs postgres

# Restart database
docker-compose restart postgres

# Wait 30 seconds for startup
sleep 30

# Test connection
docker-compose exec postgres psql -U commander -d commander -c "SELECT version();"
```

### Issue: Module not found

**Symptom:** `ModuleNotFoundError: No module named 'fastapi'`

**Solution:**
```bash
# Verify virtual environment activated
which python  # Should show venv path

# Reinstall dependencies
pip install -r requirements.txt

# Verify installation
python -c "import fastapi"
```

---

## NEXT STEPS AFTER SETUP

1. **Read Documentation**
   - Review INTRO DNA files in /INTRO folder
   - Understand system architecture
   - Study implementation roadmap

2. **Explore Codebase**
   - Navigate Commander folders
   - Review existing documentation
   - Understand folder structure

3. **Configure IDE**
   - Install recommended extensions
   - Configure Python interpreter
   - Setup linting and formatting

4. **Run Tests**
   - Execute unit tests
   - Verify all tests pass
   - Check code coverage

5. **Start Development**
   - Pick a Commander from Phase 1
   - Review ROLLE.md and TECH_SPEC.md
   - Begin implementation

---

## GETTING HELP

### Documentation Resources

- **CLAUDE.md** - System context and overview
- **INTRO/20_PROJEKTER/20_PROJECT_OVERVIEW.md** - Project scope
- **INTRO/10_ARKITEKTUR/13_TECH_STACK.md** - Technology choices
- **INTRO/15_MILJOER/15_DEVELOPMENT_ENVIRONMENT.md** - Environment details
- **INTRO/70_GUIDES/70_SETUP_GUIDE.md** - This file

### Online Resources

- FastAPI: https://fastapi.tiangolo.com/
- AGNO v2: [Documentation link]
- React Native: https://reactnative.dev/
- PostgreSQL: https://www.postgresql.org/docs/
- Docker: https://docs.docker.com/

### Getting Support

1. Check troubleshooting section above
2. Review error message carefully
3. Check Docker logs: `docker-compose logs <service>`
4. Check application logs: `tail -f logs/application.log`
5. Contact project lead with specific error message

---

## ÆNDRINGSLOG

| Dato | Tid | Handling | Af |
|------|-----|----------|-----|
| 2026-01-01 | 23:50 | 70_SETUP_GUIDE.md oprettet | Kv1nt |

---

**Document Status:** READY FOR USE
**Authority:** Development Team
**Audience:** New team members, developers
**Last Verified:** 2025-12-29
