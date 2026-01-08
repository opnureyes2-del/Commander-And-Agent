# FORBINDELSER: TERMINAL COMMANDER

**Commander ID:** M-002
**Division:** Mobile
**Forbindelser:** 5

---

## FORBINDELSESOVERSIGT

```
                    ┌─────────────────┐
                    │ Chat Commander  │
                    │     (M-001)     │
                    └────────┬────────┘
                             │ Intent: execute_command
                             │ Intent: system_check
                             ▼
┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐
│ Code Commander  │◄─│    TERMINAL     │─►│ Evolution       │
│    (M-003)      │  │   COMMANDER     │  │ Commander       │
│                 │  │    (M-002)      │  │ (M-005)         │
└─────────────────┘  └────────┬────────┘  └─────────────────┘
                             │
                             ▼
                    ┌─────────────────┐
                    │ Data Commander  │
                    │    (M-004)      │
                    └─────────────────┘
```

---

## DETALJEREDE FORBINDELSER

### 1. Chat Commander (M-001) → Terminal Commander
| Aspekt | Detalje |
|--------|---------|
| Type | UPSTREAM (modtager opgaver) |
| Protokol | Internal method call |
| Trigger | Intent: 'execute_command', 'system_check' |
| Data Flow | Command string → Terminal → Result |

**Eksempel Flow:**
```typescript
// User says: "Check server status"
// Chat analyzes intent: 'system_check'
// Chat calls Terminal:
const status = await terminalAgent.checkServerStatus();
// Terminal returns: "Server status: Online"
```

---

### 2. Terminal Commander → Code Commander (M-003)
| Aspekt | Detalje |
|--------|---------|
| Type | PEER (samarbejdspartner) |
| Protokol | Internal method call |
| Trigger | Deployment request, build command |
| Data Flow | Build/deploy result → Code validation |

**Use Case:**
- Code Commander genererer kode
- Terminal Commander deployer til server
- Terminal returnerer deployment status

---

### 3. Terminal Commander → Data Commander (M-004)
| Aspekt | Detalje |
|--------|---------|
| Type | PEER (data provider) |
| Protokol | Metrics push |
| Trigger | Health check, resource monitoring |
| Data Flow | Server metrics → Data processing |

**Eksempel Output:**
```typescript
{
  cpu: 45.2,
  memory: { used: 8192, total: 16384 },
  disk: [{ mount: "/", used: 65, total: 100 }],
  network: { rx: 1024000, tx: 512000 }
}
```

---

### 4. Terminal Commander → Evolution Commander (M-005)
| Aspekt | Detalje |
|--------|---------|
| Type | DOWNSTREAM (rapporterer til) |
| Protokol | Performance metrics |
| Trigger | Command completion, error occurrence |
| Data Flow | Execution metrics → Learning |

**Metrics:**
- Command execution time
- Success/failure rates
- Error types og frequency
- Resource consumption patterns

---

### 5. External: SSH Server Connection
| Aspekt | Detalje |
|--------|---------|
| Type | EXTERNAL (remote system) |
| Protokol | SSH (port 22) |
| Authentication | Key-based or password |
| Security | TLS encryption, known_hosts |

**Connection Flow:**
```
Mobile App → WebSocket → Backend → SSH → Remote Server
```

---

## KOMMUNIKATIONSTYPER

| Fra | Til | Type | Frekvens |
|-----|-----|------|----------|
| Chat | Terminal | Request/Response | Per command |
| Terminal | Data | Push | Periodic (5s) |
| Terminal | Evolution | Push | Per operation |
| Terminal | Code | Bidirectional | On demand |

---

## FEJLHÅNDTERING

### Forbindelsesfejl
```typescript
interface ConnectionError {
  type: 'timeout' | 'auth_failed' | 'host_unreachable';
  server: string;
  message: string;
  retryable: boolean;
}
```

### Fallback Strategy
1. Retry med exponential backoff
2. Alternative server hvis konfigureret
3. Cache last known status
4. Notify user via Chat Commander

---

## FREMTIDIGE FORBINDELSER

| Forbindelse | Status | Beskrivelse |
|-------------|--------|-------------|
| Docker Agent | Planlagt | Container management |
| CI/CD Pipeline | Planlagt | Build automation |
| Log Aggregator | Planlagt | Centralized logging |
| Backup Agent | Planlagt | Backup verification |

---

**Sidst opdateret:** 2025-12-24
**Total Forbindelser:** 5 (4 internal, 1 external)
