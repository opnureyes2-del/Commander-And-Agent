# FORBINDELSER - CHAT COMMANDER

## Connection Diagram

```
                         ┌─────────────────┐
                         │      USER       │
                         └────────┬────────┘
                                  │
                                  ▼
                    ┌─────────────────────────┐
                    │    CHAT COMMANDER       │
                    │   (Cirkel / Kv1nt)      │
                    │        M-001            │
                    └─────────────┬───────────┘
                                  │
        ┌─────────────┬───────────┼───────────┬─────────────┐
        │             │           │           │             │
        ▼             ▼           ▼           ▼             ▼
   ┌─────────┐  ┌─────────┐  ┌─────────┐  ┌─────────┐  ┌─────────┐
   │TERMINAL │  │  CODE   │  │  DATA   │  │EVOLUTION│  │ RESEARCH│
   │  M-002  │  │  M-003  │  │  M-004  │  │  M-005  │  │  R-001  │
   └─────────┘  └─────────┘  └─────────┘  └─────────┘  └─────────┘
        │
        └──────────────┬──────────────┐
                       │              │
                       ▼              ▼
                  ┌─────────┐   ┌─────────┐
                  │  FEIA   │   │   CSA   │
                  │  H-001  │   │  H-002  │
                  └─────────┘   └─────────┘
```

---

## DIRECT CONNECTIONS

### 1. Terminal Commander (M-002)
| Attribute | Value |
|-----------|-------|
| **Type** | Bidirectional |
| **Trigger** | User requests server/SSH control |
| **Priority** | P1 |
| **Latency Requirement** | < 1000ms |

**Data Flow:**
```
Chat → Terminal: { action: "status", target: "all" }
Terminal → Chat: { servers: [...], status: "healthy" }
```

### 2. Code Commander (M-003)
| Attribute | Value |
|-----------|-------|
| **Type** | Bidirectional |
| **Trigger** | User requests coding/debugging |
| **Priority** | P2 |
| **Latency Requirement** | < 2000ms |

**Data Flow:**
```
Chat → Code: { task: "generate", language: "python", spec: "..." }
Code → Chat: { code: "def sort_list...", explanation: "..." }
```

### 3. Data Commander (M-004)
| Attribute | Value |
|-----------|-------|
| **Type** | Bidirectional |
| **Trigger** | User requests data analysis |
| **Priority** | P2 |
| **Latency Requirement** | < 3000ms |

**Data Flow:**
```
Chat → Data: { action: "analyze", source: "file.csv" }
Data → Chat: { summary: {...}, charts: [...] }
```

### 4. Evolution Commander (M-005)
| Attribute | Value |
|-----------|-------|
| **Type** | Bidirectional (meta-level) |
| **Trigger** | System improvement, learning |
| **Priority** | P3 |
| **Latency Requirement** | Background |

**Function:** Monitors Chat's performance and suggests improvements.

### 5. Research Commander (R-001)
| Attribute | Value |
|-----------|-------|
| **Type** | Bidirectional |
| **Trigger** | User requests research/search |
| **Priority** | P2 |
| **Latency Requirement** | < 5000ms |

**Data Flow:**
```
Chat → Research: { query: "AI trends 2025", depth: "comprehensive" }
Research → Chat: { findings: [...], mdt_score: 0.85 }
```

### 6. FEIA Commander (H-001)
| Attribute | Value |
|-----------|-------|
| **Type** | Input-processing |
| **Trigger** | Voice input from user |
| **Priority** | P1 |
| **Latency Requirement** | < 200ms |

**Data Flow:**
```
FEIA → Chat: { transcription: "...", intent: "question", emotion: "neutral" }
```

### 7. CSA Commander (H-002)
| Attribute | Value |
|-----------|-------|
| **Type** | Output-processing |
| **Trigger** | Accessibility mode active |
| **Priority** | P3 |
| **Latency Requirement** | < 500ms |

**Data Flow:**
```
Chat → CSA: { text: "complex response" }
CSA → Chat: { simplified: "easy response" }
```

---

## COMMUNICATION PROTOCOL

### Request Format
```typescript
interface ChatRequest {
  source: "user" | "agent";
  type: "message" | "command" | "query";
  content: string;
  context?: {
    history: Message[];
    personality: "cirkel" | "kv1nt";
  };
  metadata?: {
    timestamp: number;
    session_id: string;
  };
}
```

### Response Format
```typescript
interface ChatResponse {
  target: "user" | "agent";
  type: "response" | "routing" | "status";
  content: string;
  routing?: {
    to_agent: string;
    action: string;
    payload: any;
  };
  metadata?: {
    processing_time: number;
    model_used: string;
  };
}
```

---

## ROUTING LOGIC

```typescript
function routeIntent(intent: string): string {
  const routing = {
    "server": "terminal_commander",      // M-002
    "ssh": "terminal_commander",         // M-002
    "command": "terminal_commander",     // M-002
    "code": "code_commander",            // M-003
    "program": "code_commander",         // M-003
    "debug": "code_commander",           // M-003
    "data": "data_commander",            // M-004
    "analyze": "data_commander",         // M-004
    "research": "research_commander",    // R-001
    "search": "research_commander",      // R-001
    "help": "self",
    "chat": "self"
  };

  return routing[intent] || "self";
}
```

---

## FLOW DIAGRAMS

### Standard Conversation Flow
```
┌────────┐    ┌──────┐    ┌─────────┐
│ User   │───▶│ Chat │───▶│   LLM   │
└────────┘    └──────┘    └─────────┘
     ▲            │             │
     │            │             │
     └────────────┴─────────────┘
```

### Multi-Agent Flow
```
┌────────┐    ┌──────┐    ┌──────────┐    ┌─────────┐
│ User   │───▶│ Chat │───▶│Specialist│───▶│ Result  │
└────────┘    └──────┘    └──────────┘    └─────────┘
     ▲            │                            │
     │            └────────────────────────────┘
     └─────────────────────────────────────────┘
```

### Voice Input Flow
```
┌────────┐    ┌──────┐    ┌──────┐    ┌─────┐
│ Voice  │───▶│ FEIA │───▶│ Chat │───▶│ LLM │
└────────┘    └──────┘    └──────┘    └─────┘
                  │                       │
                  └──(intent + emotion)───┘
```

---

## CONNECTION PRIORITY TABLE

| Connection | Priority | Latency Req | Critical? |
|------------|----------|-------------|-----------|
| User | P0 | < 500ms | YES |
| FEIA (H-001) | P1 | < 200ms | YES (voice) |
| Terminal (M-002) | P1 | < 1000ms | NO |
| Code (M-003) | P2 | < 2000ms | NO |
| Data (M-004) | P2 | < 3000ms | NO |
| Research (R-001) | P2 | < 5000ms | NO |
| Evolution (M-005) | P3 | Background | NO |
| CSA (H-002) | P3 | < 500ms | NO |

---

## DEPENDENCY REQUIREMENTS

| Dependency | Required For | Status |
|------------|--------------|--------|
| LLM Service | All operations | Required |
| AsyncStorage | History persistence | Required |
| Network | Cloud fallback | Optional |
| FEIA | Voice input | Optional |
| CSA | Accessibility | Optional |

---

**Document Status:** COMPLETE
**Last Updated:** 2025-12-24
**Author:** Claude Code
