# DOCKER CONFIGURATION - Commander-and-Agent

**Project:** Commander-and-Agent (Documentation Framework)
**Version:** 2.0.0
**Status:** DOKUMENTATION 100% - Implementation 0%
**Last Updated:** 2025-12-29
**Created:** 2025-12-24

---

## OVERVIEW

Docker configuration and containerization strategy for the Commander-and-Agent system supporting 25 AI Commanders.

**Status:** Docker configuration documented. Containers not yet deployed. Implementation planned for Phase 1.

---

## DOCKER ARCHITECTURE

### Container Services

The system will use the following containerized services:

| Service | Image | Port | Volume | Network |
|---------|-------|------|--------|---------|
| PostgreSQL | postgres:15 | 5432 | postgres_data | backend |
| Redis | redis:7 | 6379 | redis_data | backend |
| FastAPI Backend | commander-backend:latest | 8000 | ./src | backend |
| React Frontend | commander-frontend:latest | 3000 | ./ui | frontend |
| pgAdmin | dpage/pgadmin4:latest | 5050 | pgadmin_data | backend |
| Ollama | ollama/ollama:latest | 11434 | ollama_data | backend |

---

## DOCKER COMPOSE CONFIGURATION

### Full docker-compose.yml Structure

```yaml
version: '3.9'

services:
  # PostgreSQL Database
  postgres:
    image: postgres:15
    container_name: commander-postgres
    environment:
      POSTGRES_USER: ${DB_USER:-commander}
      POSTGRES_PASSWORD: ${DB_PASSWORD:-commander_secure_pass}
      POSTGRES_DB: ${DB_NAME:-commander}
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data
      - ./docker/postgres/init.sql:/docker-entrypoint-initdb.d/schema.sql
    networks:
      - backend
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U ${DB_USER:-commander}"]
      interval: 10s
      timeout: 5s
      retries: 5
    restart: unless-stopped

  # Redis Cache
  redis:
    image: redis:7-alpine
    container_name: commander-redis
    ports:
      - "6379:6379"
    volumes:
      - redis_data:/data
      - ./docker/redis/redis.conf:/usr/local/etc/redis/redis.conf
    networks:
      - backend
    healthcheck:
      test: ["CMD", "redis-cli", "ping"]
      interval: 10s
      timeout: 5s
      retries: 5
    restart: unless-stopped
    command: redis-server /usr/local/etc/redis/redis.conf

  # pgAdmin Database GUI
  pgadmin:
    image: dpage/pgadmin4:latest
    container_name: commander-pgadmin
    environment:
      PGADMIN_DEFAULT_EMAIL: ${PGADMIN_EMAIL:-admin@commander.local}
      PGADMIN_DEFAULT_PASSWORD: ${PGADMIN_PASSWORD:-admin}
    ports:
      - "5050:80"
    volumes:
      - pgadmin_data:/var/lib/pgadmin
    networks:
      - backend
    depends_on:
      - postgres
    restart: unless-stopped

  # FastAPI Backend
  backend:
    build:
      context: .
      dockerfile: Dockerfile.backend
    container_name: commander-backend
    environment:
      DATABASE_URL: postgresql://${DB_USER:-commander}:${DB_PASSWORD:-commander_secure_pass}@postgres:5432/${DB_NAME:-commander}
      REDIS_URL: redis://redis:6379
      OLLAMA_HOST: http://ollama:11434
      OPENAI_API_KEY: ${OPENAI_API_KEY}
      DEBUG: ${DEBUG:-false}
      LOG_LEVEL: ${LOG_LEVEL:-INFO}
    ports:
      - "8000:8000"
    volumes:
      - ./src:/app/src
      - ./logs:/app/logs
    networks:
      - backend
    depends_on:
      postgres:
        condition: service_healthy
      redis:
        condition: service_healthy
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:8000/health"]
      interval: 30s
      timeout: 10s
      retries: 3
    restart: unless-stopped
    command: uvicorn main:app --host 0.0.0.0 --port 8000 --reload

  # React Frontend
  frontend:
    build:
      context: .
      dockerfile: Dockerfile.frontend
    container_name: commander-frontend
    ports:
      - "3000:3000"
    volumes:
      - ./ui:/app/ui
      - /app/ui/node_modules
    networks:
      - frontend
    depends_on:
      - backend
    environment:
      REACT_APP_API_URL: http://localhost:8000
    restart: unless-stopped
    command: npm start

  # Ollama Local LLM (Optional)
  ollama:
    image: ollama/ollama:latest
    container_name: commander-ollama
    ports:
      - "11434:11434"
    volumes:
      - ollama_data:/root/.ollama
    networks:
      - backend
    environment:
      OLLAMA_HOST: 0.0.0.0:11434
    restart: unless-stopped

volumes:
  postgres_data:
    driver: local
  redis_data:
    driver: local
  pgadmin_data:
    driver: local
  ollama_data:
    driver: local

networks:
  backend:
    driver: bridge
  frontend:
    driver: bridge
```

