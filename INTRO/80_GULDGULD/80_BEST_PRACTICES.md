# BEST PRACTICES - Commander-and-Agent

**Project:** Commander-and-Agent (Documentation Framework)
**Version:** 2.0.0
**Status:** DOKUMENTATION 100% KOMPLET
**Last Updated:** 2025-12-29
**Created:** 2025-12-24

---

## OVERVIEW

This document establishes best practices for developing, testing, documenting, and deploying the Commander-and-Agent system.

---

## CODE QUALITY STANDARDS

### Python Code Quality

#### Style Guide

**Follow PEP 8 with these standards:**

```python
# Good: Clear naming and formatting
def process_commander_request(commander_id: str, request_data: dict) -> dict:
    """
    Process incoming request for a specific Commander.

    Args:
        commander_id: Unique identifier of the Commander
        request_data: Request payload containing instructions

    Returns:
        Response dictionary with results and metadata
    """
    if not commander_id:
        raise ValueError("commander_id cannot be empty")

    result = {
        "commander_id": commander_id,
        "status": "processed",
        "data": request_data
    }
    return result

# Bad: Unclear naming and poor formatting
def pc(c, d):
    if not c: return None
    return {"id": c, "status": "ok", "data": d}
```

#### Type Hints (Required)

```python
# Always use type hints
from typing import Dict, List, Optional

def fetch_commander_status(
    commander_id: str,
    include_metrics: bool = False
) -> Dict[str, Any]:
    """Fetch current status of a Commander."""
    pass

async def orchestrate_research(
    query: str,
    commanders: List[str]
) -> Optional[Dict]:
    """Orchestrate research across multiple Commanders."""
    pass
```

#### Docstrings (Required)

```python
def analyze_query(
    query: str,
    depth_level: int = 1,
) -> Dict[str, List[str]]:
    """
    Analyze user query for intent and entities.

    This method breaks down the query into components,
    identifies the primary intent, and extracts entities
    for further processing by specialized Commanders.

    Args:
        query: The user's input query string
        depth_level: Analysis depth (1-3, default 1)

    Returns:
        Dictionary containing:
        - intent: Primary user intent
        - entities: Extracted entities
        - keywords: Key terms for search

    Raises:
        ValueError: If query is empty
        RuntimeError: If analysis service unavailable

    Example:
        >>> result = analyze_query("What are latest AI trends?")
        >>> result["intent"]
        'research'
    """
    pass
```

#### Logging Standards

```python
import logging

logger = logging.getLogger(__name__)

def process_commander_task(task_id: str):
    """Process a task assigned to a Commander."""
    logger.debug(f"Starting task processing: {task_id}")

    try:
        logger.info(f"Processing task {task_id} for Commander")
        # ... processing logic
        logger.info(f"Task {task_id} completed successfully")
    except Exception as e:
        logger.error(f"Error processing task {task_id}: {str(e)}", exc_info=True)
        raise
```

### JavaScript/TypeScript Code Quality

#### Type Safety (Required)

```typescript
// Good: Full type coverage
interface CommanderRequest {
  commanderId: string;
  action: 'execute' | 'query' | 'learn';
  payload: Record<string, unknown>;
  timeout?: number;
}

async function executeCommand(
  request: CommanderRequest
): Promise<CommanderResponse> {
  if (!request.commanderId) {
    throw new Error('commanderId is required');
  }
  // Implementation
}

// Bad: Using any type
function executeCommand(request: any): Promise<any> {
  // Implementation
}
```

#### Component Structure

```typescript
// Good: Well-organized component
interface CommanderProps {
  id: string;
  name: string;
  status: 'idle' | 'active' | 'error';
  onExecute: (id: string) => Promise<void>;
}

export const CommanderComponent: React.FC<CommanderProps> = ({
  id,
  name,
  status,
  onExecute,
}) => {
  const handleClick = async () => {
    await onExecute(id);
  };

  return (
    <div className="commander" data-testid={`commander-${id}`}>
      <h3>{name}</h3>
      <p>Status: {status}</p>
      <button onClick={handleClick}>Execute</button>
    </div>
  );
};
```

