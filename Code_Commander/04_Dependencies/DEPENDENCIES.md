# DEPENDENCIES: CODE COMMANDER

**Commander ID:** M-003
**Division:** Mobile
**Dependency Count:** 10

---

## RUNTIME DEPENDENCIES

### LLM Providers
| Package | Version | Purpose | Status |
|---------|---------|---------|--------|
| @google/generative-ai | ^0.15.0 | Gemini API | NOT INSTALLED |
| ollama-js | ^0.5.0 | Local LLM | NOT INSTALLED |

### Code Analysis
| Package | Version | Purpose | Status |
|---------|---------|---------|--------|
| typescript | ^5.0.0 | TS compilation | AVAILABLE |
| @typescript-eslint/parser | ^6.0.0 | TS parsing | AVAILABLE |
| eslint | ^8.0.0 | Linting | AVAILABLE |

### Utilities
| Package | Version | Purpose | Status |
|---------|---------|---------|--------|
| prettier | ^3.0.0 | Code formatting | AVAILABLE |

---

## PEER DEPENDENCIES

| Commander | Purpose | Status |
|-----------|---------|--------|
| Chat (M-001) | Task delegation | AVAILABLE |
| Terminal (M-002) | Deployment | PLACEHOLDER |
| Data (M-004) | Metrics | PLACEHOLDER |
| Evolution (M-005) | Learning | PLACEHOLDER |

---

## INSTALLATION

```bash
# Required for implementation
npm install @google/generative-ai
npm install ollama-js

# Already available
# typescript, eslint, prettier (existing)
```

---

**Sidst opdateret:** 2025-12-24