---

## DOCKERFILE SPECIFICATIONS

### Backend Dockerfile (Dockerfile.backend)

```dockerfile
FROM python:3.12-slim

WORKDIR /app

# Install system dependencies
RUN apt-get update && apt-get install -y \
    gcc \
    postgresql-client \
    curl \
    && rm -rf /var/lib/apt/lists/*

# Copy requirements
COPY requirements.txt .

# Install Python dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Copy source code
COPY src/ ./src/
COPY main.py .
COPY .env.example .env

# Expose port
EXPOSE 8000

# Health check
HEALTHCHECK --interval=30s --timeout=10s --start-period=5s --retries=3 \
    CMD curl -f http://localhost:8000/health || exit 1

# Run application
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```

### Frontend Dockerfile (Dockerfile.frontend)

```dockerfile
FROM node:20-alpine

WORKDIR /app/ui

# Copy package files
COPY package*.json ./

# Install dependencies
RUN npm ci

# Copy source code
COPY ui/ .

# Expose port
EXPOSE 3000

# Build for production (optional)
# RUN npm run build

# Health check
HEALTHCHECK --interval=30s --timeout=10s --retries=3 \
    CMD wget --quiet --tries=1 --spider http://localhost:3000 || exit 1

# Start development server
CMD ["npm", "start"]
```

---

## DOCKER NETWORKING

### Network Configuration

```
┌─────────────────────────────────────────────┐
│  Docker Network: bridge (default)           │
├─────────────────────────────────────────────┤
│  Backend Network (backend):                 │
│  ├─ postgres:5432                          │
│  ├─ redis:6379                             │
│  ├─ backend:8000                           │
│  ├─ pgadmin:5050                           │
│  └─ ollama:11434                           │
│                                             │
│  Frontend Network (frontend):               │
│  ├─ frontend:3000                          │
│  └─ backend (via docker bridge)            │
└─────────────────────────────────────────────┘
```

### Service Communication

| From | To | Protocol | Port | Purpose |
|------|-----|----------|------|---------|
| backend | postgres | TCP | 5432 | Database queries |
| backend | redis | TCP | 6379 | Cache operations |
| backend | ollama | HTTP | 11434 | LLM inference |
| frontend | backend | HTTP | 8000 | API requests |
| host | backend | HTTP | 8000 | Development access |
| host | frontend | HTTP | 3000 | Browser access |
| host | postgres | TCP | 5432 | Database access |
| host | pgadmin | HTTP | 5050 | Admin interface |

---

## VOLUMES & PERSISTENCE

### Volume Configuration

| Volume | Mount Point | Purpose | Driver | Backup |
|--------|------------|---------|--------|--------|
| postgres_data | /var/lib/postgresql/data | Database files | local | Daily |
| redis_data | /data | Cache data | local | Daily |
| pgadmin_data | /var/lib/pgadmin | pgAdmin config | local | Weekly |
| ollama_data | /root/.ollama | LLM models | local | Weekly |

### Data Persistence Strategy

```bash
# Backup all volumes
docker-compose exec postgres pg_dump -U commander commander > backup.sql
docker volume inspect commander_postgres_data
docker volume inspect commander_redis_data

# Restore from backup
docker-compose exec -T postgres psql -U commander < backup.sql
```

---

## ENVIRONMENT VARIABLES

### .env.docker (Docker-specific)

```bash
# Database
DB_USER=commander
DB_PASSWORD=commander_secure_pass
DB_NAME=commander
DATABASE_URL=postgresql://commander:commander_secure_pass@postgres:5432/commander

# Redis
REDIS_URL=redis://redis:6379
REDIS_DB=0

# API Keys
OPENAI_API_KEY=your_openai_key_here
GEMINI_API_KEY=your_gemini_key_here
GROQ_API_KEY=your_groq_key_here

# LLM Settings
OLLAMA_HOST=http://ollama:11434
OLLAMA_MODEL=llama3:8b

# Search Keys
TAVILY_API_KEY=your_tavily_key_here
EXA_API_KEY=your_exa_key_here
BRAVE_SEARCH_KEY=your_brave_key_here

# Application Settings
DEBUG=false
LOG_LEVEL=INFO
ENVIRONMENT=development

# pgAdmin
PGADMIN_EMAIL=admin@commander.local
PGADMIN_PASSWORD=admin_secure_password
```