---

## TESTING STANDARDS

### Unit Testing

#### Python Unit Tests

```python
import pytest
from src.commanders.chat import ChatCommander

class TestChatCommander:
    """Test suite for Chat Commander."""

    @pytest.fixture
    def chat_commander(self):
        """Create a Chat Commander instance for testing."""
        return ChatCommander(config={
            "model": "test-model",
            "debug": True
        })

    def test_process_input(self, chat_commander):
        """Test input processing."""
        result = chat_commander.process_input("Hello")
        assert result is not None
        assert "response" in result

    def test_empty_input(self, chat_commander):
        """Test handling of empty input."""
        with pytest.raises(ValueError):
            chat_commander.process_input("")

    def test_large_input(self, chat_commander):
        """Test handling of large input."""
        large_text = "test " * 10000
        result = chat_commander.process_input(large_text)
        assert result is not None
```

#### JavaScript/TypeScript Tests

```typescript
import { render, screen, waitFor } from '@testing-library/react';
import { CommanderComponent } from './CommanderComponent';

describe('CommanderComponent', () => {
  it('should render commander name', () => {
    render(
      <CommanderComponent
        id="m-001"
        name="Chat Commander"
        status="idle"
        onExecute={jest.fn()}
      />
    );

    expect(screen.getByText('Chat Commander')).toBeInTheDocument();
  });

  it('should call onExecute when button clicked', async () => {
    const mockExecute = jest.fn();
    render(
      <CommanderComponent
        id="m-001"
        name="Chat Commander"
        status="idle"
        onExecute={mockExecute}
      />
    );

    screen.getByText('Execute').click();

    await waitFor(() => {
      expect(mockExecute).toHaveBeenCalledWith('m-001');
    });
  });
});
```

### Integration Testing

```python
import pytest
from src.orchestration import Commander Orchestrator

class TestCommanderIntegration:
    """Integration tests for Commander orchestration."""

    @pytest.fixture
    async def orchestrator(self):
        """Setup orchestrator with all P1 Commanders."""
        orch = CommanderOrchestrator()
        await orch.initialize_commanders(['M-001', 'H-001', 'R-001'])
        yield orch
        await orch.shutdown()

    @pytest.mark.asyncio
    async def test_chat_to_research_flow(self, orchestrator):
        """Test flow from Chat Commander to Research Commander."""
        # Chat processes user input
        chat_result = await orchestrator.execute('M-001', {
            'action': 'process',
            'input': 'Research AI trends'
        })

        # Research Commander receives and processes
        research_result = await orchestrator.execute('R-001', {
            'action': 'execute',
            'task_id': chat_result['task_id']
        })

        assert research_result['status'] == 'completed'
```

### Test Coverage Standards

| Metric | Minimum | Target | Phase |
|--------|---------|--------|-------|
| Unit Test Coverage | 70% | 85% | P1 |
| Integration Coverage | 60% | 75% | P1 |
| Critical Path Coverage | 90% | 95% | P1 |

---

## DOCUMENTATION STANDARDS

### Code Documentation

**Every function must have:**

1. **Docstring** - Purpose, arguments, return value
2. **Type Hints** - Parameter and return types
3. **Examples** - Usage examples where appropriate
4. **Error Handling** - Possible exceptions

**Example:**

