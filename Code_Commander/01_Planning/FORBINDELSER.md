# FORBINDELSER: CODE COMMANDER

**Commander ID:** M-003
**Division:** Mobile
**Forbindelser:** 6

---

## FORBINDELSESOVERSIGT

```
                    ┌─────────────────┐
                    │ Chat Commander  │
                    │     (M-001)     │
                    └────────┬────────┘
                             │ Intent: code_task
                             ▼
┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐
│ Terminal        │◄─│      CODE       │─►│ Evolution       │
│ Commander       │  │   COMMANDER     │  │ Commander       │
│ (M-002)         │  │    (M-003)      │  │ (M-005)         │
└─────────────────┘  └────────┬────────┘  └─────────────────┘
        ▲                     │                    │
        │                     ▼                    │
        │            ┌─────────────────┐           │
        │            │ Data Commander  │           │
        │            │    (M-004)      │           │
        │            └─────────────────┘           │
        │                                          │
        └──────────────────────────────────────────┘
                  Deployment & Learning Loop
```

---

## DETALJEREDE FORBINDELSER

### 1. Chat Commander (M-001) → Code Commander
| Aspekt | Detalje |
|--------|---------|
| Type | UPSTREAM (modtager opgaver) |
| Protokol | Internal method call |
| Trigger | Intent: 'code_task' |
| Data Flow | Code prompt → Code → Generated result |

**Trigger Keywords:**
- "write", "code", "function", "class"
- "fix", "debug", "solve"
- "refactor", "improve", "optimize"
- "explain", "what does this do"

---

### 2. Code Commander → Terminal Commander (M-002)
| Aspekt | Detalje |
|--------|---------|
| Type | PEER (samarbejdspartner) |
| Protokol | Internal method call |
| Trigger | Deployment, build, test execution |
| Data Flow | Generated code → Deploy → Result |

**Use Cases:**
```typescript
// After generating code:
await terminal.executeCommand({
  command: 'npm run build',
  workingDirectory: projectPath
});

// Run tests:
await terminal.executeCommand({
  command: 'npm test',
  workingDirectory: projectPath
});
```

---

### 3. Code Commander → Data Commander (M-004)
| Aspekt | Detalje |
|--------|---------|
| Type | PEER (metrics consumer) |
| Protokol | Metrics push |
| Trigger | Code analysis complete, generation metrics |
| Data Flow | Code metrics → Data analysis |

**Metrics Pushed:**
```typescript
{
  linesGenerated: number;
  complexity: number;
  testCoverage: number;
  analysisTime: number;
  languageDistribution: Record<string, number>;
}
```

---

### 4. Code Commander → Evolution Commander (M-005)
| Aspekt | Detalje |
|--------|---------|
| Type | DOWNSTREAM (rapporterer til) |
| Protokol | Learning feedback |
| Trigger | Code accepted/rejected, user edits |
| Data Flow | Generation outcomes → Learning |

**Feedback Types:**
- Code accepted without changes
- Code modified by user
- Code rejected
- Error occurred during execution

---

### 5. External: LLM Provider
| Aspekt | Detalje |
|--------|---------|
| Type | EXTERNAL (AI service) |
| Options | Gemini Nano (local), Gemini Flash (cloud), Ollama |
| Protocol | API call |
| Fallback | Local model if cloud unavailable |

**Provider Selection:**
```typescript
interface LLMConfig {
  primary: 'gemini_nano' | 'gemini_flash' | 'ollama';
  fallback: 'gemini_nano' | 'ollama';
  timeout: number;
  maxTokens: number;
}
```

---

### 6. External: Project Filesystem
| Aspekt | Detalje |
|--------|---------|
| Type | EXTERNAL (local storage) |
| Access | Read project files for context |
| Protocol | File system API |
| Security | Sandboxed to project directory |

**File Operations:**
- Read existing code for context
- Read package.json for dependencies
- Read tsconfig/eslint for style
- Write generated code (with user approval)

---

## KOMMUNIKATIONSFLOW

```
User: "Write a function to validate email"
          │
          ▼
   ┌──────────────┐
   │ Chat (M-001) │ Analyzes intent: code_task
   └──────┬───────┘
          │
          ▼
   ┌──────────────┐
   │ Code (M-003) │ ←──── LLM Provider
   │              │ ←──── Project Context
   └──────┬───────┘
          │
          ├─────────────► Data (M-004): Log metrics
          │
          ├─────────────► Evolution (M-005): Track outcome
          │
          ▼
   ┌──────────────┐
   │ Chat (M-001) │ Formats response
   └──────┬───────┘
          │
          ▼
      User sees:
   "function validateEmail(email: string): boolean { ... }"
```

---

## ERROR HANDLING

| Error Type | Handler | Fallback |
|------------|---------|----------|
| LLM Timeout | Retry 3x | Use local model |
| Invalid Prompt | Ask clarification | Return generic template |
| Parse Error | Log to Evolution | Return raw response |
| Context Missing | Warn user | Generate without context |

---

## FREMTIDIGE FORBINDELSER

| Forbindelse | Status | Beskrivelse |
|-------------|--------|-------------|
| Git Agent | Planlagt | Version control operations |
| Test Agent | Planlagt | Automatic test generation |
| Doc Agent | Planlagt | Documentation generation |
| Review Agent | Planlagt | Code review automation |

---

**Sidst opdateret:** 2025-12-24
**Total Forbindelser:** 6 (4 internal, 2 external)