---

## DOCKER COMMANDS

### Common Operations

```bash
# Start all services
docker-compose up -d

# Start specific service
docker-compose up -d postgres

# Stop all services
docker-compose down

# View logs
docker-compose logs -f

# View specific service logs
docker-compose logs -f backend

# Execute command in container
docker-compose exec backend bash
docker-compose exec postgres psql -U commander

# Rebuild images
docker-compose build

# Remove all containers and volumes
docker-compose down -v

# Health check status
docker-compose ps
```

### Database Operations

```bash
# Access PostgreSQL
docker-compose exec postgres psql -U commander -d commander

# View database size
docker-compose exec postgres psql -U commander -d commander -c "SELECT pg_size_pretty(pg_database_size('commander'));"

# Backup database
docker-compose exec postgres pg_dump -U commander commander > backup.sql

# Restore database
docker-compose exec -T postgres psql -U commander < backup.sql
```

### Monitoring

```bash
# Container status
docker-compose ps

# Resource usage
docker stats

# Network inspection
docker network ls
docker network inspect commander-and-agent_backend

# Volume inspection
docker volume ls
docker volume inspect commander-and-agent_postgres_data
```

---

## DEVELOPMENT WORKFLOW

### Initial Setup

```bash
# 1. Clone repository
git clone <repo-url>
cd commander-and-agent

# 2. Create .env file
cp .env.example .env.docker

# 3. Start Docker services
docker-compose up -d

# 4. Wait for health checks
docker-compose ps  # All should show "Up"

# 5. Initialize database
docker-compose exec backend python -m alembic upgrade head

# 6. Access services
# Backend: http://localhost:8000
# Frontend: http://localhost:3000
# pgAdmin: http://localhost:5050
```

### During Development

```bash
# Rebuild after code changes
docker-compose build backend

# Restart service
docker-compose restart backend

# View live logs
docker-compose logs -f backend

# Run migrations
docker-compose exec backend python -m alembic upgrade head

# Run tests
docker-compose exec backend pytest
```

### Cleanup

```bash
# Remove containers
docker-compose down

# Remove volumes too
docker-compose down -v

# Remove dangling images
docker image prune -f

# Complete cleanup
docker-compose down -v
docker image prune -af
```

---

## PRODUCTION DEPLOYMENT

### Pre-Production Checklist

- [ ] All environment variables configured
- [ ] Database backups automated
- [ ] Logging configured and centralized
- [ ] Health checks verified for all services
- [ ] Network security reviewed
- [ ] Volume backup strategy in place
- [ ] Container image security scan completed
- [ ] Load testing performed

### Production docker-compose Differences

```yaml
# Changes from development:
# 1. Remove auto-reload
# 2. Set DEBUG=false
# 3. Use persistent volumes with backups
# 4. Configure logging
# 5. Set restart policies to always
# 6. Configure resource limits
```

---

## TROUBLESHOOTING

### Container Issues

| Issue | Symptom | Solution |
|-------|---------|----------|
| Container won't start | `Error response from daemon` | Check logs: `docker-compose logs servicename` |
| Port already in use | `Bind for 0.0.0.0:5432 failed` | Change port or kill process using port |
| Network issues | `Cannot connect to Docker daemon` | Start Docker Desktop or systemctl start docker |
| Out of disk space | Docker operations fail | `docker system prune` and check disk space |

### Database Issues

| Issue | Symptom | Solution |
|-------|---------|----------|
| Connection refused | Cannot connect to postgres | Ensure postgres container healthy, wait 30s |
| Data lost on restart | Data not persisting | Check volume mounts in docker-compose |
| Slow queries | Performance issues | Check indexes, run `ANALYZE` |

---

## ÆNDRINGSLOG

| Dato | Tid | Handling | Af |
|------|-----|----------|-----|
| 2026-01-01 | 23:50 | 15_DOCKER_CONFIGURATION.md oprettet | Kv1nt |

---

**Document Status:** READY FOR IMPLEMENTATION
**Authority:** Development Team
**Review Date:** Pre-Phase 1 Docker Deployment