```python
async def query_commander(
    commander_id: str,
    query: str,
    context: Optional[Dict] = None,
) -> CommanderResponse:
    """
    Send query to a specific Commander for processing.

    This method routes queries to the appropriate Commander
    based on type and context, handling authentication,
    rate limiting, and response validation.

    Args:
        commander_id: ID of the Commander (e.g., 'M-001')
        query: The query string to process
        context: Optional context dictionary for the query

    Returns:
        CommanderResponse with:
        - status: 'success' or 'error'
        - result: Processing result or error message
        - metadata: Timing and resource usage info

    Raises:
        ValueError: If commander_id invalid or query empty
        TimeoutError: If processing exceeds timeout
        AuthenticationError: If authentication fails

    Example:
        >>> response = await query_commander(
        ...     'R-001',
        ...     'What are latest AI developments?',
        ...     {'language': 'en', 'depth': 'detailed'}
        ... )
        >>> print(response.result)
    """
    if not commander_id:
        raise ValueError("commander_id required")
    if not query:
        raise ValueError("query cannot be empty")

    # Implementation
    pass
```

### Commit Messages

**Format: Conventional Commits**

```
<type>(<scope>): <subject>

<body>

<footer>
```

**Examples:**

```
feat(chat-commander): implement intent detection

Add intent detection capability to Chat Commander using
Socratic questioning framework. Supports 15+ intent types
with 92% accuracy on test set.

Closes #123

feat(research): add tavily search integration
fix(deployment): resolve Docker compose port conflict
docs(architecture): update system overview
test(commands): add integration test for chat flow
chore: update dependencies to latest versions
```

### INTRO DNA Documentation Standards

**Each INTRO file must include:**

1. Header with metadata
2. Overview section
3. Main content with clear hierarchy
4. Tables/diagrams where helpful
5. Verification checklist (if applicable)
6. ÆNDRINGSLOG with entries

---

## GIT WORKFLOW STANDARDS

### Branch Naming Convention

```
<type>/<feature-name>

Types:
  feat/     - New feature
  fix/      - Bug fix
  refactor/ - Code refactoring
  docs/     - Documentation
  test/     - Testing
  chore/    - Maintenance

Examples:
  feat/chat-commander-implementation
  fix/database-connection-timeout
  docs/api-specification
  test/integration-tests-p1
```

### Pull Request Standards

**Every PR must have:**

1. **Descriptive Title** - What changed and why
2. **Detailed Description** - Problem, solution, testing
3. **Testing Evidence** - Test results, coverage
4. **Documentation** - Updated docs, CHANGELOG
5. **Code Review** - 2+ approvals before merge

**Template:**

```markdown
## Description
Brief explanation of changes

## Problem Solved
What issue or requirement this addresses

## Solution Implemented
How the problem was solved

## Testing
- [ ] Unit tests pass
- [ ] Integration tests pass
- [ ] Manual testing completed
- [ ] Coverage > 80%

## Documentation
- [ ] Code commented
- [ ] ÆNDRINGSLOG updated
- [ ] README updated if needed
- [ ] API docs updated if needed

## Checklist
- [ ] Code follows style guide
- [ ] No hardcoded secrets
- [ ] Performance reviewed
- [ ] Error handling complete
```

---

## PERFORMANCE STANDARDS

### Python Performance

```python
# Use context managers for resource cleanup
with database_connection() as db:
    result = db.query(...)

# Async for I/O operations
async def fetch_from_multiple_sources(urls: List[str]):
    tasks = [fetch_url(url) for url in urls]
    return await asyncio.gather(*tasks)

# Batch database operations
bulk_insert(records)  # Not: for r in records: insert(r)

# Cache expensive operations
@cache(ttl=3600)
def expensive_calculation():
    pass
```

### Response Time Targets

| Commander Priority | Target | P1 | P2 | P3 |
|-------------------|--------|----|----|-----|
| P1 | < 500ms | ✓ | - | - |
| P2 | < 2000ms | ✓ | ✓ | - |
| P3 | < 5000ms | ✓ | ✓ | ✓ |

---

## SECURITY STANDARDS

### Secrets Management

```python
# Bad: Hardcoded secrets
API_KEY = "sk-1234567890abcdef"
DATABASE_PASSWORD = "admin123"

# Good: Environment variables
import os
API_KEY = os.getenv("OPENAI_API_KEY")
DATABASE_PASSWORD = os.getenv("DB_PASSWORD")

# Better: Secret manager
from secret_manager import get_secret
API_KEY = get_secret("openai-api-key")
```

### Input Validation

```python
from pydantic import BaseModel, Field, validator

class CommanderQuery(BaseModel):
    """Validated command request."""

    commander_id: str = Field(..., min_length=3, max_length=10)
    query: str = Field(..., min_length=1, max_length=10000)
    timeout: int = Field(default=30, gt=0, le=300)

    @validator('commander_id')
    def validate_commander_id(cls, v):
        """Validate commander ID format."""
        if not v.matches(r'^[MHS]-\d{3}$'):
            raise ValueError('Invalid commander ID format')
        return v
```

### Authentication & Authorization

```python
# Require authentication for protected endpoints
@app.post("/api/commanders/{commander_id}/execute")
async def execute_command(
    commander_id: str,
    request: CommandRequest,
    current_user: User = Depends(get_current_user),
):
    """Execute command with user authentication."""
    # Check user permission
    if not current_user.can_execute(commander_id):
        raise PermissionError(f"User cannot execute {commander_id}")

    # Process command
    result = await commander.execute(request)
    return result
```

---

## ERROR HANDLING STANDARDS

### Exception Handling

```python
# Good: Specific exceptions
try:
    result = await commander.execute(request)
except CommanderNotFound as e:
    logger.warning(f"Commander not found: {e}")
    raise HTTPException(status_code=404, detail=str(e))
except CommandTimeoutError as e:
    logger.error(f"Command timeout: {e}")
    raise HTTPException(status_code=504, detail="Processing timeout")
except Exception as e:
    logger.critical(f"Unexpected error: {e}", exc_info=True)
    raise HTTPException(status_code=500, detail="Internal server error")

# Bad: Catching all exceptions
try:
    result = await commander.execute(request)
except Exception as e:
    return {"error": str(e)}
```

---

## DEPLOYMENT STANDARDS

### Pre-Deployment Checklist

- [ ] All tests pass (> 80% coverage)
- [ ] Code review approved (2+ reviewers)
- [ ] Documentation updated
- [ ] No hardcoded secrets or credentials
- [ ] Performance benchmarks met
- [ ] Security audit passed
- [ ] Database migrations prepared
- [ ] Rollback plan documented

### Production Configuration

```yaml
# Only production-safe settings
DEBUG: false
LOG_LEVEL: WARNING  # Not DEBUG
WORKERS: 8          # Based on CPU cores
DATABASE_POOL_SIZE: 20
CACHE_ENABLED: true
SECURITY_HEADERS: true
```

---

## MONITORING & LOGGING

### Logging Levels

| Level | When to Use | Example |
|-------|-----------|---------|
| DEBUG | Development troubleshooting | Detailed state tracking |
| INFO | General progress | Command execution start/finish |
| WARNING | Recoverable issues | Retry attempt, slow response |
| ERROR | Significant problems | Failed request, timeout |
| CRITICAL | System failure | Database unavailable |

### Metrics to Track

```python
# Response time per Commander
metrics.histogram('commander.response_time', response_ms, {'commander_id': id})

# Error rate
metrics.counter('commander.errors', {'commander_id': id, 'error_type': type})

# Cache hit rate
metrics.gauge('cache.hit_rate', hit_rate * 100, {'cache_name': name})

# Queue depth
metrics.gauge('queue.depth', queue_size, {'queue_name': name})
```

---

## ÆNDRINGSLOG

| Dato | Tid | Handling | Af |
|------|-----|----------|-----|
| 2026-01-01 | 23:50 | 80_BEST_PRACTICES.md oprettet | Kv1nt |

---

**Document Status:** REFERENCE STANDARD
**Authority:** Development Team
**Compliance:** Required for all code contributions
**Review:** Quarterly
